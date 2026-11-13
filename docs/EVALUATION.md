# Proposed manual relevance annotation and pilot evaluation

**Status:** methodology proposal, not data collection or a measured evaluation. The study is Turkish-only: each eligible Turkish junior/mid-level backend developer posting is ranked against multiple eligible Turkish resumes. Senior, lead, managerial, English-resume, and cross-language cases are excluded.

## 1. Unit and annotation rubric

Unit of judgment: one anonymized resume–posting pair. Annotators judge **how much the resume text documents evidence relevant to the explicit requirements and responsibilities of this specific posting**. They do not judge a person's overall worth, protected traits, likely interview success, or hiring outcome.

| Grade | Operational definition | Minimum evidence record |
|---|---|---|
| 0 — No/very limited documented relevance | Resume contains no relevant evidence for most explicit essential backend requirements/responsibilities, or the available evidence clearly concerns a different role. This means evidence is absent from the text, not that the person lacks the skill. | Record the relevant posting criterion and that no supporting resume passage was found, or cite contradictory/out-of-scope passage. |
| 1 — Partial/uncertain documented relevance | Resume documents some relevant backend skills or work, but evidence is incomplete, only preferred requirements are covered, role/experience level is ambiguous, or one or more essential requirements lack clear evidence. | At least one cited resume passage linked to a posting criterion and a short reason for partial/uncertain judgment. |
| 2 — Strong documented relevance | Resume provides direct, specific evidence across the posting’s central backend responsibilities and essential technical requirements; the documented experience level is not clearly inconsistent with the junior/mid scope. | Cite the resume passages supporting the central requirements and map them to the relevant posting criteria. |

This is a text-evidence relevance label, not a suitability or competency ground truth. A posting with vague, internally contradictory, or no extractable essential criteria is flagged “not rateable” with reason; annotators must not fill gaps by guessing.

## 2. Annotation procedure and bias controls

- Before any labels are collected, freeze the rubric, fields, eligibility rules, candidate-list construction, and annotation form.
- Two annotators independently label every pair. Where possible, use reviewers with backend software experience; record relevant reviewer expertise without collecting unnecessary personal details. If only non-specialist reviewers are available, disclose that limitation and do not imply recruiter expertise.
- Annotators receive pseudonymous IDs and the approved redacted posting/resume text plus section labels. Hide applicant identity, demographics, school/employer prestige, source identifiers, and all model scores/ranks.
- Each judgment records grade, confidence/uncertain flag, supporting resume passage(s), linked job requirement(s), and concise reason. A grade 0 must not be phrased as lack of actual skill.
- Preserve both initial labels. Discuss disagreements using only the rubric/evidence; if unresolved, a third reviewer adjudicates. Save the adjudicated label and rationale separately; never overwrite independent judgments.
- Report raw agreement and an ordinal inter-rater reliability statistic (proposed Krippendorff’s alpha with ordinal distance). If only two raters or insufficient label variation makes the statistic unstable/undefined, disclose it and provide the full disagreement table; do not present a misleading single reliability value.
- Do not collect demographic attributes for these relevance labels. This means the project cannot claim subgroup fairness or absence of bias. Remove identity cues from rating materials and report residual bias risk.

## 3. Feasible pilot plan (target, not access claim)

After lawful sources and institutional requirements are approved, run a small annotation/pipeline pilot with a **design target** of 5 permitted postings and 10 eligible resumes per posting (50 unique pairs; 100 independent initial judgments by two raters). If actual recruitment cannot meet this target, record the achieved counts and ask whether the pilot can still answer feasibility questions; do not pretend the target was achieved.

The pilot checks:
1. Can the eligibility rules consistently isolate Turkish junior/mid-level backend postings and resumes?
2. Can the approved fields be extracted and redacted without losing required evidence?
3. Can annotators apply the rubric and cite evidence consistently?
4. Does every pilot posting have at least five candidates, and does each query have nonzero ideal relevance gain?
5. Are both model pipelines reproducible on the same candidate lists without changing the rubric or selecting models by pilot-test results?

The pilot is for feasibility and rubric refinement. If the rubric changes after pilot review, freeze a revised version and re-annotate the pilot or exclude it from the confirmatory test. Pilot scores are not evidence of general performance and are not used to tune the final method and then reused as test.

## 4. Candidate split and leakage safeguards

No split can be finalized before actual source/pair graph structure is inspected. Proposed rule:
- Create connected groups from the bipartite graph of job IDs and resume IDs, also joining exact/near-duplicate records and repeated versions.
- Keep a connected group entirely in one partition; do not place the same resume, job, or near-duplicate content in both development and test.
- Fit all corpus-derived TF-IDF vocabulary/IDF only on training/reference documents. Do not fit thresholds, weights, chunk settings, or model choices on the final test.
- Reserve a locked job-wise test set only if enough independent job groups and candidate lists exist. If graph connectivity or low volume prevents a defensible split, report a pilot/descriptive comparison rather than claiming an independent held-out test.
- Freeze document snapshot IDs, permitted source provenance, transformations, model revisions, and annotation version for reruns.

## 5. Metrics and validity conditions

### Candidate primary: NDCG@5

For each posting q, use adjudicated grades rel(q,i)∈{0,1,2}. Define gain G(rel)=2^rel−1 and discount D(k)=log₂(k+1).

**DCG@5(q) = Σ from k=1 to min(5,n_q) of [G(rel at rank k) / D(k)]**
**NDCG@5(q) = DCG@5(q) / IDCG@5(q)**, where IDCG is the maximum DCG@5 for that posting’s observed judged grades.

Calculate per posting, then macro-average across rateable postings so a posting with many applicants does not dominate. Report the number of included and excluded postings and why.

NDCG@5 is valid only when the candidate pool for each evaluated posting is fixed and fully judged (or the pooling policy is prespecified and all pooled candidates are judged), n_q≥5 for the intended top-five interpretation, and IDCG@5(q)>0. It is undefined if no judged candidate has grade 1 or 2. Do not silently score unjudged resumes as grade 0. If a posting has fewer than five candidates, it may be reported separately as NDCG@min(5,n_q) with that exact denominator, but it is not directly comparable to NDCG@5; exclude from primary NDCG@5 aggregate or report a separate stratum.

### Secondary metrics, conditional

- **Kendall’s tau-b** between each model’s order and the consensus mean ordinal label order, per posting then macro-average, can account for ties. Report only for postings with at least two candidates and non-constant judgments/ranks; otherwise undefined. It summarizes association, not top-five retrieval quality.
- **Precision@5 / Recall@5** only if a binary relevance threshold is frozen before test labels are examined. Proposed operational choice for review: treat grade ≥1 as relevant; because grade 1 is partial/uncertain, report the threshold and preferably a sensitivity analysis using grade 2 only. Require n_q≥5 for Precision@5; Recall@5 requires at least one relevant candidate and a complete judged candidate pool. No valid denominator means “not calculated.”
- **MRR** only if relevance is binarized with a declared threshold and the full candidate pool is judged; otherwise a first relevant result cannot be established.
- **Ordinal inter-rater agreement:** report Krippendorff’s alpha (ordinal) on independent pre-adjudication labels if label variation and sample size support it. Adjudicated labels are not used to calculate initial rater agreement.
- **Descriptive error review:** anonymized examples of false-high/false-low rankings, Turkish inflection, spelling variants, negation, skill keyword stuffing, long text, and unclear job criteria. No fabricated examples/results; any constructed illustration must be marked synthetic.
- **Operational latency/resource metrics** are future implementation measures, not research ranking outcomes and not collected in this methodology-only phase.

Do not calculate aggregate ranking metrics from a lone posting, incomplete candidate judgments, synthetic labels, pseudo-labels, or a test set reused to select model/configuration. A model may score a pair without there being a valid benchmark metric for it.

Metric implementation reference: [scikit-learn ndcg_score](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ndcg_score.html) documents per-query relevance arrays, top-k truncation, ties, and the normalized range. Metric availability does not make an incomplete or unrepresentative dataset valid.

## 6. Synthetic demonstration versus permissioned real evaluation

| Data path | Permitted purpose and label status | Strengths | Limitations and safeguards |
|---|---|---|---|
| Permissioned real job postings + consented/otherwise lawfully processed anonymized resumes | Potential empirical relevance study only after written text-use rights, privacy basis/notice, retention controls, and institutional requirements are confirmed; pair labels are independent human judgments. | Real Turkish phrasing and realistic document variability; allows a defensible, bounded empirical comparison if sample/list depth is adequate. | No current permissions or resume source are confirmed. Volunteers may not be actual applicants to each role, so call labels human-judged text relevance, not hiring suitability. Selection bias, re-identification risk, and small samples remain. |
| Explicitly fictional, authored synthetic examples | UI demonstrations, extraction edge cases, rubric training examples, and pipeline smoke fixtures only. Mark every record and label as synthetic/author-assigned; use no real names, employers, copied CVs, or copied postings. | No collection of personal resume data; precise control over scenarios and edge cases. | Synthetic text is artificial and may encode author assumptions. Author-assigned “expected” labels are scenario metadata, not independently human-verified ground truth; exclude all synthetic examples from empirical performance metrics and claims. |

## 7. No results yet

No dataset has been acquired or approved, no annotation has occurred, and no model has been run. There are no measured metrics, performance claims, screenshots, or implementation tests to report.
