# learnde

A [Frappe](https://frappeframework.com/) app for practising German.

## German Numbers & Alphabets

Open `http://<your-site>/germannumbers`. The page has three modes:

- **Numbers**: The browser speaks a random German number. Type the number you hear. Set the minimum and maximum to change the range (default 1–100).
- **Alphabets**: The page plays a recorded German letter, including ä, ö, ü and ß. Type the letter you hear.
- **Browse A-Z**: Click a letter to hear how it sounds.

Use the replay button to hear the audio again. Press Enter to submit an answer.

## Installation

```bash
cd $YOUR_BENCH
bench get-app https://github.com/arunjoyt/learnde --branch develop
bench --site <your-site> install-app learnde
bench --site <your-site> migrate
```

## Development

### Running the dev server

```bash
cd $YOUR_BENCH
bench start
```

### Applying schema changes

```bash
bench --site <your-site> migrate
```

### Rebuilding JS/CSS assets

```bash
bench build --app learnde
```

### Running tests

```bash
bench --site <your-site> set-config allow_tests true
bench --site <your-site> run-tests --app learnde
```

## Contributing

This app uses `pre-commit` for formatting and linting. Install it once:

```bash
cd apps/learnde
pre-commit install
```

Run checks manually:

```bash
pre-commit run --all-files
```

Tools: **ruff** (Python lint + format), **eslint** + **prettier** (JS/SCSS).

## CI

- **Linter** (`linter.yml`): runs Frappe Semgrep rules and `pip-audit` on PRs.

## License

MIT
