# AGENTS.md

## Cursor Cloud specific instructions

### Project overview

ClawWork is an AI agent economic benchmarking platform with two services: a Python FastAPI backend (port 8000) and a React/Vite frontend (port 3000). No database — all data is flat JSONL files under `livebench/data/agent_data/`. See `README.md` for full documentation.

### Running services

- **Backend**: `python3 livebench/api/server.py` (runs on port 8000)
- **Frontend**: `cd frontend && npm run dev` (runs on port 3000, proxies `/api` and `/ws` to backend)
- **Both together**: `./start_dashboard.sh` (note: this script expects conda — skip this in Cloud and start services individually)

### Non-obvious caveats

- `start_dashboard.sh` runs `conda activate base` at the top. In Cursor Cloud there is no conda, so start the backend and frontend separately as shown above.
- Python packages install to `~/.local/bin` which may not be on PATH. Add `export PATH="$HOME/.local/bin:$PATH"` before running `uvicorn` or `fastapi` CLI commands.
- The frontend uses `npm` (lockfile: `package-lock.json`).
- The agent simulation (`livebench/main.py`, `run_test_agent.sh`) requires `OPENAI_API_KEY` to run. The dashboard and API work without it using pre-existing agent data in the repo.
- Frontend build produces some chunk-size warnings — these are non-blocking.
- The backend emits a `DeprecationWarning` about `on_event` in FastAPI — this is cosmetic and does not affect functionality.

### Lint / Test / Build

- **Frontend build**: `cd frontend && npm run build`
- **Frontend dev**: `cd frontend && npm run dev`
- **Backend**: No separate test suite or linter is configured in the repo. The backend is a single FastAPI app that can be validated by starting it and hitting `GET /api/agents`.
