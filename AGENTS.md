# Codex Repository Instructions — CV–Job Matching

## Mission
**Research-first objective:** “Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması”. Focus on **Turkish-language resumes and Turkish job ads in one profession (provisional: software developer)**, with one posting ranked against multiple CVs. English technical terms may appear naturally in Turkish documents; bilingual cross-language matching is a possible later extension, **not a requirement of the core research experiment**. Localhost React/FastAPI/SQLite is the planned eventual prototype, not the starting task.

**Do not build an automated hiring or rejection system.** Scores are text similarity signals and must not be presented as objectively measured applicant suitability.

## Required reading order (before editing code)
1. `docs/INSTRUCTOR_FEEDBACK_AND_SCOPE.md`
2. `docs/PROJECT_PROPOSAL.md`
3. `docs/REPORT_REQUIREMENTS.md`
4. `README.md`
2. `docs/PRODUCT_REQUIREMENTS.md`
3. `docs/ARCHITECTURE.md`
4. `docs/DATASET_STRATEGY.md`
5. `docs/NLP_AND_SCORING.md`
6. `docs/EVALUATION.md`
7. `docs/API_AND_DATABASE.md`
8. `docs/SECURITY_AND_PRIVACY.md`
9. `TASKS.md`
10. The nearest nested `AGENTS.md` for any directory being edited

## Hard rules
- **Research gate:** Do not scaffold or write application code until the instructor-facing concise project document, verified literature, lawful Turkish-language data access, one profession, scoring equation, annotation plan and evaluation protocol are documented and approved. Phase 0–2 are documents/research only.
- Work **one approved phase at a time** according to `TASKS.md`. Before implementation, inspect the repository and report the plan, files impacted, risks, and verification commands; await user approval for a new implementation phase.
- Never fabricate data, labels, model scores, tests, metrics, citations, or screenshots. Distinguish implemented, planned, and blocked work.
- Never treat proxy/pseudo labels as independently human-verified ground truth. Review any dataset's license, provenance, privacy, schema and leakage risks prior to importing it.
- Maintain a strict boundary between uploaded personal documents and research datasets. Do not commit either to Git. Do not log resume text or sensitive data.
- Two distinct, testable models are mandatory: **TF-IDF + cosine similarity** baseline and **multilingual sentence embeddings + cosine similarity**. A deterministic skills extractor supports explanations; it is not a substitute for the comparison.
- Never call raw cosine similarity a probability, applicant competency percentage, calibrated fit score, or hiring recommendation. Name and explain each output.
- No external paid AI service, cloud deployment, authentication or OCR in the MVP without explicit approval. Scraping/crawling may be proposed only after site-specific terms, robots/access policy, privacy and lawful collection checks; no scraping before source approval.
- Backend: Python/FastAPI. Frontend: React/TypeScript/Vite. DB: SQLite with SQLAlchemy and Alembic. Host locally at ports 8000 and 5173.
- Avoid gratuitous dependencies, unnecessary third-party services and a redundant Node.js API server.
- Write maintainable code with clear type definitions, small modules, input validation, error handling and tests. Never silently ignore test failures.
- Keep user-visible text in Turkish by default. Turkish CVs/job postings are the **primary experimental scope**. Preserve English-origin technical skill names; cross-language TR–EN experiments are optional only after completing Turkish evaluation.
- Preserve existing working files; inspect `git status` and existing code before modifications. Do not perform destructive git actions or push without permission.
- Update docs and `TASKS.md` when behavior and phase status actually change.
- Two distinct reports are required at delivery: **Analiz Raporu** (planned research) and **Gerçekleştirim Raporu** (actually implemented/tested). Apply the teacher-supplied formatting, APA 7, citations, privacy and AI disclosure rules in `docs/REPORT_REQUIREMENTS.md`. Never fabricate sources, results or implementation evidence.

## Reporting format after each batch
1. Scope implemented and what remains
2. Files added/changed
3. Migrations and commands required
4. Tests, lint, typecheck/build commands and actual results
5. Known issues, security/privacy implications, and blockers
6. Suggested next small task (do not start without approval)

## Definition of done
A documented dataset and data dictionary; defensible evaluation set and split; two model implementations with quantitative comparison and error analysis; functioning localhost UI/API with multi-CV matching, ranked results, matched/not-detected skills; repeatable setup and automated tests; working demonstration and academic report artifacts.
