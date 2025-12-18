# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Python learning project (crashcourse) + PRD Workflow Kit pro Keboola PM.

## PRD Workflow Kit

### Agent: Elis
Lokální PRD coach agent v `.claude/agents/keboola-prd-coach.md`.

**Aktivace:** Řekni "Elis" nebo "potřebuju PRD"

**Co umí:**
- Hypothesis-driven přístup (navrhuje, PM validuje)
- Challenge patterns (persona mismatch, segment mismatch, vague metriky)
- Flexibilní vstup (nápad → draft → review → update)
- Linear submission (automaticky vytvoří Project + Issue)
- Používá `AskUserQuestion` tool pro strukturované dotazy

### Struktura
```
.claude/
├── agents/
│   └── keboola-prd-coach.md    # Agent Elis
└── skills/
    └── prd-linear/
        ├── SKILL.md             # Linear submission
        └── references/
            ├── prd-template.md      # PRD šablona
            └── keboola-context.md   # Keboola kontext
```

### Testování
E2E testy v `docs/e2e-tests.md` - 42 scénářů pro ověření funkčnosti.

### Plán implementace
Detailní plán v `docs/prd-validator-plan.md`.

### Zdroje
- PRD šablona: `docs/michal_jerabek.md` (konvertováno z PDF)

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
