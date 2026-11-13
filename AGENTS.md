# Codex Repository Instructions — CV–Job Matching

## Mission
**Research-first objective:** “Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması”. Approved scope: **Turkish resumes matched to Turkish backend software developer postings**, junior and mid-level only; exclude senior, lead, and managerial roles. Rank multiple resumes for each job posting. Compare TF-IDF + cosine with a multilingual sentence-embedding approach. NDCG@5 is only a candidate primary metric subject to data feasibility. English technical terms may appear naturally in Turkish documents; English CVs and cross-language matching are deferred. Localhost React/FastAPI/SQLite is a planned later prototype, not the starting task.

**Do not build an automated hiring or rejection system.** Scores are text similarity signals and must not be presented as objectively measured applicant suitability.

## Required reading order
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
- Follow the ordered project phases in `TASKS.md`. Do not add instructor/user approval checkpoints beyond explicit task direction. The current project direction permits moving from planning to implementation in the next implementation task.
- **Data-use gate:** software development and automated tests may proceed with clearly identified synthetic fixtures. Do not import, collect, parse, label, or evaluate real personal CVs or restricted job-ad text until source provenance, research/use rights, applicable privacy notice/consent or other legal basis, retention/deletion, and institutional requirements have been checked. This data-use gate does not prevent coding or synthetic software tests.
- Never fabricate data, labels, model scores, tests, metrics, citations, or screenshots. Distinguish implemented, planned, and blocked work.
- Never treat proxy/pseudo labels as independently human-verified ground truth. Review any dataset's license, provenance, privacy, schema and leakage risks prior to importing it.
- Maintain a strict boundary between uploaded personal documents and research datasets. Do not commit either to Git. Do not log resume text or sensitive data.
- Two distinct, testable methods are required: **TF-IDF + cosine similarity** and **multilingual sentence embeddings + cosine similarity**. A deterministic skills extractor supports explanations and required-skill coverage; it is not a substitute for the comparison. Follow `docs/NLP_AND_SCORING.md` and `docs/EVALUATION.md`; preserve explicit conditionality where data feasibility is unknown.
- Never call raw cosine similarity a probability, applicant competency percentage, calibrated fit score, or hiring recommendation. Name and explain each output.
- No external paid AI service, cloud deployment, authentication or OCR in the MVP. Do not scrape/crawl unless a future task explicitly calls for it and site-specific terms, access policy, privacy and lawful collection checks establish permission; public visibility alone is not permission.
- Backend: Python/FastAPI. Frontend: React/TypeScript/Vite. DB: SQLite with SQLAlchemy and Alembic. Host locally at ports 8000 and 5173.
- Avoid gratuitous dependencies, unnecessary third-party services and a redundant Node.js API server.
- Write maintainable code with clear type definitions, small modules, input validation, error handling and tests. Never silently ignore test failures.
- Keep user-visible text in Turkish by default. Turkish resumes and backend postings are the **approved experimental scope**. Preserve English-origin technical terms in Turkish documents; English CVs and cross-language experiments are deferred.
- Preserve existing working files; inspect `git status` and existing code before modifications. Do not perform destructive git actions or push without permission.
- Update docs and `TASKS.md` when behavior and phase status actually change.
- Two distinct reports are required at delivery: **Analiz Raporu** (planned research) and **Gerçekleştirim Raporu** (actually implemented/tested). Apply the teacher-supplied formatting, APA 7, citations, privacy and AI disclosure rules in `docs/REPORT_REQUIREMENTS.md`. Never fabricate sources, results or implementation evidence.

## Reporting format after each implementation batch
1. Scope implemented and what remains
2. Files added/changed
3. Migrations and commands required
4. Tests, lint, typecheck/build commands and actual results
5. Known issues, security/privacy implications, and blockers
6. Suggested next small task in the ordered `TASKS.md` roadmap

## Definition of done
A documented dataset and data dictionary; defensible evaluation set and split; two model implementations with quantitative comparison and error analysis; functioning localhost UI/API with multi-CV matching, ranked results, matched/not-detected skills; repeatable setup and automated tests; working demonstration and academic report artifacts.
