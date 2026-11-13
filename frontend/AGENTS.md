# Frontend-specific Codex rules

Read root `AGENTS.md`, product and API specs before editing.

- React + TypeScript + Vite; prefer accessible components, strongly typed API contracts and a modest, professional dashboard.
- Turkish UI copy by default; process Turkish and English documents; UI translation itself is not an MVP requirement.
- Screens: Jobs dashboard, Job detail/upload, Candidate ranking, Model evaluation.
- Must have clear empty/loading/error states, per-file upload failures and responsive tables.
- Show separately labeled TF-IDF and semantic scores; ranking changes with selected model.
- Never display fabricated scores/metrics or describe cosine similarity as hiring fit probability.
- Do not expose private local file paths. Ensure delete actions have confirmation.
- Run and report typecheck, lint, tests and build after relevant changes.
