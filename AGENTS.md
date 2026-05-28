# AGENTS.md

## Cursor Cloud specific instructions

### Overview

This is a Python MCP (Model Context Protocol) server for Google Workspace APIs. It uses `uv` for dependency management and `FastMCP` as the MCP framework.

### Development commands

- **Install dependencies:** `uv sync --group dev`
- **Lint:** `uv run ruff check .` and `uv run ruff format --check .`
- **Tests:** `uv run pytest` (264 unit tests, all mock Google APIs — no credentials needed)
- **Run server (HTTP):** `uv run main.py --transport streamable-http`
- **Run server (stdio):** `uv run main.py`

See `README.md` for full CLI options (`--tool-tier`, `--tools`, `--single-user`, `--read-only`, `--permissions`).

### Gotchas

- The MCP endpoint path is `/mcp` (no trailing slash); requests to `/mcp/` get a 307 redirect.
- The health endpoint is `GET /health`.
- `OAUTHLIB_INSECURE_TRANSPORT=1` must be set for local dev (allows HTTP redirect URIs).
- Google OAuth credentials (`GOOGLE_OAUTH_CLIENT_ID`, `GOOGLE_OAUTH_CLIENT_SECRET`) are only needed for live integration testing or running the server with actual Google API access. Unit tests do not require them.
- `uv` auto-downloads the correct Python version (3.11) specified in `.python-version`; the system Python version doesn't matter.
- The manual test file `tests/gappsscript/manual_test.py` is excluded from pytest via `pyproject.toml` (`addopts = "--ignore=tests/gappsscript/manual_test.py"`).
