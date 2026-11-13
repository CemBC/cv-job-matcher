# Research-first roadmap — approval gates
Statuses: `[ ]` not started; `[~]` in progress; `[x]` done and verified. Every task is initially not started.

## Phase 0 — Teacher feedback and specification alignment (NO CODE)
- [ ] Read `docs/INSTRUCTOR_FEEDBACK_AND_SCOPE.md`, teacher-supplied report constraints and all specs.
- [ ] Propose and request approval for a **single profession** (default proposal: software developer); identify what is deferred.
- [ ] Highlight conflicts between old bilingual scope and Turkish-only research emphasis.
**Gate:** research scope and instructor deliverables explicitly understood.

## Phase 1 — Literature review and lawful dataset feasibility (NO CODE)
- [ ] Verify 3+ relevant peer-reviewed studies: bibliographic info, method, comparison, relevance, limitations.
- [ ] Audit a concrete Turkish CV source and concrete Turkish job-ad source: availability, terms/licensing, privacy, access mechanism, counts, fields.
- [ ] If no safe real CV dataset, present consented/anonymized collection protocol; don't claim dataset already exists.
- [ ] Define a small, feasible annotation protocol for multiple CVs per job and decide how to address insufficient labels.
**Gate:** written, source-backed feasibility; no unverifiable datasets.

## Phase 2 — Instructor-facing proposal and ANALİZ RAPORU (NO CODE)
- [ ] Finalize `docs/PROJECT_PROPOSAL.md` into a short, concise submission: one objective, concrete targets, literature, data, formula, architecture, metrics and outputs.
- [ ] Prepare **Analiz Raporu** following `docs/REPORT_REQUIREMENTS.md` with APA 7 and clearly planned activities.
- [ ] Obtain explicit approval to begin implementation; do not automatically advance.
**Gate:** approved research specification and identified data source(s), not aspirational claims.

## Phase 3 — Local app scaffolding and data ingestion
- [ ] React/Vite/TypeScript + FastAPI health endpoint; SQLite/SQLAlchemy/Alembic.
- [ ] Secure batch PDF/DOCX uploads, parsing, deletion, privacy handling; job management.
- [ ] Data audit/import only for approved datasets.
**Gate:** working and tested upload/inspect/delete localhost demo.

## Phase 4 — Two independent NLP models and explained ranking
- [ ] Turkish-friendly TF-IDF cosine baseline; Turkish-capable sentence embedding cosine.
- [ ] Job-specific candidates ranked by selected method; independent skill coverage, snippets and missing-document-evidence labels.
- [ ] Preserve model versions, chunk strategy and reproducibility.
**Gate:** genuine model outputs on repeatable tests; no fake percentages.

## Phase 5 — Empirical comparison
- [ ] Human-labelled job-specific ranking test set, rubric, leakage-safe splits.
- [ ] NDCG@5 primary, Precision@5 secondary where valid; reported baseline and error cases.
- [ ] Graphs and interpretation.
**Gate:** computed reproducible metrics only.

## Phase 6 — GERÇEKLEŞTİRİM RAPORU and delivery
- [ ] Prepare actual system/results report following `docs/REPORT_REQUIREMENTS.md` (A4, APA 7, specific margins etc).
- [ ] Documentation, source/data provenance, actual tests, model comparison and local demo.
- [ ] AI-use disclosure, error analysis and privacy check.
**Gate:** working app + two properly evidenced reports.

## Change control
No code while Phase 0–2 are being conducted. Stop at each gate and await user approval.
