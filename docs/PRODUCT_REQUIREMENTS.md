# Product requirements

## Primary workflow
1. Create a job posting with title, free-text description and optional user-entered required skills.
2. Upload multiple CVs for that posting (PDF/DOCX); initial product limits: at most **20 files per upload request**, **10 MB per file**. Limits are configurable and benchmarkable.
3. Extract text; record extraction success/failure per file; preserve source text for transparent review.
4. Extract skills using a curated versioned bilingual skill lexicon and alias normalization; preserve source evidence/snippets where feasible.
5. Compute **both** TF-IDF and multilingual embedding similarity; allow sorting under either model, never combine them with undocumented coefficients.
6. List ranked CVs, scores per model, matched skills, skills *not detected in the CV*, and model/method descriptions.
7. Inspect candidate details and previous analyses; export a CSV ranking with model name/version/date.
8. Delete jobs and associated uploaded CV content and processing results.

## Pages
- Dashboard: list/create jobs and recent matching runs.
- Job detail: job description, skill list and batch CV upload with per-file outcomes.
- Ranking: selectable model, candidates table, score explanation, matched/not-detected skills, CSV export.
- Evaluation: offline test results, model comparison chart/table, examples of errors and dataset provenance.

## Explicit exclusions for MVP
- Hiring decision or automatic rejection, fairness guarantees or calibrated hiring probabilities
- OCR of scanned PDFs; external job boards/scraping; account/role management; SaaS deployment
- Translation by cloud LLM, paid inference APIs, applicant contact, email notifications

## Acceptance scenarios
- Turkish CV versus Turkish job; English versus English; both cross-language directions.
- Multi-upload with one corrupt file: valid files succeed, failing file gives a useful error.
- Duplicated upload handling is explicit, predictable and test-covered.
- New matching run produces repeatable ranking with model/version metadata.
- No misleadingly precise 'X% qualified' wording.
- Accessible error/loading/empty states; files and run history removable locally.

## Instructor override (2026-11)
The current academic project title specifies **Türkçe Özgeçmişler**. Narrow to Turkish software developer ads and CVs as provisional occupation; confirm exact job family before data acquisition. Multi-CV per job is retained. English skills inside Turkish documents remain valid; English CV processing is future scope only. Research and short instructor proposal precede code.
