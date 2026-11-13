# NLP pipelines, matching and explanations

## Shared preprocessing
- Parse PDF/DOCX into text; report encrypted/image-only/scanned/corrupt input explicitly (OCR excluded from MVP).
- Unicode normalization, whitespace handling, and Turkish-sensitive case treatment; do not indiscriminately strip `çğıöşü` or convert skill tokens into nonsense.
- Preserve technical terms (`C++`, `C#`, `.NET`, `Node.js`, `SQL`, `CI/CD`) and original unaltered extracted text.
- Detect or use explicit document language where reliable; uncertain detection should be `unknown`.
- Avoid capturing names, gender, age, university prestige, photos and protected/sensitive attributes as matching features.

## Model A: lexical baseline
- scikit-learn TF-IDF + cosine similarity with documented vectorizer config, fit corpus and token strategy.
- For multilingual queries, document limitations (different-language token mismatch) rather than implying semantic bilingual understanding.
- Prevent data leakage by fitting any corpus-dependent transformation on the allowed training/reference partition and applying it consistently.

## Model B: semantic comparison
- Sentence Transformers multilingual model, proposed `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`, **verify license, availability and suitability before pinning**.
- Account for model token limits: section-aware segmentation or chunks with clearly documented aggregation instead of naive full-resume truncation.
- Compare CV and job with identical input policy across candidates; model version and aggregation strategy must be recorded per run.

## Skills extraction and explanations
- Curated bilingual vocabulary, aliases and phrase matching with term boundary safeguards.
- Distinguish `detected matching skill`, `required skill not detected in CV`, and `CV-only skill`; absence in a document does **not** establish lack of competence.
- Show evidence snippets only where safe and avoid hallucinated skills. User-provided requirements and auto-detected skills should be visually distinguished.

## Score semantics
- Provide **two separate similarity scores**; sorting uses the selected one.
- Do not call cosine x 100 'suitability percentage' or calibrate to a hiring probability without a separately validated procedure.
- If scores are transformed to a UI-friendly interval, document formula, range, signed values and limitations. Prefer raw, appropriately rounded cosine numbers in early phases.
- No arbitrary weighted formula mixing skill coverage, TF-IDF and embeddings without defined objective, calibration and validation.

## Performance and reproducibility
- Pin model/artifact versions and seeds where relevant; document embedding batch size, CPU/RAM implications and latency.
- Run same labeled evaluation under both models. Explain errors including negation, language mismatch, missing skills, keyword stuffing and long-document truncation.

## Turkish-first and clear formula
Primary study: Turkish CVs, Turkish developer job ads; English technical terms are retained, but cross-language CV/ads are deferred. Show raw cosine values per independent model, not suitability probabilities. Report separately `skill_coverage = matched_required_skills / explicitly_listed_required_skills` (undefined if no requirements). Never merge with semantic similarity using arbitrary weights. Job-specific ranking evaluation prioritizes NDCG@5 with manually labelled graded relevance, followed by Precision@5 if relevant/non-relevant threshold is documented.
