# Hotel booking: end of P06 (Python)

Install once (internet needed the first time):

    python -m venv .venv
    source .venv/bin/activate        # Windows: .venv\Scripts\activate
    python -m pip install import-linter
    # with uv:  uv add --dev import-linter

Run:

    python -m unittest -v            # 8 tests
    lint-imports                     # 3 contracts   (uv: uv run lint-imports)
    python main.py                   # demo
    python main.py --race            # race (still double-books: P09 fixes it)

docs/ holds requirements, quality scenarios, contexts, design notes, ADR-001 and diagrams.drawio.
