# Backend-specific Codex rules

Read root `AGENTS.md`, architecture, API/data and NLP specs before editing.

- Use FastAPI, Pydantic, SQLAlchemy 2, Alembic; DB is SQLite in local mode.
- Routes are thin; IO and model logic in services. Model implementations must be independently testable.
- Use `pydantic` response schemas rather than returning ORM models directly.
- Use migrations for schema updates. Avoid direct destructive schema resets.
- Handle multi-upload partial failures and no sensitive raw file text in logs/exceptions.
- Make storage-path handling safe, deletion behavior testable and model selection explicit.
- Tests: pytest; validate boundaries, international text, C++/C#/.NET skill tokens, cross-language preprocessing and scoring reproducibility.
- Do not silently download models at API startup without documenting first-run prerequisites.
