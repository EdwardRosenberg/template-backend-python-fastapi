# Claude Code Instructions — template-backend-python-fastapi

This is a **Tier 2 archetype template** — a ready-to-run FastAPI microservice scaffold. It inherits Python stack tooling from `template-backend-python` (Tier 1) and organization-wide governance from `template-base` (Tier 0).

## Repository Purpose

Provide a minimal but fully functional FastAPI application that passes linting, type checking, and tests out of the box. New projects are scaffolded from this template via the `create-project.yml` workflow with variable substitution.

## Tech Stack

- **Language**: Python 3.11+
- **Framework**: FastAPI (with uvicorn ASGI server)
- **Validation**: Pydantic v2
- **Linter/Formatter**: Ruff (rules inherited from `template-backend-python`)
- **Type checker**: mypy (strict mode, with `pydantic.mypy` plugin)
- **Test framework**: pytest + pytest-asyncio + httpx (async test client)
- **CI**: Calls `template-base` reusable workflow with `backend-tech-stack: python`

## Build Commands

```bash
# Setup
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows
pip install -e ".[dev]"

# Run the server
uvicorn app.main:app --reload --port 8080

# Quality checks
ruff check .               # Lint
ruff format --check .      # Format check
mypy .                     # Type check
pytest                     # Tests
```

**Always run all four quality checks before submitting a PR.**

## Project Structure

```
app/
├── __init__.py            # Package marker
├── main.py                # FastAPI app factory, Swagger/ReDoc configuration
└── routes.py              # Hello World and health check route handlers

tests/
├── __init__.py            # Package marker
└── test_app.py            # Async test suite using httpx AsyncClient

pyproject.toml             # Dependencies, tool config (ruff, mypy, pytest)
template-variables.yml     # Scaffolding variable definitions
```

## What You Can Change

- `app/` — Application code (routes, models, services, middleware)
- `tests/` — Test code
- `pyproject.toml` — Add dependencies to `[project.dependencies]` or `[project.optional-dependencies.dev]`

## What You Must NOT Change

- Do not remove `fastapi`, `uvicorn`, or `pydantic` from dependencies
- Do not remove the health check or root endpoints (they serve as smoke tests)
- Do not change `asyncio_mode = "auto"` in pytest config — all tests use async by default

## FastAPI Conventions

- **App creation**: Use a single `FastAPI()` instance in `app/main.py`
- **Route organization**: Group related endpoints in separate modules under `app/`, include them with `app.include_router()`
- **Path operations**: Use `@router.get()`, `@router.post()`, etc. on an `APIRouter` in route modules
- **Request/Response models**: Define Pydantic models for request bodies and structured responses
- **Status codes**: Return explicit status codes: `status.HTTP_201_CREATED`, `status.HTTP_404_NOT_FOUND`
- **Dependency injection**: Use FastAPI's `Depends()` for shared logic (auth, DB sessions, config)
- **Error handling**: Raise `HTTPException` with appropriate status codes; add custom exception handlers for domain errors
- **API docs**: FastAPI auto-generates Swagger at `/docs` and ReDoc at `/redoc` — keep these enabled

## Python Conventions

- **Type hints**: Required everywhere — mypy strict mode is on with `pydantic.mypy` plugin
- **Async by default**: Use `async def` for route handlers; use synchronous `def` only for CPU-bound helpers
- **Imports**: Absolute imports only; ruff enforces isort ordering
- **Line length**: 120 characters
- **Naming**: `snake_case` for functions/variables/modules, `CamelCase` for classes and Pydantic models
- **Docstrings**: Google-style for public functions and classes

## Testing Conventions

- **Test client**: Use `httpx.AsyncClient` with `ASGITransport` for HTTP-level tests
- **Async tests**: All tests are async by default (`asyncio_mode = "auto"`)
- **Naming**: `test_<route_or_behavior>_<scenario>` (e.g., `test_hello_returns_greeting`)
- **Assertions**: Use plain `assert` statements (pytest rewrites them for clear diffs)
- **Every new endpoint must have at least one test**
- **Test isolation**: Each test should be independent; don't rely on test execution order

## API Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/` | Hello World greeting |
| GET | `/health` | Health check |
| GET | `/docs` | Swagger UI |
| GET | `/redoc` | ReDoc API docs |
| GET | `/openapi.json` | OpenAPI schema |

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

Tool configuration in `pyproject.toml` (ruff rules, mypy settings, pytest options) is **owned by this repo** and not synced — it can be customized freely.

## Versioning

When making changes, bump the `version` field in `pyproject.toml` as part of your commit using semver:

- **PATCH** (e.g., `0.1.0` → `0.1.1`) — bug fixes, doc changes, dependency updates, test additions
- **MINOR** (e.g., `0.1.1` → `0.2.0`) — new endpoints, new dependencies, new features
- **MAJOR** (e.g., `0.2.0` → `1.0.0`) — breaking API changes, framework major upgrade, removing endpoints

Always update the version in the same commit as your functional changes.

## PR Conventions

- Titles must follow Conventional Commits: `<type>(<scope>): <description>`
- Run all quality gates before submitting — all four checks must pass
- Include test coverage for any new endpoint or service function

