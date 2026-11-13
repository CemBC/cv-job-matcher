# Dataset, provenance and import strategy

## Research goal
Find legally usable, safely processed CV text, job posting text and meaningful CV–job pairing labels or evaluations. The project cannot be academically validated merely by collecting unlabelled resumes or by copying synthetic scores into `ground_truth`.

## Candidate source (not yet approved)
A previously identified Hugging Face candidate: `nonameee12233/job-resume-matching` with separately claimed `cv.csv`, `job.csv`, `matches.csv` files. This is **a lead for verification, not a guaranteed source**. Before download/import: verify current availability, owner, explicit license, source provenance, permitted use, privacy, exact schema, label derivation and data leakage. Do not rely on previously quoted counts until independently audited. If it fails review, choose another source or construct a controlled, consented/fictional example collection. Do not scrape restricted CVs.

## Input audit outputs
`scripts/audit_dataset.py` should support local file inputs and report:
- Row counts, column names/types, required-field coverage and missing values
- Candidate/job identifier uniqueness and match-table foreign-key consistency
- Duplicates, near-duplicates, implausibly short/empty documents
- Languages and text lengths; label types and distributions
- Inferred/synthetic/pseudo label status and dataset provenance
- Potential personally identifying information, sensitive fields and redistribution concerns
- Train/dev/test leakage risks including repeated CVs, jobs and cloned text

## Data organization
- `data/raw/`: unmodified original allowed source files, local only, gitignored.
- `data/processed/`: audited normalized samples; local only, gitignored.
- `data/evaluation/`: human-reviewed label sheets, split manifests and summaries; private content gitignored.
- `data/skills/`: versioned non-sensitive bilingual skill dictionary/aliases; include only authored/licensed files.
- `docs/`: public-safe schema, license notes, data dictionary, aggregate stats, evaluation methodology.

## Proposed initial targets (not factual dataset properties)
- Prototype selection: approximately 1,000 CVs and 100 postings **if** verified dataset supports it.
- Independent manually reviewed evaluation: target >=100 CV–job pairs across TR–TR, EN–EN, TR–EN, EN–TR, with documented annotation rubric and label disagreement handling.
- Group-based splits that prevent the same source CV/job or near-identical content from leaking between test and model-selection sets.

## Labels
Choose the scale after inspecting data: binary relevance or ordinal relevance (e.g. 0/1/2). Define human annotation criteria focused on explicitly stated job requirements and CV evidence, not presumed traits or demographic attributes. Reserve a held-out final test split. Evaluate manually reviewed labels separately from weak/pseudo-label experiments.

## Data minimization
Use fictional/consented examples for UI demonstrations. Do not include real names, contacts or resume bodies in git commits, logs or exported public reports.

## Priority override for Turkish research
Earlier English Hugging Face dataset candidates are **not acceptable as the primary evidence for Turkish CV–job ranking**. Research first: properly consented/anonymized Turkish software-developer CVs and job ads with site-specific usage permission or licensed sources. Do not claim availability or counts without inspection; the proposal must name concrete sources and contingency if none. Scraping is only a conditional option after terms, permissions, robots and privacy review. A dataset without independent ranking labels cannot support NDCG conclusions on its own.
