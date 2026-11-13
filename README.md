# CV–İş İlanı Akıllı Eşleştirme Sistemi

**Project status:** research and planning only; no application functionality is claimed yet.

## Goal
A future localhost academic research prototype comparing **one Turkish backend software developer job posting** against **multiple Turkish resumes**. Include junior and mid-level roles; exclude senior, lead, and managerial roles. English technical terms inside Turkish documents are retained naturally. English resumes and cross-language experiments are deferred. The project compares TF-IDF + cosine with multilingual sentence embeddings + cosine; outputs are text-similarity signals, not applicant suitability judgments.

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
Research-first deliverables include verified literature, lawful and documented source access, a data dictionary, annotation and evaluation feasibility, and an Analiz Raporu. Later deliverables may include a working app, two-model comparison, reproducible evaluation and a separate implementation report. Dataset availability, license, language, labels, and volume must be verified before commitment.

## Academic scope note
Based on project guide item 11, *CV–İş İlanı Akıllı Eşleştirme Sistemi* (page 8). Proposed architecture, UI, evaluation protocol and dataset sampling numbers are implementation decisions, not requirements explicitly specified in that item.

## IMPORTANT — current priority
Approved research scope is Turkish resumes and Turkish **backend software developer** postings, junior and mid-level only. One posting is ranked against multiple resumes. The next project phase is implementation; clearly identified synthetic fixtures may be used for software tests. Real personal CVs and restricted job-ad text remain subject to source-rights, privacy and institutional requirements. English resumes and all cross-language experiments from older plans are deferred.
