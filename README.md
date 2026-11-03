# deep-research-poc handover archive

One file: `deep-research-poc-handover-2026-11-03.zip` (9.5 MB, 749 files, unpacks to a `deep-research-poc/` folder).
It is the project folder packed on 2026-11-03 for a new machine: code, tests, prompts, docs, golden sets and
every eval result. It contains no virtual environment, no document corpora and no API key.

## Download

Direct link (any computer, no sign-in):

    https://github.com/UtkarshRedd/deep-research-poc-handover/raw/main/deep-research-poc-handover-2026-11-03.zip

## Verify

    SHA-256  23c50e544d22a7574eb00e457bfea0d748fe52928e44c4682682270d1e387b3d

    certutil -hashfile deep-research-poc-handover-2026-11-03.zip SHA256   (Windows)
    shasum -a 256 deep-research-poc-handover-2026-11-03.zip              (macOS, Linux)

## First steps

    unzip deep-research-poc-handover-2026-11-03.zip
    cd deep-research-poc
    python -m venv .venv
    .venv\Scripts\activate            (or: source .venv/bin/activate)
    pip install -e ".[dev,ingest]" -c requirements.lock
    python -m pytest -q               expect: 108 passed, 8 skipped

Then open the folder in Claude Code. `CLAUDE.md` points to `HANDOVER.md`; its section 7 covers the key,
the endpoint health check and rebuilding the CUAD corpus.
