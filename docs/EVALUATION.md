# Academic evaluation and experiment plan

The course guide expects **at least two methods**, measured performance, graphs, short technical interpretation, and a working system. Evaluation must be independent of the app demo.

## Research questions
1. How do lexical TF-IDF/cosine and multilingual embeddings/cosine compare on CV–job pair relevance?
2. How robust is each to TR–TR, EN–EN, TR–EN and EN–TR inputs?
3. What errors are associated with long documents, skill synonyms and cross-lingual phrasing?

## Label strategy
- Prepare a review rubric: relevance according to explicit required skills and documented responsibilities in the job advertisement; no demographic scoring.
- Define labels before seeing model outputs; record who labeled, disputes and adjudication if possible.
- Treat machine-generated compatibility scores only as weak labels, not human truth.
- Maintain held-out test data and avoid fitting thresholds or ranking weights on test.

## Suggested metrics, conditioned on verified label format
- Binary labels and calibrated threshold selected on dev: Precision/Recall/F1; confusion matrix.
- Ordinal judgments and a candidate list per job: NDCG@k and/or Spearman rank correlation, if sample distribution supports these.
- Retrieval with multiple relevant CVs per job: Recall@k, MRR if meaningful for the labels.
- Operational metrics: average and p95 inference latency, document processing failure rate, memory/CPU environment.

## Required outputs
- Dataset report and split manifest; model settings and versions
- Per-method aggregate evaluation plus breakdown by language pair
- Graphs/tables with real values (no fabricated percentages)
- Example failure analysis with appropriately anonymized/fictional text
- Reproducible command or script for rerun

## Success definition
Show a **measured comparison** and transparently explain which method performs better for the verified corpus; no guaranteed preselected winner. Accuracy of hiring decisions is not established by text similarity experiments.

## Instructor-defined primary outcome
Use **NDCG@5** as the predeclared primary metric for job-wise candidate rankings on a manually annotated Turkish test set; use Precision@5 as a secondary metric only when an explicit binary threshold is set. Sample counts and label quality must be verified. Initial multilingual TR–EN metrics are deferred.
