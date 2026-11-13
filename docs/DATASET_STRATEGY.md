# Dataset and provenance strategy

## Approved study scope

Turkish resumes matched to Turkish backend software developer job postings. Include junior and mid-level positions; exclude senior, lead, and managerial roles. Each job posting must have multiple resumes for a job-wise ranking task. English technical vocabulary inside Turkish documents is in scope; English resumes and cross-language comparisons are deferred.

## Evidence rule

No source is treated as licensed, accessible, suitable, Turkish, or independently labeled until its owner, source provenance, actual language/content, terms, privacy basis, schema, volume, and label construction have been verified. Public visibility is not permission to scrape, store, redistribute, or publish text. Current research work does not scrape or download questionable datasets.

## Current source candidates

See [`RESEARCH_FEASIBILITY.md`](RESEARCH_FEASIBILITY.md) for the evidence-backed comparison. At present, no Turkish resume source or job-ad source is cleared for acquisition. A commercial Turkish job feed advertises substantial volume but is paid and its reuse rights for this academic project are unknown. A Turkish job-ad skill dataset is a possible skills resource, not an evaluation set. Consent-based resume collection and permission-based job-post collection are the most defensible fallback paths, contingent on approved notices/permissions and institutional requirements.

## Evaluation data requirements (not yet designed)

A defensible list-ranking set needs multiple Turkish junior/mid-level backend resumes per job, human judgments made independently of model output, documented provenance and access permissions, and a held-out design that prevents near-duplicate resume/job leakage. NDCG@5 remains a candidate primary metric only if per-job list depth and annotation support it. No main-study sample-size target is asserted before access and pilot feasibility are established. `docs/EVALUATION.md` gives a small 5-posting × 10-resume design target for a feasibility pilot only, not a claim about available records.

## Permissioned real data vs. synthetic demonstrations

- **Permissioned real data:** candidate path for empirical evaluation only after written rights for job text, a confirmed lawful/privacy process for resumes, required institutional review, and independent human relevance labels. A volunteer resume paired with an unrelated posting is a human-judged text-relevance example, not a real application or hiring outcome. No such source or approval is currently confirmed.
- **Synthetic demonstrations:** fully fictional, clearly marked examples may be used later for UI demonstrations, extraction edge cases, and rubric training. Keep their provenance and author-assigned scenario labels explicit; exclude them from model evaluation metrics and empirical claims. Do not adapt real resumes or copy real postings into synthetic examples without permission.

The proposed pilot target and annotation procedure are documented in `docs/EVALUATION.md`; they are design targets, not claims of available or collected data.

## Audit requirements before any later import

- Verify row counts, fields, field types, missingness, language, role level, duplicates, near duplicates, and text lengths.
- Verify candidate/job IDs, pairing provenance, label definitions, and whether labels are human, behavioral, inferred, synthetic, or model-generated.
- Review source license, terms, privacy and personal-data risks, redistribution rights, and institutional approvals.
- Check repeated CV/job leakage and source/temporal effects before defining splits.
- Keep raw/processed resumes and evaluation labels private and outside Git; publish only approved aggregate statistics and public-safe schema.

## Data separation and minimization

Research resumes must remain separate from any future user-uploaded demo documents. Do not commit CVs, identities, contact details, raw posting text without rights, or private labels. Use only demonstrably fictional or specifically permissioned examples in public demonstrations. Never use pseudo labels as human ground truth.
