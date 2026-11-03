# deep-research-poc handover archive

Two files, both direct downloads (any computer, no sign-in):

| File | Size | What it is |
|---|---|---|
| `deep-research-poc-handover-2026-11-03.zip` | 9.5 MB, 749 files | the project folder packed on 2026-11-03: code, tests, prompts, docs, golden sets and every eval result; no virtual environment, no document corpora, no API key |
| `deep-research-poc-cuad-dataset-2026-11-03.zip` | 8.3 MB, 1,025 files | the test dataset the evals ran on: the 510-contract CUAD corpus in the project's document format, plus the CUAD question files and the provenance note |

## 1. Project archive

    https://github.com/UtkarshRedd/deep-research-poc-handover/raw/main/deep-research-poc-handover-2026-11-03.zip

    SHA-256  23c50e544d22a7574eb00e457bfea0d748fe52928e44c4682682270d1e387b3d

First steps:

    unzip deep-research-poc-handover-2026-11-03.zip
    cd deep-research-poc
    python -m venv .venv
    .venv\Scripts\activate            (or: source .venv/bin/activate)
    pip install -e ".[dev,ingest]" -c requirements.lock
    python -m pytest -q               expect: 108 passed, 8 skipped

Then open the folder in Claude Code. `CLAUDE.md` points to `HANDOVER.md`; its section 7 covers the key,
the endpoint health check and rebuilding the CUAD corpus.

## 2. CUAD test dataset

    https://github.com/UtkarshRedd/deep-research-poc-handover/raw/main/deep-research-poc-cuad-dataset-2026-11-03.zip

    SHA-256  b770fe88ad2f878ad5cb610ecb1eb9db651771563fb2fab6edd82187d4814766

Unzip it **inside the `deep-research-poc` folder** produced by the archive above; its entries are rooted at
`data/`, so the files land where the code expects them:

    data/cuad_docs/<stem>.md          510 contracts, `<!-- page N -->` before every page, title + metadata table on page 1
    data/cuad_docs/<stem>.meta.json   510 sidecars: title, doc_type, date, parties, source PDF, page-alignment flags
    data/cuad_docs/_alignment.json    page start offsets in the original CUAD text (needed by tests and the golden generator)
    data/golden/cuad_questions.jsonl  112 questions derived from the expert clause annotations
    data/golden/cuad_smoke.jsonl      12 of them, 2 per question type (the quick eval)
    data/golden/README.md             golden-set format and scoring rules
    data/raw/cuad/SOURCE.md           provenance and licence of the source data

With the dataset in place `python -m pytest -q` reports 116 passed (the 8 CUAD tests no longer skip), and the
eval commands in `HANDOVER.md` section 7 run as written, for example:

    python -m dr.eval.run --dry --golden data/golden/cuad_questions.jsonl --docs data/cuad_docs

Keep the folder at `data/cuad_docs` and pass `--docs data/cuad_docs`: document ids hash the path string as
passed, so a different spelling (or a different OS) changes every id relative to the checked-in results.

The raw Zenodo download (510 PDFs and text files, 106 MB) is not included; `scripts/fetch_cuad.py` fetches and
md5-verifies it, and `scripts/make_cuad_corpus.py` regenerates this exact corpus from it in about two minutes.

**Attribution.** The corpus is a derived form (reflowed text with PDF page anchors) of CUAD v1, the Contract
Understanding Atticus Dataset, by The Atticus Project, licensed under CC BY 4.0
(https://creativecommons.org/licenses/by/4.0/). Source record: https://zenodo.org/records/4595826
(DOI 10.5281/zenodo.4595826). Hendrycks, Burns, Chen and Ball (2021), *CUAD: An Expert-Annotated NLP Dataset for
Legal Contract Review*, NeurIPS 2021 Datasets and Benchmarks, https://arxiv.org/abs/2103.06268.
The SOURCE.md inside the zip carries the same notice; keep it with any further redistribution.

## Verify a download

    certutil -hashfile <file>.zip SHA256       (Windows)
    shasum -a 256 <file>.zip                   (macOS, Linux)

Each zip has a `.sha256` file beside it in this repo with the expected value.
