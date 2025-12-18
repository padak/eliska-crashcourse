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
