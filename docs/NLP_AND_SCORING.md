# Research methodology: text preparation, models, and score meaning

**Status:** proposed methodology for user review; no implementation or empirical result exists. The scope is Turkish resumes against Turkish backend developer postings, junior/mid-level only, with multiple resumes ranked for one job.

## 1. Eligible source fields

Use the same approved, redacted job/resume text for both models. Preserve a private original source file only if permitted and necessary; it is never a model feature or report artifact.

### Job posting
Use:
- job title and explicit backend role/seniority wording (the source eligibility filter is junior or mid-level);
- duties and responsibilities relevant to backend software development;
- explicitly stated mandatory technical skills, tools, platforms, and methods;
- explicitly stated preferred technical skills, kept distinguishable from mandatory requirements;
- explicit required relevant experience and education/certification only where the posting states them.

Exclude company/employer name and branding, recruiter/contact details, address and location, compensation/benefits, equal-opportunity boilerplate, application instructions, and unrelated company marketing. Work arrangement/location may be retained as eligibility metadata only if needed to define the study population; it is not similarity text.

### Resume
Use:
- role-relevant profile/summary text;
- technical skills/tools section;
- backend-relevant employment experience descriptions, responsibilities, and accomplishments;
- backend-relevant project descriptions and contributions;
- explicitly stated relevant experience duration/seniority, if provided (do not calculate age or infer years from dates);
- degree/field and certifications only when the posting specifies them as relevant requirements or preferences.

Exclude name, photograph, email, telephone, personal links, exact address/location, date of birth/age, gender/pronouns, nationality, marital/family status, health/disability data, school and former-employer names/prestige, references, and unrelated hobbies. Do not infer protected traits. Never use institution or company prestige as a proxy for ability.

## 2. Extraction and cleaning

1. Read text from permitted digital PDF/DOCX documents; OCR is out of scope. Record unreadable, encrypted, scanned, or corrupt documents as extraction failures instead of silently treating them as empty resumes.
2. Identify sections using headings plus manual spot review. Extract only the fields above. Keep section provenance and the minimum evidence offsets needed for review.
3. Redact direct identifiers and excluded fields before annotation/modeling. Use pseudonymous record IDs. A human quality check must confirm the redaction did not remove job-relevant evidence.
4. Apply Unicode NFC normalization and normalize line endings and repeated whitespace. Apply a Turkish-aware casing function (map capital I to ı and İ to i before lowercase); preserve Turkish characters.
5. Preserve technical forms such as C++, C#, .NET, Node.js, CI/CD, ASP.NET, SQL, and version strings. Do not remove punctuation globally or stem/lemmatize skill names. Keep an unchanged, access-controlled source version only where its use is authorized.
6. Remove repeated headers/footers and boilerplate using documented, auditable rules. Do not summarize, translate, or rewrite content with an LLM.
7. If a source document is not predominantly Turkish, mark it ineligible for the core set and record the exclusion reason; English technical names inside Turkish prose remain eligible.

Specific parser, package, and redaction-rule versions will be recorded and pinned before implementation. No document is included until its source rights are approved.

## 3. Deterministic skills extraction

Use a versioned, hand-reviewed Turkish backend skills lexicon. Each entry has a canonical skill ID, display label, exact aliases, category (language, framework, database, platform, practice, etc.), and source/provenance. ESCO may inform concept IDs where suitable and permitted; it is not assumed to cover backend software terms completely.

- Match longest aliases first using Unicode-aware token boundaries; protect punctuation-rich names (for example, C++, C#, .NET, Node.js, CI/CD) with explicit patterns. Do not substring-match “Go” inside another word.
- Apply Turkish-aware normalization and alias mapping, but preserve the original surface form and a short redacted evidence snippet.
- Use deterministic phrase matching only. Do not infer a skill from a job title, neighboring technology, embedding similarity, or generated explanation.
- A detected mention can be tagged affirmed, explicitly negated, or ambiguous using reviewed local context rules. An ambiguous/negated mention is not counted as a confirmed match; retain its evidence state.
- Distinguish required and preferred job skills. Report “CV’de tespit edildi”, “CV metninde tespit edilmedi” and “bağlam belirsiz/olumsuz” as separate states. Absence from the text is not evidence that the person lacks a skill.

## 4. Two model approaches

The feature text consists of the eligible sections above in a fixed order, with section markers retained. Same documents, redaction, field selection, and role filter are used by both approaches. No skill coverage value is mixed into either model score.

### Model A: TF-IDF + cosine baseline

Proposed reproducible configuration:
- Word channel: before tokenization, replace each reviewed skills-lexicon alias (longest exact boundary match first) with a stable single-token marker derived from its canonical skill ID. Then tokenize with Unicode-aware word runs including single-character tokens and underscores; use word unigrams/bigrams, no stop-word list, stemming, or lemmatization; use_idf=True, smooth_idf=True, sublinear_tf=True, norm=l2, min_df=1, max_df=0.95. Keep the alias map and tokenizer version/hash with the run.
- Character channel: analyzer=char_wb, n-grams 3–5, use_idf=True, smooth_idf=True, sublinear_tf=True, norm=l2, min_df=1, max_df=0.95.
- L2-normalize each channel, concatenate with equal channel scaling (each multiplied by 1/sqrt(2)), then L2-normalize the joint vector.
- Fit vocabularies and IDF statistics on the training/reference partition only; transform validation/test documents without refitting. Record scikit-learn version, tokenizer/regex and fitted vocabulary hash.

For posting j and resume i, let x_j and x_i be the final joint vectors:
**S_TFIDF(j,i) = (x_j · x_i) / (||x_j||₂ ||x_i||₂)**.
If a document has no usable features or a zero vector, return “score unavailable”; do not invent a numeric score.

This word+character configuration is a proposed baseline, not a claim of Turkish backend performance. It must be frozen before the held-out test is viewed.

### Model B: multilingual sentence embeddings + cosine

Candidate model: sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2. Its current model card declares 50 languages, Apache-2.0, 384-dimensional sentence/paragraph embeddings, mean-token pooling, and max sequence length 128. This establishes model-card metadata only; it does **not** establish Turkish backend ranking quality. Before selection, verify license at the pinned revision, Turkish capability/suitability, dependency compatibility, CPU resource needs, and local reproducibility. Record the exact Hub revision hash and software versions.

Proposed long-text policy:
- Preserve section boundaries; segment by paragraphs/sentences into chunks no longer than 112 model-tokenizer wordpieces, leaving room below the model card’s 128-token limit for special tokens. Split an overlong single sentence at token boundaries. No OCR, truncation, generated summaries, or cross-section mixing.
- Use the same chunking policy for posting and resume text. Encode every chunk with the same model and mean-pooling artifact, float32 output; L2-normalize each chunk vector. No task prompt unless the pinned model documents one; record this choice.
- For document D with chunks c, calculate a token-count-weighted mean of chunk vectors, then normalize:
  **e(D) = normalize(Σ_c n_c z_c / Σ_c n_c)**,
  where z_c is the normalized embedding and n_c is the count of unique content wordpieces assigned to that chunk (overlap is not used).
- Score:
  **S_embed(j,i) = (e(j) · e(i)) / (||e(j)||₂ ||e(i)||₂)**.
  If no usable chunks exist, score is unavailable.

Chunk size, section inclusion, model ID/revision, library versions, and device/precision are part of each reproducible run. No fine-tuning is proposed for the initial comparison. A small Turkish qualitative check can identify obvious model failure but cannot establish expected ranking performance.

### Model score interpretation

Both S_TFIDF and S_embed are raw cosine similarity signals (mathematically from −1 to 1; observed ranges depend on these data and models). Show them as separate model-specific values and sort only by the selected method. Do not multiply by 100 or label as a fit/suitability percentage, probability, competency measure, hiring recommendation, or calibrated score.

## 5. Separate descriptive skill coverage

For posting j with explicitly stated mandatory skill set R_j and resume i with confirmed, lexicon-detected skill set C_i:
**Coverage_required(j,i) = |R_j ∩ C_i| / |R_j|**, only when |R_j| > 0.

Report preferred-skill coverage separately:
**Coverage_preferred(j,i) = |P_j ∩ C_i| / |P_j|**, only when |P_j| > 0.

These are document mention-coverage ratios, not competence rates. If the posting does not distinguish mandatory from preferred, mark that field unclassified for review rather than silently assigning a class. If the denominator is zero, show “N/A”. Aliases are deduplicated by canonical skill ID; negated/ambiguous CV mentions do not enter C_i. Always display matched evidence and the “not detected in CV text” wording alongside ratios.

## 6. Calibrated job-fit scores

No calibrated job-fit score is proposed. Calibration would require a separately approved target construct and representative human judgments, a training/development partition, a calibration procedure, held-out calibration assessment, and validity review. Even then, it would predict the defined annotation target—not objective applicant competence or hiring success. Raw cosine and skill coverage must not be relabeled as calibrated fit.

## Reproducibility references

- The candidate model card currently lists 50 languages, Apache-2.0, 384-dimensional embeddings, mean-token pooling, and a 128-token maximum: [Hugging Face model card](https://huggingface.co/sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2). Pin and re-check the exact revision before model use.
- TF-IDF configuration is based on the documented vectorizer options: [scikit-learn TfidfVectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html).
- Cosine computations and embedding similarity are documented by [scikit-learn cosine_similarity](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.pairwise.cosine_similarity.html) and [Sentence Transformers semantic similarity](https://sbert.net/docs/sentence_transformer/usage/semantic_textual_similarity.html).
- Documentation establishes available interfaces and model-card metadata, not task effectiveness on Turkish backend resumes.
