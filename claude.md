# Claude Code Instructions — template-backend-python-fastapi

This is a **Tier 2 archetype template** — a ready-to-run FastAPI microservice scaffold. It inherits Python stack tooling from `template-backend-python` (Tier 1) and organization-wide governance from `template-base` (Tier 0).

## Repository Purpose

Provide a minimal but fully functional FastAPI application that runs, passes tests, and has CI green from day one. New projects are scaffolded from this template via the `create-project.yml` workflow.

## Tech Stack

- **Language**: Python 3.11+
- **Framework**: FastAPI 0.115+
- **ASGI server**: Uvicorn 0.32+
- **Validation**: Pydantic 2.10+
- **API docs**: Swagger UI at `/docs`, ReDoc at `/redoc` (built into FastAPI)
- **Test**: pytest + pytest-asyncio + httpx
- **Linter/Formatter**: Ruff
- **Type checker**: mypy (strict mode with pydantic plugin)
- **CI**: Calls `template-base` reusable workflow with `backend-tech-stack: python`

## Build Commands

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows

# Install dependencies
pip install -e ".[dev]"

# Run all quality checks
ruff check .               # Lint
ruff format --check .      # Format check
mypy .                     # Type check
pytest                     # Tests

# Start the server
uvicorn app.main:app --reload --port 8080
```

**Always run all quality checks before submitting a PR.**

## Project Structure

```
app/
├── __init__.py           # Package marker
├── main.py               # FastAPI application instance and configuration
└── routes.py             # Hello World and health check endpoints

tests/
├── __init__.py           # Package marker
└── test_app.py           # Async test suite using httpx TestClient
```

## What You Can Change

- `app/` — Application code (routes, services, models, schemas)
- `tests/` — Test code
- `pyproject.toml` — Add dependencies to `[project.dependencies]`, add dev tools to `[project.optional-dependencies.dev]`

## What You Must NOT Change

- Do not remove `fastapi`, `uvicorn`, or `pydantic` from `pyproject.toml` dependencies
- Do not remove the `/` (hello) or `/health` endpoints (they serve as smoke tests)
- Do not remove Swagger/ReDoc auto-docs configuration

## FastAPI Conventions

- **App factory**: Define the `FastAPI()` instance in `app/main.py`
- **Router organization**: Use `APIRouter` in separate modules (e.g., `routes.py`), include via `app.include_router()`
- **Endpoint naming**: Use descriptive function names; FastAPI uses them as operation IDs in OpenAPI
- **Response models**: Use Pydantic `BaseModel` subclasses for request/response schemas
- **Path parameters**: Use type annotations for automatic validation (`item_id: int`)
- **Dependency injection**: Use FastAPI's `Depends()` for shared logic (auth, DB sessions)
- **Async endpoints**: Prefer `async def` for I/O-bound endpoints; use plain `def` for CPU-bound work
- **Status codes**: Use `status.HTTP_*` constants from `starlette`

## Python Conventions

- **Python version**: 3.11+ — use modern syntax (match statements, type union `X | Y`, `Self` type)
- **Type hints**: Required on all function signatures — mypy strict mode is enabled
- **Imports**: Use absolute imports; ruff enforces isort-compatible ordering
- **Line length**: 120 characters (configured in ruff)
- **String quotes**: Double quotes (ruff default)
- **Docstrings**: Use Google-style docstrings for public functions and classes
- **Naming**: `snake_case` for functions/variables, `CamelCase` for classes, `UPPER_SNAKE_CASE` for constants

## Testing Conventions

- **Test client**: Use `httpx.AsyncClient` with `ASGITransport` for async HTTP-level tests
- **Async tests**: All test functions should be `async def` — `asyncio_mode = "auto"` is configured
- **Naming**: Test functions should describe behavior: `test_endpoint_condition_expected_result`
- **Every new endpoint must have at least one test**
- **Assertions**: Use plain `assert` statements (pytest style)

## API Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/` | Hello World greeting |
| GET | `/health` | Health check |
| GET | `/docs` | Swagger UI |
| GET | `/redoc` | ReDoc API docs |
| GET | `/openapi.json` | OpenAPI JSON spec |

## Quality Gates

| Gate | Command | Enforced in CI |
|---|---|---|
| Lint | `ruff check .` | ✅ |
| Format | `ruff format --check .` | ✅ |
| Type check | `mypy .` | ✅ |
| Tests | `pytest` | ✅ |
| PR title lint | `pr-title-lint.yml` workflow | ✅ |

## Sync Awareness

**Hierarchy**: `template-base` → `template-backend-python` → **this repo**

Governance files (`.editorconfig`, `.gitignore`, CI workflows, copilot-instructions) are synced from upstream via the platform sync workflow. Do not modify synced files directly — changes will be overwritten.

## Versioning

When making changes, bump the `version` in `pyproject.toml` as part of your commit using semver:

- **PATCH** (e.g., `0.1.0` → `0.1.1`) — bug fixes, doc changes, dependency updates, test additions
- **MINOR** (e.g., `0.1.1` → `0.2.0`) — new endpoints, new dependencies, new features
- **MAJOR** (e.g., `0.2.0` → `1.0.0`) — breaking API changes, Python version upgrade, removing endpoints

Always update the version in the same commit as your functional changes.

## PR Conventions

- Titles must follow Conventional Commits: `<type>(<scope>): <description>`
- Run all quality gates (`ruff check .`, `ruff format --check .`, `mypy .`, `pytest`) before submitting — all must pass

