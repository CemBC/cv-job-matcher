# CV–İş İlanı Akıllı Eşleştirme Sistemi

**Project status:** specification only; no application functionality is claimed yet.

## Goal
A localhost-based academic decision-support application that compares **one job advertisement** against **multiple CVs**, supporting both **Turkish and English** text. It shows candidate ordering under a **user-selected similarity method**, two comparable model scores, detected/missing-from-CV skills and transparent explanations. Data is stored locally with SQLite.

## Stack (planned)
- React + TypeScript + Vite; Tailwind CSS; charting library when evaluation UI is implemented
- Python + FastAPI
- NLP: scikit-learn TF-IDF/cosine; sentence-transformers multilingual embeddings/cosine
- PDF: PyMuPDF; DOCX: python-docx; SQLite + SQLAlchemy + Alembic
- pytest (backend); frontend tests/typecheck/lint appropriate to framework

## Local endpoints (after setup)
- UI: `http://localhost:5173`
- API: `http://localhost:8000`
- FastAPI OpenAPI: `http://localhost:8000/docs`

## Codex workflow
Read `AGENTS.md` and the `docs/` specs first. Start with `CODEX_PROMPTS.md` prompt 0. Proceed one phase at a time in `TASKS.md`. Do not report functionality complete before running verification.

## Key deliverables
Working app, datasets' provenance and data dictionary, preprocessing pipeline, two-model comparison, labeled evaluation and reproducible error analysis, tests, documented local setup, final report material.

## Academic scope note
Based on project guide item 11, *CV–İş İlanı Akıllı Eşleştirme Sistemi* (page 8). Proposed architecture, UI, evaluation protocol and dataset sampling numbers are implementation decisions, not requirements explicitly specified in that item.

## IMPORTANT — current priority
Instructor-directed scope is Turkish CVs and Turkish **software developer** job postings as provisional profession. **Do not start coding yet.** First complete `docs/INSTRUCTOR_FEEDBACK_AND_SCOPE.md`, `docs/PROJECT_PROPOSAL.md`, `docs/REPORT_REQUIREMENTS.md` and research-first `TASKS.md`. English CV and TR–EN experiments from older plans are deferred.
