# AGENTS.md

This file gives lightweight, practical guidance for humans and agents working in this repository for research, web scraping, and related data tasks. It applies to the entire repo unless a more deeply nested `AGENTS.md` overrides something.

## Scope & Goals
- Keep changes minimal, focused, and reversible.
- Prefer small, well‑scoped PRs with clear commit messages.
- Default to reproducibility: code + config over manual steps.
- Never commit secrets, tokens, or large data blobs.

## Repo Conventions
- Languages: Use the stack already present in a subproject. If starting fresh:
  - Python 3.11+ preferred for scraping/analysis. Alt: Node.js 20+.
- Suggested layout (adapt to context; do not force):
  - `scripts/` one‑off utilities and entrypoints
  - `src/` reusable packages/modules
  - `notebooks/` exploratory notebooks (keep clean, see below)
  - `data/` local, unversioned working data (add to `.gitignore`)
  - `data_samples/` tiny sample files (<1MB) safe to commit
  - `configs/` YAML/TOML/JSON configs for runs
  - `reports/` generated summaries/figures (prefer reproducible generation)
  - `tests/` minimal tests for reusable code

## Data & Secrets
- Do not commit API keys, cookies, or tokens.
  - Use a `.env` file and load via `python-dotenv` (Python) or `dotenv` (Node).
  - Mirror keys in `.env.example` with placeholder values.
- Prefer Git LFS or external storage for files >10MB; otherwise keep data out of git.
- Include a small anonymized sample in `data_samples/` to make examples runnable.

## Scraping Guardrails
- Respect `robots.txt`, site Terms of Service, and rate limits.
- Identify responsibly: set a descriptive `User-Agent` and contact email when appropriate.
- Be polite and resilient:
  - Add jittered backoff on failures (HTTP 429/5xx).
  - Cache responses locally when feasible to reduce load.
  - Avoid parallelism that overloads targets; start small, scale carefully.
- Legal/ethical: Do not bypass paywalls, authentication, or technical protections.
- Prefer structured sources and official APIs where available.

## Suggested Tooling (optional)
- Python: `httpx`/`requests`, `beautifulsoup4`/`lxml`, `selectolax`, `playwright` for headless, `pydantic` for schemas, `tenacity` for retries, `pandas/polars` for data, `typer` for CLIs.
- Node: `undici`, `cheerio`, `playwright`, `zod`, `p-limit`.
- Notebooks: `jupytext` or `nbstripout` to keep diffs clean; avoid committing large cell outputs.

## Coding Style
- Match the style already present. If new Python code:
  - Format with `ruff format` or `black`; lint with `ruff`.
  - Typing: `pyright` or `mypy` on `src/` where practical.
- Keep functions small and pure where possible; isolate I/O and side effects.
- Prefer configuration over hard‑coded constants (store in `configs/`).

## Testing & Reproducibility
- Add minimal tests for reusable logic (e.g., HTML parsers, URL builders, schema validation).
- For scripts/CLIs, provide a `--dry-run` and `--limit` flag for safe trial runs.
- Document a single command to reproduce key results (e.g., in `README.md` or `reports/`).

## Commits & Branches
- Conventional Commits strongly preferred:
  - `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `perf:`, `ci:`
- Write imperative, present‑tense subjects; include scope when helpful.
- Default branch is `main`. Use short‑lived feature branches; rebase or squash on merge.

## Operational Notes for Agents
- Read this file before editing; keep changes minimal and focused.
- When modifying files, prefer surgical diffs and avoid unrelated edits.
- Update docs/configs when behavior changes.
- If a task requires credentials, stop and request them via `.env.example` placeholders.

## Compliance & Attribution
- Preserve attribution and licenses for third‑party datasets/content.
- Note data provenance in `reports/` or dataset README files.
- If scraping, log source URLs and timestamps for traceability.

## Getting Started (example)
- Python:
  1) Create venv, install deps: `uv pip install -r requirements.txt` or `poetry install`.
  2) Copy `.env.example` to `.env` and fill values.
  3) Run a script: `python scripts/scrape_example.py --limit 10 --dry-run`.
- Node:
  1) `corepack enable && pnpm i` (or `npm i`).
  2) Copy `.env.example` to `.env`.
  3) `node scripts/scrape_example.mjs --limit 10 --dry-run`.

---
This file is intentionally concise. Expand conservatively as workflows mature.
