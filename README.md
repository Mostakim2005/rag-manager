# RAG Manager (`rag-manager`)

**RAG Manager** is the central multimodal search, cross-referencing (`xref_id`), and synchronization hub connecting the **5-App Learning Pentagram**:

1. **Obsidian (`Nihon` Vault)** — Markdown notes, canvas boards, `Whiteboards/` sidecars, `Practice Engine` banks
2. **GaussBoard** — Handwritten whiteboards, audio-synced ink strokes, circuits, subtitles
3. **Mermaid Manager** — `.mmd` / `.drawio` / `.svg` diagrams
4. **DeepTutor (`v1.7.4`)** — Tutoring sessions, quizzes, LightRAG, workspace outputs
5. **Anki Desktop (`AnkiConnect`)** — Spaced repetition cards, decks, occlusion images

Plus your local textbook archive (`E:\Books`) and lecture recordings transcribed with **Groq Whisper v3** and embedded with **Ollama (`embeddinggemma-2:440m` multimodal image + text)** into a single SQLite WAL FTS5 + vector index.

## Core Design Principles

* **Zero Duplicate Source Copies:** Each app remains the single canonical owner of its files; `RAG Manager` stores normalized chunks, multimodal embeddings, and `xref_id` edges so updates in any app propagate cleanly without stale duplicates.
* **Zero-Context Discovery:** Resolves all sibling apps, ports, and shared tools dynamically from `%USERPROFILE%\.local\share\ecosystem-registry.json` (`LOCAL_ECOSYSTEM_REGISTRY`) and authenticates using `%USERPROFILE%\.local\share\ecosystem-secrets.json` (`LOCAL_ECOSYSTEM_SECRETS`).
* **Agent & CLI First:** Serves `http://127.0.0.1:27130` (REST + Streamable `/mcp`) and provides the `rag-manager` Typer CLI with strict `--json` output on `stdout` and deterministic exit codes (`0`/`1`/`2`/`3`).

## Documentation Map

* [`docs/PRODUCT.md`](docs/PRODUCT.md) — User vision, planned features, and Pentagram interoperability requirements
* [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) — Visual Mermaid flows, tech-stack rationale, planned module layout, and integration contracts
* [`docs/DECISIONS_AND_TRAPS.md`](docs/DECISIONS_AND_TRAPS.md) — Locked architectural decisions (including Python `uv` over Go+Python split) and engineering traps
* [`AGENTS.md`](AGENTS.md) — Project-specific verification commands and trap pointers

*(Current status: Architecture & specification locked (`2026-11-23`); ready for Phase 1 implementation.)*
