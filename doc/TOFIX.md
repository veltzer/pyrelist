# TOFIX

Findings from a code scan on 2026-10-04.

## High

- `src/pyrelist/main.py:33` - `enumerate(stream)` starts at 0, so every reported `file:line` is off by one versus editors and other linters; use `enumerate(stream, start=1)`.

## Medium

- `src/pyrelist/main.py:36` - `line` still carries its trailing newline, so each match prints an extra blank line, and a line matching several patterns is printed once per pattern; strip the line (`line.rstrip("\n")`) and `break` after the first matching pattern (or report which pattern matched).
- `src/pyrelist/main.py:29` - an invalid regex in the patterns file raises a raw `re.error` traceback without saying which entry is bad; catch it and report the offending pattern and its index.
- `tests/unit_tests/test_basic.py:1` - only import smoke tests exist; the `match` endpoint (the tool's only feature) has no test; add a pytest that writes a patterns JSON and a sample file to `tmp_path` and asserts the exit code and reported locations.
- `README.md:5` - the README (rendered from the shared template) never explains usage or the patterns file format (a JSON list of regex strings, `src/pyrelist/main.py:26-29`); add a `tera.snippets/` usage section rather than editing the generated README.

## Low

- `.yamllint.yaml:1` - a yamllint config is committed but `rsconstruct.toml` has no `[processor.yamllint]`, so `.github/dependabot.yml` and `.github/actionlint.yaml` are not linted; add the processor with `src_files` for those files (and `yamllint` to the dev group).
- `pyproject.toml:85` - `mypy_path = "src:python:scripts"` names `python/` and `scripts/`, which do not exist in this repo; trim to `src`.
- `doc/TODO.txt:1` - the only TODO (raw strings for regexps) is moot since patterns come from JSON, not Python literals; delete it. `doc/DONE.txt:1` refers to an unrelated repo (demos-javascript); remove or explain.
