# Instructor feedback, frozen scope and research-first rule

## Instructor-approved project title
**Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması**

## Approved operational scope (2026-11)
- Turkish resumes matched to Turkish **backend software developer** job postings.
- Include junior and mid-level positions; exclude senior, lead, and managerial roles.
- Rank multiple resumes for each individual posting.
- Compare TF-IDF + cosine similarity with a multilingual sentence-embedding approach.
- Consider NDCG@5 as a candidate primary ranking metric only if dataset structure, size, and annotation feasibility support it.
- No dataset is considered licensed, accessible, suitable, or independently labeled until evidence is checked. No scraping, downloading questionable data, application code, or unapproved personal-data collection during research.
- English resumes and TR–EN/EN–TR matching are deferred extensions. English-origin technical terms may occur naturally in Turkish documents.

The original instructor report requirements document is present at `docs/reference/Proje raporu hakkında.docx`. It was read and compared against `docs/REPORT_REQUIREMENTS.md` on 2026-11-13; the Markdown checklist matches the original's report structures, formatting, APA 7, academic-integrity/AI guidance, and distinction between the short proposal, Analysis Report, and Implementation Report. The DOCX remains authoritative if a discrepancy is found.

## What instructor expressly asks before any implementation
- Short and concise project document: problem, **one** purpose, specific measurable objectives, verified related literature, identified datasets/sources and access terms, proposed method, system architecture, success metrics and expected outputs.
- Identify where resumes and job ads will come from, which **non-sensitive** features will be used, how ranking works and exactly how scores are calculated.
- Initially limit project to **one profession**. The user's approved specialization is backend software developer; include junior and mid-level, exclude senior/lead/managerial.
- Scraping/crawling **may** be used only with explicit verification of access rights, website terms, personal-data protections and permissible rate limits.
- Priority is **literature review, problem definition and feasible method**; do not start coding until these are settled.

## Scope changes from initial spec
- Turkish resumes / Turkish job posts are mandatory; English-origin words such as Python, SQL and API may naturally appear as technical terms. English-CV and cross-language experiments are deferred optional extensions.
- Multi-CV-per-posting and SQLite/local-first architecture are retained for later implementation.
- Do not promise accuracy of any pre-existing English-language pair dataset for Turkish study.
- Use only legal, licensed, anonymized or consented CVs. Public job descriptions still require terms/rights review.

## Implementation direction and data-use boundary
The project roadmap proceeds to implementation after the prepared planning documents. No additional instructor-approval checkpoint is introduced by this repository note. Synthetic fixtures may support software development and tests when clearly identified. Real CV or restricted job-ad data still require verified source rights, provenance, applicable privacy and institutional requirements before collection or processing. Synthetic labels must never be represented as independent empirical ground truth.
