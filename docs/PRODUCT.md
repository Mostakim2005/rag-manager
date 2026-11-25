# Product Intent & Feature Map (`PRODUCT.md`)
> **1-Line Summary:** Permanent source of truth for `RAG Manager` (`rag-manager`), the central multimodal search, cross-referencing (`xref_id`), and sync backbone connecting Obsidian, GaussBoard, Mermaid Manager, DeepTutor, and Anki.

## 1. Core Vision & User Experience Intent
* **What This App Does (Plain English):** `RAG Manager` is the central brain connecting your five study applications (**Obsidian `Nihon` vault**, **GaussBoard**, **Mermaid Manager**, **DeepTutor**, and **Anki Desktop**) plus your textbook library (`E:\Books`) and audio/video lectures. Instead of copying notes or PDFs into separate databases that drift out of sync, every app keeps ownership of its own files while `RAG Manager` indexes text, diagrams, PDF pages, handwritten board exports, flashcards, and Groq Whisper audio transcripts in one shared index and resolves cross-references (`xref_id`) across all five apps.
* **How It Must Feel to Use:** Instant local search (`<50 ms` FTS5 + vector hybrid), zero duplicate files, silent background/event-triggered re-indexing whenever you edit a note/board/diagram/deck, and scriptable CLI (`--json`) + MCP control for DeepTutor and coding agents.

## 2. Planned Core Features (`ECOSYSTEM_INTEGRATION_CONTRACT.md` v2.0.0)

### [WISHLIST: Multimodal-SQLite-Hybrid-Search] | Aliases: fts5, vector search, rrf, embeddinggemma-2, 440m, text and image
> **1-Line Summary:** Indexes text, diagrams, PDF page images, and whiteboard exports using SQLite FTS5 + Ollama `embeddinggemma-2:440m` multimodal embeddings ranked via Reciprocal Rank Fusion (RRF).
* **User Intention:** Search across handwritten boards, PDF textbook diagrams, Mermaid flowcharts, Obsidian notes, and Anki cards in one query without uploading duplicate files to DeepTutor.
* **Target Integration Point:** `src/rag_manager/db.py` (`IndexStore`), `src/rag_manager/embedder.py` (`OllamaMultimodalClient` targeting `127.0.0.1:11434`).

### [WISHLIST: Pentagram-Cross-Reference-And-Live-Propagation] | Aliases: xref_id, canonical owner, deep links, sync propagation, pentagram
> **1-Line Summary:** Tracks canonical ownership (`source_app`), shared `xref_id` edges, and deep links (`obsidian://`, `gaussboard://`, `mermaid-manager://`, `deeptutor://`, `anki://`) so updates in one app refresh indexes and notify dependents without silent file clobbering.
* **User Intention:** Changing a board, diagram, note, or flashcard in one app immediately updates cross-references and search results across the entire Pentagram while never overwriting human edits without confirmation.
* **Target Integration Point:** `src/rag_manager/xref.py` (`XrefGraph`, `sync_xref_edges`), `POST /v1/ingest/event`.

### [WISHLIST: Groq-Whisper-Audio-Transcript-Pipeline] | Aliases: groq whisper, whisper-large-v3, audio transcription, ffmpeg, srt, vtt
> **1-Line Summary:** Automatically chunks lecture audio/video via `ffmpeg.exe`, transcribes speech via Groq Whisper v3 (`whisper-large-v3`), and indexes timestamped cues linked to GaussBoard audio strokes and `Nihon` notes.
* **User Intention:** Search spoken explanations inside recorded lectures and jump straight to the matching video timestamp or GaussBoard ink replay timestamp.
* **Target Integration Point:** `src/rag_manager/transcribe.py` (`transcribe_media_with_groq`, reusing the `HKCU\Environment\GROQ_API_KEY` pattern from `Nihon Subtitles Manager`).

### [WISHLIST: Loopback-REST-And-MCP-Server] | Aliases: port 27130, mcp server, bearer auth, ecosystem-secrets, fastAPI
> **1-Line Summary:** Serves authenticated loopback HTTP REST and Streamable `/mcp` on `127.0.0.1:27130` with DNS-rebinding Host checks, browser Origin rejection, and auto-discovery via `ecosystem-registry.json` and `ecosystem-secrets.json`.
* **User Intention:** Let DeepTutor, Obsidian (`Obsidian Agent Manager`), GaussBoard, Mermaid Manager, and coding agents query and trigger re-indexing safely over one standard local API.
* **Target Integration Point:** `src/rag_manager/server.py` (`create_app`), `src/rag_manager/mcp_server.py` (`register_mcp_tools`).

### [WISHLIST: Agent-Ready-JSON-CLI] | Aliases: rag-manager cli, json output, cli-hub, typer, exit codes
> **1-Line Summary:** Exposes every search, ingest, transcribe, health, and xref action via `rag-manager <cmd> --json` on `stdout` with deterministic exit codes (`0`/`1`/`2`/`3`).
* **User Intention:** Let AI coding agents, PowerShell scripts, GaussBoard, and `cli-hub` workflows drive every capability headlessly.
* **Target Integration Point:** `src/rag_manager/cli.py` (`app`).

### [WISHLIST: Pentagram-App-Connectors] | Aliases: obsidian connector, gaussboard connector, mermaid connector, anki connector, deeptutor connector, alexandria books
> **1-Line Summary:** Dedicated source adapters reading `Nihon` vault notes (live `:27125` + disk fallback), `E:\Books`, GaussBoard `.gaussboard`/sidecars, Mermaid Manager `.mmd`/`.drawio`, AnkiConnect (`:8765`), and DeepTutor (`:8001`).
* **User Intention:** Ingest each app's native format accurately while keeping every original file owned and edited only by its native app.
* **Target Integration Point:** `src/rag_manager/connectors/` (`obsidian.py`, `gaussboard.py`, `mermaid.py`, `anki.py`, `deeptutor.py`, `books.py`).
