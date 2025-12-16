# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Python learning project (crashcourse). Currently in initial setup phase.

## Development Environment

- Python 3.14 with virtual environment in `.venv/`
- Activate: `source .venv/bin/activate`

## Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run tests
pytest

# Run single test
pytest tests/test_file.py::test_function

# Update dependencies after installing new packages
pip freeze > requirements.txt
```

## Project Structure (Planned)

```
src/           # Source code
tests/         # pytest tests
docs/          # Documentation
scripts/       # Helper utilities
```

## Python Conventions

- Use type hints in function signatures
- Use `pathlib.Path` instead of `os.path`
- Use f-strings for string formatting
- Follow PEP 8: `snake_case` for functions/variables, `PascalCase` for classes
- Handle exceptions specifically, avoid bare `except:`
