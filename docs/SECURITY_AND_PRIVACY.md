# Privacy, safety and security

CVs contain personal information. Treat research data and uploaded documents as private even on localhost.

## Research collection boundary
No personal CV data may be collected or processed for this research until the source, purpose, access rights, privacy notice/consent or other applicable legal basis, retention/deletion plan, and any institutional review requirements are confirmed. Publicly viewable job ads are not automatically licensed for collection or redistribution. Do not scrape during the current research phase.

## Required
- Do not store or publish real resumes, phones, addresses, emails, photos or dataset raw text in Git, screenshots or console logs.
- Add `.gitignore` for `.env`, SQLite files, uploads, raw/processed/evaluation datasets, caches, virtual environments and model artifacts.
- Verify license, terms, source and consent for any dataset. Use fictional/consented CVs for public demos.
- Validate extension, MIME where available, file signatures, file size, document parsing errors and multipart limits; use randomized file paths.
- Bind server to localhost by default; restrict CORS to local dev frontend; avoid exposing DB and file directory as static assets.
- Support deletion and record retention behavior; make SQLite backups and private files consistently removable.
- Never auto-reject an applicant, infer protected traits, or describe the algorithm as an impartial hiring judge.
- Avoid sending personal documents to remote models or trackers. No analytics telemetry by default.
- Document limitations and risks: embedding bias, lexical keyword gaming, extracted text quality, cross-language mismatch and false-negative skills.

## Security tests
Unsafe filename, corrupt/oversize upload, duplicate file, invalid extension, encrypted PDF, path traversal, delete cascade, CORS, invalid IDs, and no CV text leakage via logging or exceptions.

## Instructor feedback
Scraping job ads is only an option following explicit website-specific permission/terms/access checks. Prefer consented and anonymized CVs; no personal data in Git or external AI tools. Record provenance in the analysis report.
