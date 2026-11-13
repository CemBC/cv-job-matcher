# Architecture and project layout

```text
cv-job-matcher/
├── AGENTS.md
├── README.md
├── TASKS.md
├── CODEX_PROMPTS.md
├── docs/
│   ├── PRODUCT_REQUIREMENTS.md
│   ├── ARCHITECTURE.md
│   ├── DATASET_STRATEGY.md
│   ├── RESEARCH_FEASIBILITY.md
│   ├── NLP_AND_SCORING.md
│   ├── EVALUATION.md
│   ├── API_AND_DATABASE.md
│   └── SECURITY_AND_PRIVACY.md
├── frontend/
│   ├── AGENTS.md
│   ├── src/{pages,components,services,types}/
│   └── package.json
├── backend/
│   ├── AGENTS.md
│   ├── app/{api,core,db,schemas,services,nlp}/
│   ├── alembic/
│   ├── tests/
│   └── requirements.txt
├── data/{raw,processed,evaluation,skills}/
├── scripts/{audit_dataset.py,import_dataset.py}/
├── experiments/
└── .env.example
```

The folders shown are a **target** structure. Codex should create only files needed by the approved phase, not empty decorative modules. The current approved work is documents/research only; no application code is authorized by this target architecture.

## Runtime flow
Browser (React on localhost:5173) → REST API (FastAPI on localhost:8000) → services (text extraction, normalization, skill extraction, both similarity methods) → SQLite + local private storage. The frontend never directly accesses SQLite or private file paths. Heavy NLP inference may initially run sequentially; benchmark before adding a job queue.

## Design constraints
- Keep backend route handlers thin; business logic in services; model inference behind replaceable adapters.
- Pin dependencies and embedding model revision where feasible. Cache/download model once with documented offline behavior and resource needs.
- Save matching run parameters and versions. Avoid storing redundant copies of full CV contents when possible.
- Configuration through env variables with safe localhost defaults; `.env` excluded from version control.
- No Docker required for initial MVP; optional later.

## Implementation status and data-use boundary
This is the target localhost prototype architecture for Turkish backend developer postings and Turkish resumes. Implementation is the next project phase. Synthetic fixtures may be used for software development and tests. Real personal CVs or restricted job-ad text may enter the system only after provenance, use rights, applicable privacy process, retention/deletion and institutional requirements are established. English and cross-language work is deferred.
