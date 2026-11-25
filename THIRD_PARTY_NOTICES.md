# Third-Party Notices

This project is licensed under **PolyForm Noncommercial 1.0.0** (see `LICENSE`). All third-party open-source packages and tools remain under their respective upstream licenses.

## Ships Inside the App (Runtime Dependencies — Planned)

| Component / Package | License | Upstream Link | Purpose |
| :--- | :--- | :--- | :--- |
| FastAPI | MIT | https://github.com/fastapi/fastapi | Loopback HTTP & REST server (`127.0.0.1:27130`) |
| Uvicorn | BSD-3-Clause | https://github.com/encode/uvicorn | ASGI server runtime |
| MCP Python SDK | MIT | https://github.com/modelcontextprotocol/python-sdk | Streamable HTTP `/mcp` server |
| Typer | MIT | https://github.com/fastapi/typer | `--json` CLI command interface |
| HTTPX | BSD-3-Clause | https://github.com/encode/httpx | Async client for Ollama, AnkiConnect, Obsidian Agent Manager |
| PyMuPDF (`pymupdf`) | AGPL-3.0 / Commercial *(evaluate `pypdfium2` Apache-2.0 before shipping)* | https://github.com/pymupdf/PyMuPDF | PDF page rendering to images for multimodal embeddings |
| Groq Python SDK | Apache-2.0 | https://github.com/groq/groq-python | Groq Whisper v3 (`whisper-large-v3`) audio transcription |
| NumPy | BSD-3-Clause | https://github.com/numpy/numpy | Vector normalization & similarity scoring |

*(Note on PDF rendering: prefer **Apache-2.0 `pypdfium2` + Pillow (HPND)** over AGPL PyMuPDF so licensing stays clean under PolyForm Noncommercial 1.0.0.)*

## Build / Test Only — Not Shipped

| Component / Package | License | Upstream Link | Purpose |
| :--- | :--- | :--- | :--- |
| `uv` | MIT / Apache-2.0 | https://github.com/astral-sh/uv | Python environment and lockfile manager |
| `ruff` | MIT | https://github.com/astral-sh/ruff | Linter and formatter |
| `mypy` | MIT | https://github.com/python/mypy | Static type checking |
| `pytest` | MIT | https://github.com/pytest-dev/pytest | Unit and integration testing |
