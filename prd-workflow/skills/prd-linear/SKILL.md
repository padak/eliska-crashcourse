---
name: prd-linear
description: Linear submission for Keboola PRD. Creates Project + Issue with PRD in description. Used by keboola-prd-coach agent.
---

# PRD Linear Skill

This skill handles the submission of Product Requirements Documents (PRDs) to Linear by creating projects and issues. It integrates with Linear via MCP tools and is designed to be used by the keboola-prd-coach agent.

## Linear MCP Tools Reference

### Discovery & Listing
- `mcp__linear-server__list_teams` - PM selects target team
- `mcp__linear-server__list_projects` - Check for duplicates, find existing PRD projects
- `mcp__linear-server__list_issues` - Find existing PRD issues in a project
- `mcp__linear-server__list_issue_labels` - Verify available labels

### Create Operations
- `mcp__linear-server__create_project` - Create a new project for the feature
- `mcp__linear-server__create_issue` - Create issue with PRD content

### Update Operations
- `mcp__linear-server__update_issue` - Update existing PRD issue

## Payload Schemas

### Project Creation Payload
```json
{
  "name": "[Feature Name]",
  "description": "PRD for [feature name] - created via PRD Workflow Kit",
  "team": "[team ID]",
  "state": "planned"
}
```

### Issue Creation Payload
```json
{
  "title": "PRD: [Feature Name]",
  "description": "[PRD in markdown format]",
  "project": "[project ID]",
  "team": "[team ID]",
  "labels": ["PRD"]
}
```

## Workflow Steps

### New PRD Submission Flow
1. **List teams** - Present available teams to PM for selection
2. **List projects** - Check for duplicate projects with similar names
3. **Create project** - Create new project or use existing one (if PM confirms)
4. **Create issue** - Create issue with full PRD content in markdown
5. **Return URLs** - Provide links to both the project and issue

### Update Existing PRD Flow
1. **List projects** - Find the target project by name or ID
2. **List issues** - Locate the PRD issue within the project
3. **Update issue** - Replace issue description with updated PRD content

## Error Handling

### Linear MCP Not Installed
- Detect missing MCP connection
- Provide installation instructions:
  ```bash
  claude mcp add --transport sse linear-server https://mcp.linear.app/sse
  ```
- Offer to save PRD locally as fallback

### Timeout Issues
- Retry operation once on timeout
- If retry fails, save PRD locally with timestamp
- Log error details for debugging

### Project Already Exists
- Display existing project details
- Ask PM whether to:
  - Use existing project
  - Create new project with modified name
  - Cancel operation

### Missing Permissions
- Identify permission issue and affected team
- Inform PM of access restrictions
- Offer alternative teams with proper permissions
- Suggest contacting Linear admin if needed

### PRD Content Too Long
- If description exceeds Linear limits:
  - Create executive summary for issue description
  - Add link to full PRD (stored in docs or wiki)
  - Notify PM about the truncation

## Reference Files

This skill relies on the following reference documents:
- `references/prd-template.md` - Standard PRD template structure
- `references/keboola-context.md` - Keboola-specific context and guidelines

## Usage Example

```
PM: Submit this PRD to Linear
Agent:
1. Lists available teams
2. PM selects "Product Team"
3. Checks for existing projects
4. Creates new project "User Authentication Redesign"
5. Creates issue "PRD: User Authentication Redesign" with full PRD content
6. Returns:
   - Project URL: https://linear.app/keboola/project/auth-redesign
   - Issue URL: https://linear.app/keboola/issue/PRD-123
```

## Best Practices

- Always check for duplicates before creating new projects
- Validate team permissions before attempting creation
- Preserve markdown formatting in issue descriptions
- Include project context in project description
- Use consistent naming convention: "PRD: [Feature Name]"
- Add "PRD" label to all PRD issues for easy filtering
