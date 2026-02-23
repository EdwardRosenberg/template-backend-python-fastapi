# template-backend-python-fastapi

**Tier 2 — FastAPI Archetype Template**

A ready-to-run FastAPI microservice scaffold. It inherits Python stack tooling from [`template-backend-python`](https://github.com/EdwardRosenberg/template-backend-python) and organization-wide governance from [`template-base`](https://github.com/EdwardRosenberg/template-base).

## What This Template Provides

| File / Directory | Purpose |
|---|---|
| `app/main.py` | FastAPI application with Swagger/ReDoc auto-docs |
| `app/routes.py` | Hello World and health check endpoints |
| `tests/test_app.py` | Async test suite using `httpx` + `pytest-asyncio` |
| `pyproject.toml` | Full dependency and tooling config (FastAPI, uvicorn, ruff, mypy, pytest) |
| `.python-version` | Pins Python 3.11 for pyenv / CI consistency |
| `.github/workflows/ci.yml` | CI calling the base reusable workflow with `backend-tech-stack: python` |
| `template-variables.yml` | Variable definitions for the create-project scaffolding workflow |

## Hierarchy

```
template-base  (Tier 0 — org-wide governance)
    └── template-backend-python  (Tier 1 — Python stack tooling)
            └── template-backend-python-fastapi  (Tier 2 — this repo)
```

## Getting Started

### Prerequisites
- Python 3.11+
- pip

### Run locally

```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate   # or venv\Scripts\activate on Windows

# Install dependencies
pip install -e ".[dev]"

# Start the server
uvicorn app.main:app --reload --port 8080
```

### Endpoints

| Method | Path | Description |
|---|---|---|
| GET | `/` | Hello World greeting |
| GET | `/health` | Health check |
| GET | `/docs` | Swagger UI |
| GET | `/redoc` | ReDoc API docs |
| GET | `/openapi.json` | OpenAPI schema |

### Run tests

```bash
pytest
```

### Run linter & formatter

```bash
ruff check .
ruff format --check .
```

## Creating a New Project

Use the **Create Project** workflow in [`template-platform`](https://github.com/EdwardRosenberg/template-platform):

1. Go to **Actions → Create Project → Run workflow**
2. Select `template-backend-python-fastapi` as the template
3. Provide your project name and variable overrides

The workflow will clone this template, substitute your project-specific values, and push to a new repository with CI green from day one.

