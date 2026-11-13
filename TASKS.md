# Ordered project and implementation checklist

Statuses: `[ ]` not started; `[~]` in progress; `[x]` completed. Research documentation is prepared; application implementation is not started. The next work phase is implementation. Do not add a new instructor-approval loop. Real-data use remains conditional on documented rights and privacy requirements; synthetic fixtures may be used for software development and tests, never as empirical model labels.

## Phase 0 — Scope, sources, and research documents

- [x] Fix scope: Turkish resumes; Turkish backend developer postings; junior/mid only; one job ranked against multiple resumes; English/cross-language deferred.
- [x] Review relevant literature and record publication metadata, methods, transfer limitations, and source links in `docs/LITERATURE_STARTERS.md`.
- [x] Audit concrete source candidates and unknowns in `docs/RESEARCH_FEASIBILITY.md`; no Turkish matching corpus or source permission is claimed as acquired.
- [x] Record fields, preprocessing, separate scoring definitions and annotation/metric conditions in `docs/NLP_AND_SCORING.md` and `docs/EVALUATION.md`.
- [x] Prepare the short proposal in `docs/PROJECT_PROPOSAL.md` and Analysis Report source in `docs/reports/ANALYSIS_REPORT.md` against the original instructor DOCX.
- [x] Verify `docs/REPORT_REQUIREMENTS.md` against `docs/reference/Proje raporu hakkında.docx`.

## Phase 1 — Dataset validation and provenance

- [ ] For every candidate source, record owner, source URL, terms/license, upstream rights, lawful access method, date obtained, schema, language, role-level counts, volume, missingness, duplicate rate, and label origin.
- [ ] Do not acquire a dataset until its intended research processing, storage, publication/quotation, and deletion rights are documented.
- [ ] Preferred real-data pathway: obtain written employer/data-owner permission for Turkish junior/mid backend job text and a controlled export.
- [ ] Obtain Turkish resumes directly from volunteers only after institutional process, participant information, appropriate legal basis, secure transfer, retention and deletion plan are established.
- [ ] Validate file formats, field definitions, Turkish-language eligibility, role scope, record quality and provenance; keep source proof separate from research text.
- [ ] If real data is unavailable, continue software development with clearly labeled fictional synthetic fixtures. Do not present fixture outcomes as model-effectiveness results.
- [ ] If either job text or CV rights cannot be established, do not run an empirical real-data comparison; document the blocker and use a scope change only if instructed.

## Phase 2 — Privacy-safe ingestion and preprocessing

- [ ] Add private local storage and `.gitignore` coverage for source files, extracted data, labels, SQLite files, model artifacts and environment secrets.
- [ ] Validate extension, MIME/signature where available, size, encryption/corruption and parse failures for PDF/DOCX; OCR is out of MVP scope.
- [ ] Extract only approved job and resume fields; preserve provenance and section origin.
- [ ] Redact direct identifiers and excluded/proxy fields before annotation or modeling; human-check that relevant evidence remains.
- [ ] Apply Unicode NFC, Turkish-aware I/İ casing and whitespace cleanup while preserving technical spellings and punctuation.
- [ ] Record exclusions, parse errors, language/role checks, missing fields and duplicate/near-duplicate decisions without logging private text.

## Phase 3 — Explainable skill extraction

- [ ] Create a versioned Turkish backend skills lexicon with canonical IDs, reviewed aliases, categories and provenance.
- [ ] Implement longest exact-boundary matching for aliases, including punctuation-rich tokens (C++, C#, .NET, Node.js, CI/CD) and false-substring guards (for example, “Go”).
- [ ] Separate posting-required and preferred skills; do not infer requirements from unrelated context.
- [ ] Detect affirmed, negated and ambiguous mentions using explicit, testable context rules; retain evidence snippets in private outputs.
- [ ] Report matched evidence and “CV metninde tespit edilmedi” separately; do not interpret absent text as absent competence.
- [ ] Calculate required-skill mention coverage only when the denominator is nonzero; keep it separate from model scores.

## Phase 4 — TF-IDF + cosine baseline

- [ ] Implement the approved word unigram/bigram and character 3–5 gram channels with the frozen normalization and equal channel weighting in `docs/NLP_AND_SCORING.md`.
- [ ] Fit corpus-derived vocabulary/IDF on the training/reference partition only; transform other partitions without refitting.
- [ ] Return an explicit unavailable state for empty/zero vectors.
- [ ] Record library, tokenizer, lexicon, preprocessing, fitted vocabulary hash and run configuration.
- [ ] Verify deterministic behavior with synthetic unit fixtures and privacy-safe tests.

## Phase 5 — Multilingual sentence embeddings + cosine

- [ ] Before model download/use, verify candidate model revision, license, tokenizer/model-card limits, local dependency compatibility and resource requirements; pin exact revision and versions.
- [ ] Preserve approved section boundaries and chunk long documents using the specified tokenizer-based limit; no silent truncation or generated summaries.
- [ ] Encode all chunks consistently, create the normalized token-weighted document vector, and calculate cosine similarity.
- [ ] Return an explicit unavailable state for documents with no usable chunks.
- [ ] Record model revision, pooling, chunking, tokenization, precision/device, library versions and run parameters.
- [ ] Test deterministic/repeatable behavior with synthetic fixtures; do not claim task quality from smoke tests.

## Phase 6 — Multi-resume ranking and data persistence

- [ ] For each job, rank its multiple resumes by either selected model score; keep model scores separate and never silently blend them.
- [ ] Show model name/version, raw similarity signal, ranking order, skill coverage, evidence and unavailable/error states.
- [ ] Implement SQLite entities and Alembic migrations for jobs, candidates, provenance/consent metadata, skills/evidence, matching runs, result rows and isolated research labels.
- [ ] Define duplicate/reuse behavior and deletion cascades across database records and private files.
- [ ] Ensure research labels cannot accidentally mix with ordinary demo uploads; synthetic records remain marked synthetic.

## Phase 7 — Evaluation set and ranking metrics

- [ ] After real-data permissions/institutional requirements are met, freeze eligibility rules, fixed candidate lists, annotation form and rubric before labels.
- [ ] Have two annotators independently apply 0/1/2 text-evidence relevance labels while blinded to model scores/ranks and identity/demographic cues; preserve first judgments and resolve disagreements by evidence/adjudication.
- [ ] Do not equate volunteer-CV relevance labels with actual hiring outcome or applicant competence when the person did not apply to the job.
- [ ] Check graph-connected leakage across job IDs, resume IDs, duplicates and repeated versions; reserve a held-out job-wise partition only if enough independent groups exist.
- [ ] Calculate each model’s NDCG@5 per rateable job and macro average only for fixed, fully judged lists with at least five candidates and positive IDCG; report both model values and their difference.
- [ ] Compute Kendall tau-b, Precision@5/Recall@5, MRR and ordinal inter-rater reliability only when their specific assumptions/denominators are met; state reasons when undefined.
- [ ] Never treat unjudged pairs, pseudo labels, generated labels or synthetic labels as grade 0 or independent ground truth.
- [ ] Report sample counts and limitations; a five-job pilot is feasibility-only and cannot support generalizable performance claims.

## Phase 8 — FastAPI backend and SQLite integration

- [ ] Implement typed request/response schemas, input validation, useful errors and local health endpoint.
- [ ] Implement job/candidate CRUD, safe batch upload/parse status, matching-run start/read, ranking retrieval, evaluation summary and export according to `docs/API_AND_DATABASE.md`.
- [ ] Bind to localhost defaults, restrict CORS, prevent path traversal, use randomized private paths and avoid raw resume text in logs/errors.
- [ ] Implement deletion and retention behavior consistently across SQLite and private storage.
- [ ] Run migrations on a clean local database and document repeatable setup.

## Phase 9 — React frontend

- [ ] Build Turkish-first dashboard and job detail screens.
- [ ] Support one job with multiple PDF/DOCX resumes, per-file parse states and clear failure messages.
- [ ] Present selectable TF-IDF or embedding rank, separate raw scores, method descriptions, matched skills, not-detected evidence wording, and safe CSV export.
- [ ] Include empty, loading, error and deletion states; never use “% suitable” or hiring/rejection language.
- [ ] Add evaluation comparison view only for actual, permitted results and label synthetic development fixtures clearly.

## Phase 10 — Automated quality and security checks

- [ ] Backend unit/API/database tests for extraction, validation, matching, migrations, error behavior and deletion.
- [ ] Frontend tests for upload, ranking selection, scores/skills display, errors and deletion.
- [ ] Security/privacy cases: oversize/corrupt/encrypted files, invalid extension, duplicate, path traversal, CORS, invalid IDs, delete cascade and no private-text leakage to logs.
- [ ] Run and report lint, typecheck, build and automated tests; fix failures rather than suppressing them.
- [ ] Use synthetic fixtures for normal software tests; use real documents only when their use is authorized.

## Phase 11 — Reports and delivery

- [ ] Keep `docs/reports/ANALYSIS_REPORT.md` as the plan/source report and update only for material plan changes.
- [ ] Prepare a separate Gerçekleştirim Raporu after implementation/evaluation, reporting only actual features, tests, data provenance and measured results.
- [ ] Convert report to the instructor’s requested format (unless official university rules supersede it); preserve APA 7 citation/reference correspondence.
- [ ] Include privacy limitations, error analysis, synthetic-fixture boundary and the required AI-use disclosure.
- [ ] Provide reproducible local setup and demonstration; do not commit raw personal data or secrets.

## Phase transition

Research and proposal documents are prepared. The next project phase is implementation; no additional instructor-approval checkpoint is required by this roadmap. Real-data ingestion and empirical evaluation remain gated by the applicable source-rights, privacy, and institutional requirements above.
