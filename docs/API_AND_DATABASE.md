# API and local database design (proposed contract)

## Endpoints
- `GET /api/v1/health` – health check
- `POST /api/v1/jobs` – create a posting (title, description, optional required skills)
- `GET /api/v1/jobs` – list postings
- `GET /api/v1/jobs/{job_id}` – posting details
- `DELETE /api/v1/jobs/{job_id}` – delete posting and dependent data/files under documented policy
- `POST /api/v1/jobs/{job_id}/candidates` – multipart batch PDF/DOCX upload
- `GET /api/v1/jobs/{job_id}/candidates` – candidates and parse statuses
- `GET /api/v1/candidates/{candidate_id}` – safe metadata and extracted text preview
- `DELETE /api/v1/candidates/{candidate_id}` – remove candidate and dependent records/files
- `POST /api/v1/jobs/{job_id}/matching-runs` – compute both models and persist scores (Phase 3+)
- `GET /api/v1/matching-runs/{run_id}` – ranked candidates, selectable `sort_model` (Phase 3+)
- `GET /api/v1/matching-runs/{run_id}/export.csv` – ranking export (Phase 4+)
- `GET /api/v1/evaluation/summary` – audited metrics (Phase 5+)

All response schemas, status codes and failure shapes must be documented and tested; versioned routes should remain backwards-compatible where practical.

## SQLite entities
- `jobs`: id, title, description, language, source, created_at, updated_at
- `candidates`: id, original_name, storage_token, sha256, extracted_text, normalized_text, language, status, created_at
- `job_candidates`: relation between jobs and uploaded/reused candidates, upload provenance; uniqueness within a job
- `job_skills` / `candidate_skills`: normalized skill ID, evidence, extractor version and optional source (entered/detected)
- `matching_runs`: id, job_id, model_versions, config snapshot, created_at, state, errors
- `match_results`: run_id, candidate_id, tfidf_score, embedding_score, sort/rank information, explanation JSON
- `evaluation_labels`: local research sample links, reviewer/rubric/version, label, split; do not attach to production uploads by accident

The final normalized schema may evolve after examining actual workflows; implement migrations with Alembic. Distinguish candidates reused across jobs from physically duplicated files and make delete semantics explicit.

## Upload defaults
- 20 documents maximum/request; 10 MB each; MIME/signature checks plus extension allowlist.
- Save to private local storage using randomized names; defend against traversal and dangerous files.
- Accept partial success and return per-file errors. Detect duplicates by hash and define behavior.
- No raw file contents in error responses/logs.
