# workflows

Reusable GitHub Actions workflows shared by Stauffer Labs repos. Projects created from
[`templates`](https://github.com/stauffer-labs/templates) already call these.

| Workflow | What it runs |
|---|---|
| `python-ci.yml` | `uv sync --locked`, `ruff check`, `ruff format --check`, `pytest` on Python 3.12–3.14 |
| `js-ci.yml` | `npm ci`, `npm run lint`, `npm run typecheck`, `npm test`, `npm run build` on Node LTS |
| `java-ci.yml` | `mvn -B verify` on Java 21 and 25 |

## Usage

```yaml
# .github/workflows/ci.yml in another repo
name: ci
on:
  push:
    branches: [main]
  pull_request:

jobs:
  python:
    uses: stauffer-labs/workflows/.github/workflows/python-ci.yml@v1
    # with:
    #   working-directory: backend
    #   python-versions: '["3.13"]'
```

Every workflow accepts `working-directory`, so one repo can run several (for example
`backend/` and `frontend/` in a full-stack app).

## Versioning

Callers pin the `v1` tag. Backward-compatible changes move `v1` forward:

```bash
git tag -f v1 && git push -f origin v1
```

Breaking changes (renamed inputs, removed steps) get a new major tag (`v2`).

## Access

This repo is public so that both public and private repos can call these workflows. It
contains only CI configuration; keep secrets out of it and pass them from the calling
workflow instead.
