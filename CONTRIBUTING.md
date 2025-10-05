# Contributing to YouTube to Cloud Uploader

Thanks for helping improve **YouTube to Cloud Uploader**! This guide reflects the post-#38 project structure, which separates backend and frontend work and standardizes on the [uv](https://github.com/astral-sh/uv) toolchain for Python development.

## Quick checklist

- [ ] Search existing issues, especially the prioritized ones, before opening something new.
- [ ] Comment on the issue you plan to tackle so we can mark it “in progress”.
- [ ] Create a feature branch from `main` and keep your fork in sync.
- [ ] Run the relevant tests/linters (see below) and update docs as needed.
- [ ] Reference the issue number in your pull request title or description, e.g. `Fixes #45`.

## Project structure overview

```text
.
├── backend/        # FastAPI service (yt-dlp integration, Google Drive upload)
├── frontend/       # Web client (landing/login flows, currently being built out)
├── docs/           # Project documentation (architecture, endpoints, pages)
└── public/         # Static assets and SVG diagrams
```

Most active development happens inside `backend/` right now. Frontend scaffolding is intentionally lightweight and will evolve; check open issues or discussions before introducing new frameworks or build tools.

## Getting started

1. **Fork & clone**

   ```bash
   git clone https://github.com/<your-username>/yt-to-cloud-uploader.git
   cd yt-to-cloud-uploader
   git remote add upstream https://github.com/Manjunath3155/yt-to-cloud-uploader.git
   ```

1. **Install uv (once per machine)**

   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

   On Windows PowerShell, run the installer from the docs: <https://github.com/astral-sh/uv#installation>.

1. **Sync backend dependencies**

   ```bash
   cd backend
   uv sync
   ```

   This creates a `.venv/` managed by uv and installs everything defined in `pyproject.toml` / `uv.lock`.

1. **Run the FastAPI server**

   ```bash
   uv run uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
   ```

1. **Optional: legacy Streamlit UI**

   The historical Streamlit entrypoint is still available at the repo root. If you need it for regression work:

   ```bash
   uv run streamlit run backend/app/main.py
   ```

## Backend contributions

- Follow FastAPI best practices—route handlers live in `backend/app/`.
- Prefer dependency-injected services and keep business logic out of route functions when possible.
- Add or update tests in `backend/tests/` (create the folder if your change introduces the first test for a module).
- Use uv for all tasks:

  ```bash
  uv run pytest
  uv run ruff check
  uv run ruff format
  ```

- If you introduce new dependencies, add them to `pyproject.toml` (not `requirements.txt`) and regenerate the lockfile via `uv lock --upgrade`.
- Secrets (Google OAuth credentials, cloud tokens, etc.) must never be committed. Reference them through environment variables (e.g., `GOOGLE_CLIENT_SECRET_PATH`) and document any new keys in `README.md`.

## Frontend contributions

- Coordinate in the issue tracker before choosing a stack—the team is converging on a modern SPA (Vite/React or similar) but the final decision is tracked publicly.
- Document new scripts and package manager usage (pnpm/npm/yarn) in `frontend/README.md` if you add them.
- Keep the Dockerfile in sync with whichever tooling you introduce. If you’re not touching Docker, leave it as-is.

## Documentation updates

- Update `docs/` when you change endpoints, authentication flows, environment variables, or UI pages.
- The main `README.md` is the landing spot for end users. Keep it concise; move deep details into the docs folder if needed.
- When altering architecture diagrams, commit the source assets (Figma/Draw.io exports) if allowed by licensing.

## Git & branching workflow

1. Create a descriptive branch name, e.g., `backend/add-drive-folder-sync` or `docs/update-oauth-instructions`.
2. Make focused commits with meaningful messages (imperative mood preferred).
3. Rebase on `upstream/main` before opening a PR to minimize merge conflicts.
4. Fill out the PR template, include screenshots or terminal output as proof of successful tests, and list outstanding follow-ups if any.

## Issue reporting & feature requests

- Use the provided issue templates (`Bug report`, `Feature request`).
- Include reproduction steps, logs, and expected vs. actual behaviour for bugs.
- For features, describe the user problem first; proposed implementations can come later in the thread.

## Code of Conduct & licensing

Participation in this project is governed by the [Code of Conduct](CODE_OF_CONDUCT.md). By submitting a contribution, you agree that it will be licensed under the repository’s MIT License.

Thanks again for contributing—see you in the pull requests! ✨
