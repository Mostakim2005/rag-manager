# Project Operating Instructions (`AGENTS.md`)
> **Notice:** This project inherits user persona, confirmation gates, OS-default vs. Shared Libraries/Ecosystem Registry rules, GitHub-first CI/CD defaults, on-demand `AUDITS.md`/`UIUX.md` and Downloads-folder disposable report routing, pre-commit doc sync, visual standards, and documentation blueprints from the global master `AGENTS.md`. Do not duplicate those rules here.

## 1. Repository Classification & Tech Stack
* **Verification Mode:** `[GitHub-First (Remote Target: https://github.com/Mostakim2005/rag-manager)]`
* **Runtime / Language:** Python 3.12+ managed via `uv`
* **Framework / Host:** FastAPI + Uvicorn (loopback HTTP + Streamable `/mcp`), Typer (`--json` CLI), SQLite WAL (FTS5 + vector cosine / RRF)
* **Target OS / Output:** Windows x64 (`uv run rag-manager`, registered in `%USERPROFILE%\.local\bin` and `ecosystem-registry.json`)
* **Shared Libraries & Local Ports:**
  * Canonical Contract: `C:\Users\kazir\Documents\Agentic Harness\Index & Flowcharts\ECOSYSTEM_INTEGRATION_CONTRACT.md` (v2.0.0)
  * Registry: `%USERPROFILE%\.local\share\ecosystem-registry.json` (`LOCAL_ECOSYSTEM_REGISTRY`)
  * Secrets: `%USERPROFILE%\.local\share\ecosystem-secrets.json` (`LOCAL_ECOSYSTEM_SECRETS`)
  * Service Port: `127.0.0.1:27130` (`http://127.0.0.1:27130`, MCP `http://127.0.0.1:27130/mcp`)
  * Upstream Dependencies: Ollama `127.0.0.1:11434` (`embeddinggemma-2:440m` multimodal image+text primary; `270m` fallback), Obsidian Agent Manager `127.0.0.1:27125`, Obsidian Local REST API `https://127.0.0.1:27124`, AnkiConnect `127.0.0.1:8765`, DeepTutor `127.0.0.1:8001`, CLIProxyAPI `127.0.0.1:8317`, `ffmpeg.exe` / `ffprobe.exe` in `%USERPROFILE%\.local\bin`, Groq Whisper v3 (`GROQ_API_KEY`)

## 2. Verification & Build Commands (Mirrored in CI)
*(GitHub Actions `.github/workflows/ci.yml` is the authoritative CI gate; lightweight local checks run with `uv` once source code is added.)*
* **Format & Lint:** `uv run ruff check . && uv run ruff format --check .`
* **Static Type Check:** `uv run mypy src tests`
* **Unit & Integration Tests:** `uv run pytest -q`
* **Security & Secret Scan:** `uv run pip-audit`
* **Smoke Test:** `uv run rag-manager health --json`
* **Structure & Doc Checks:** Validate `docs/*.md` search-first `### [TYPE: Slug]` headers and UTF-8 without BOM JSON parity

## 3. Repository Directory Structure & Conventions
* `.github/workflows/` - GitHub Actions CI (`ci.yml` with patch-ready failure report artifact).
* `src/rag_manager/` - Modular Python source (planned: `config.py`, `db.py`, `embedder.py`, `transcribe.py`, `xref.py`, `ingest/`, `connectors/`, `server.py`, `mcp_server.py`, `cli.py`).
* `tests/` - Pytest unit and integration tests with offline fixture mocks for Ollama/Groq/AnkiConnect.
* `docs/` - Core Markdown documentation suite (`PRODUCT.md`, `ARCHITECTURE.md`, `DECISIONS_AND_TRAPS.md`). Scan with `grep -A 1 "^### \[" docs/`.
* `preview/` - On-demand interactive HTML/CSS/JS visual previews only when explicitly requested.

## 4. Critical Trap Pointers (1-Line Warnings Only)
* **TRAP 1:** Never bind the HTTP/MCP server to `0.0.0.0` or allow browser `Origin` headers — see `docs/DECISIONS_AND_TRAPS.md` -> `[TRAP: Loopback-Binding-And-Origin-Guard]`.
* **TRAP 2:** Never open a second writer to the SQLite index DB or omit WAL/busy_timeout pragmas — see `docs/DECISIONS_AND_TRAPS.md` -> `[TRAP: SQLite-WAL-Single-Writer-Only]`.
* **TRAP 3:** Never silently overwrite files owned by Obsidian, GaussBoard, Mermaid Manager, or Anki — see `docs/DECISIONS_AND_TRAPS.md` -> `[TRAP: Canonical-Owner-No-Silent-Clobber]`.
* **TRAP 4:** Never write JSON/YAML/config files with a UTF-8 BOM or commit `ecosystem-secrets.json` / `GROQ_API_KEY` — see `docs/DECISIONS_AND_TRAPS.md` -> `[TRAP: UTF8-No-BOM-And-Secret-Hygiene]`.
* **TRAP 5:** Never flood local Ollama RAM with unconstrained concurrency (`OLLAMA_NUM_THREAD=4`, `MAX_LOADED_MODELS=1`) — see `docs/DECISIONS_AND_TRAPS.md` -> `[TRAP: Ollama-Memory-And-Model-Thrashing]`.
