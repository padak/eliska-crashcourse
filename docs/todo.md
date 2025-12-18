# TODO: PRD Workflow Kit

## Pending Tasks

### 1. Agent instructions in English
All agent instructions (prompts, system messages) must be written in English for consistency and maintainability.

### 2. Linear content always in English
When the agent writes to Linear (Projects, Issues, PRD content), everything must be saved in English - regardless of the language the user communicates in. The agent can respond to the user in their language, but Linear artifacts are always English.

### 3. Linear MCP tool permissions config (part of deployment process of Elis agent)
As part of deployment (final phase), prepare a configuration for Linear MCP tool call permissions:
- **Auto-approve (read operations):**
  - `list_teams`
  - `list_projects`
  - `list_issues`
  - `list_issue_labels`
  - `get_issue`
  - `get_project`
  - `list_documents`
  - `search_documentation`
- **Require approval (write operations):**
  - `create_project`
  - `create_issue`
  - `update_issue`
  - `create_comment`
  - `create_issue_label`

### 4. GitHub issues pro feedback
Navrhnout mechanismus, jak agent Elis bude vytvářet GitHub issues, pokud uživatel není spokojený s něčím (bug report, feature request, feedback na agenta).

### 5. PRD popis na Linear Project (ne na Issue)
PRD obsah/popis má být uložen na Linear **projektu** (Project description), nikoliv na issue v rámci projektu. Issues v projektu slouží pro jednotlivé úkoly/milestones implementace.

### 6. BUG: Agent nepoužívá AskUserQuestion tool
Agent Elis klade otázky jako plain text/markdown (A/B/C/D choices, numbered lists) místo použití `AskUserQuestion` toolu.

**Status:** NEŘEŠENO - instrukce v agentovi byly posíleny, ale agent je stále ignoruje.

**Pokus 1:** Přidána CRITICAL sekce na začátek `.claude/agents/keboola-prd-coach.md`:
- "YOU MUST use the `AskUserQuestion` tool for ANY question that offers choices"
- "BEFORE your first response: Call `AskUserQuestion`"
- **Výsledek:** Agent stále píše A/B/C/D jako plain text (1 tool use - pravděpodobně čtení souborů, ne AskUserQuestion)

**Možné příčiny:**
1. Agent má přístup k AskUserQuestion toolu? (Potřeba ověřit)
2. Instrukce jsou v markdown code blocks - možná model bere jako příklad, ne instrukci
3. Model preferuje text odpovědi před tool calls
4. Potřeba jiný přístup - např. "NEVER write A/B/C/D options in text"

**Další kroky:**
- [ ] Ověřit že subagenti mají přístup k AskUserQuestion toolu
- [ ] Zkusit negativní instrukce ("NEVER write options as text")
- [ ] Možná potřeba system-level fix, ne jen prompt engineering
