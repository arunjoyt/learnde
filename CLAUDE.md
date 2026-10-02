# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

`learnde` is a [Frappe](https://frappeframework.com/) app for practicing German. It lives inside a Frappe bench at `$BENCH/apps/learnde` and is installed on a Frappe site.

## Skills

use frappe-app-dev skill

## Common bench commands

All `bench` commands must be run from the bench root (e.g. `~/frappe-bench`), not from inside this app directory.
bench is available at /Users/arunjoyt/Desktop/Work/venv/fb/bin/bench
e.g /Users/arunjoyt/Desktop/Work/venv/fb/bin/bench --version

Frappe site is available at: http://127.0.0.1:8008/
Name of the site is: learnde1.test

```bash
# Run all tests for this app
bench --site <site> run-tests --app learnde

# Run a single test module
bench --site <site> run-tests --app learnde --module learnde.tests.test_foo

# Run a single test case
bench --site <site> run-tests --app learnde --test TestCaseName

# Apply DB migrations after changing DocTypes
bench --site <site> migrate

# Rebuild JS/CSS assets
bench build --app learnde

# Start the dev server (auto-reload on Python changes)
bench start

# Enable tests on a site (required before running tests)
bench --site <site> set-config allow_tests true
```

## Linting & formatting

Pre-commit handles all formatting. Install it once:

```bash
cd apps/learnde
pre-commit install
```

To run checks manually without committing:

```bash
pre-commit run --all-files
```

Tools in use: **ruff** (lint + format, Python), **eslint** + **prettier** (JS/SCSS).

Python style: double quotes, tab indentation, 110-char line length (enforced by ruff).

## Architecture

This is a standard Frappe app. Key conventions:

- **`learnde/hooks.py`** — central wiring file. All event hooks, scheduled tasks, asset includes, and app metadata are registered here. This is the first place to look when adding new behavior.
- **`learnde/modules.txt`** — lists Frappe modules in this app (currently just `Learnde`). Each module maps to a subdirectory under `learnde/learnde/` containing DocTypes, reports, etc.
- **`learnde/learnde/`** — the `Learnde` module. DocTypes live here as subdirectories: each DocType folder contains a `<name>.json` (schema), `<name>.py` (controller), and optionally `<name>.js` (form client script).
- **`learnde/www/`** — portal/web pages (Jinja templates served at their filename path).
- **`learnde/public/`** — static assets (JS/CSS). Built output goes to `public/dist/` (git-ignored).
- **`learnde/templates/`** — shared Jinja includes and page templates.

### Adding a DocType

Create the folder and files under `learnde/learnde/<module>/doctype/<doctype_name>/`, then run `bench --site <site> migrate`. Alternatively use the Desk UI and export with `bench export-fixtures`.

### Whitelisted API methods

Expose a Python function to the client by decorating it with `@frappe.whitelist()` and calling it via `frappe.call('learnde.path.to.method')` from JS.

## CI

- **Linter workflow** (`linter.yml`): runs pre-commit + Frappe Semgrep rules + `pip-audit` on PRs.
