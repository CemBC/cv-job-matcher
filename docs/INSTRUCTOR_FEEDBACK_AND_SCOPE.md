# Instructor feedback, frozen scope and research-first rule

## Instructor-approved project title
**Türkçe Özgeçmişler ile İş İlanlarının Doğal Dil İşleme Yöntemleriyle Eşleştirilmesi ve Uygunluk Sıralaması**

## What instructor expressly asks before any implementation
- Short and concise project document: problem, **one** purpose, specific measurable objectives, verified related literature, identified datasets/sources and access terms, proposed method, system architecture, success metrics and expected outputs.
- Identify where resumes and job ads will come from, which **non-sensitive** features will be used, how ranking works and exactly how scores are calculated.
- Initially limit project to **one profession**. Provisional proposed choice: **software developer** jobs (backend/full-stack/junior should not be mixed without a clear eligibility rule). Seek teacher approval for the final specialization.
- Scraping/crawling **may** be used only with explicit verification of access rights, website terms, personal-data protections and permissible rate limits.
- Priority is **literature review, problem definition and feasible method**; do not start coding until these are settled.

## Scope changes from initial spec
- Turkish CVs / Turkish job posts are mandatory; English words such as Python, SQL and API are normal technical skills. Cross-language CV–job experiments and English UI are deferred optional extensions.
- Multi-CV per posting and SQLite/local-first architecture are retained for later implementation.
- Do not promise accuracy of any pre-existing English-language pair dataset for Turkish study.
- Use only legal, licensed, anonymized or consented CVs. Public job descriptions still require terms/rights review.

## Gate to begin implementation
The user must explicitly approve the concise project proposal after concrete source/label availability, scoring specification and academic metrics are established. No generated fake CV corpus should be passed off as real empirical data.
