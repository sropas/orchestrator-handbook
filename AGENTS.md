# Repository Guidelines

## Project Structure & Module Organization
Core backend logic lives under `service/app/` (FastAPI endpoints, pgvector access, templated web UI). Infrastructure configs sit at the root: `docker-compose.yml`, `.env.example`, and `sql/init.sql`. Automation assets go to `tools/` and `examples/`, while operational docs stay in `docs/` (runbook, decision log, meeting template). When you add tests, mirror package namespaces inside `tests/` so fixtures and data builders stay close to the code they verify. Keep n8n exports in `examples/` with clear filenames per flow.

## Build, Test, and Development Commands
Create a virtual environment via `python3 -m venv .venv && source .venv/bin/activate`, then `python3 -m pip install -r requirements.txt`. Launch the API locally with `uvicorn app.main:app --reload --app-dir service/app` and connect to Postgres through Docker or your native instance. Unit tests live under `tests/`; run `python3 -m pytest` before every PR. Static checks use `python3 -m ruff check service tools` and formatting is enforced by `python3 -m black service tools`. For container workflows run `docker compose up -d --build` and share any helper commands through `scripts/`.

## Coding Style & Naming Conventions
Follow PEP 8 (four-space indentation, 88-column guardrails). Use snake_case for functions, PascalCase for classes, and SCREAMING_SNAKE_CASE for constants. FastAPI routers stay in `service/app/main.py` or feature-specific modules; keep schemas in `service/app/schemas.py` and database logic in `service/app/rag.py` or submodules. All new endpoints must declare request/response models and include brief docstrings clarifying behavior or dependencies.

## Testing Guidelines
Favor pytest with descriptive filenames (`tests/test_query_endpoint.py`) and scenarios (`test_upsert_rejects_empty_text`). Mock external embedding providers; integration tests can spin up Dockerized Postgres using `docker compose`. Mark resource-heavy tests with `@pytest.mark.slow` and gate them behind `PYTEST_ADDOPTS=--run-slow`. Target ≥85% coverage for backend modules and add smoke tests for UI routes once templates grow more complex.

## Commit & Pull Request Guidelines
Use Conventional Commit prefixes (`feat:`, `fix:`, `chore:`, `docs:`). Limit subject lines to 72 characters and keep body text wrapped at 100 characters. Every PR must describe scope, validation commands (tests/lint/docker), and link related Jira/GitHub issues. Include screenshots or cURL output when touching `/ask`, `/upload`, or n8n flows. Do not request review until CI is green and security/PDPA notes (if any) are recorded in `docs/decisions/`.

## Role Playbooks
บทบาทแต่ละทีมมีแฟ้มหน้าที่ ความรับผิดชอบ ทักษะ และบทเรียนอยู่ใน `docs/orchestrator/roles/` (เช่น `backend.md`, `security.md`, `qa.md`). อัปเดตทุกครั้งที่มี incident หรือ onboarding เพื่อ reuse ในโครงการถัดไป และอ้างอิงควบคู่กับสถานะรายวันใน `docs/orchestrator/status/`.
