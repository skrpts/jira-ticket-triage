---
type: service
id: jira-mcp
title: Jira MCP
description: "Jira MCP server for reading issues, sprints, and project data via Atlassian Cloud"
tags: [Production, Project Management]
connections: []
metadata:
  provider: atlassian
  protocol: mcp
  auth_type: api_token
  env_var: JIRA_API_TOKEN
  required_scopes: [read]
---

## Service Description

Provides access to Jira issue tracking data via the Model Context Protocol (MCP). This service is used to search issues with JQL, fetch issue details, check sprint progress, and read worklog data for triage and reporting.

## Configuration

### Authentication

Requires three environment variables:

- `JIRA_URL` — your Atlassian Cloud instance URL (e.g. `https://yourcompany.atlassian.net`)
- `JIRA_USERNAME` — your Atlassian account email
- `JIRA_API_TOKEN` — an API token generated at [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens)

API tokens inherit the permissions of the account that created them. Ensure the account has read access to the projects you want to triage.

### MCP Server Setup

The Jira MCP server must be configured in your MCP settings using the `mcp-atlassian` package:

```json
{
  "mcpServers": {
    "jira": {
      "command": "uvx",
      "args": ["mcp-atlassian"],
      "env": {
        "JIRA_URL": "{JIRA_URL}",
        "JIRA_USERNAME": "{JIRA_USERNAME}",
        "JIRA_API_TOKEN": "{JIRA_API_TOKEN}"
      }
    }
  }
}
```

Note: `mcp-atlassian` is a Python package installed via `uvx`. It also supports Confluence, but this skrpt only uses the Jira tools.

## Capabilities Used

### Reading

- `search` — search issues using JQL (Jira Query Language). Supports complex filters by project, status, assignee, sprint, priority, labels, and date ranges.
- `get_issue` — retrieve full issue details including title, description, status, assignee, priority, labels, story points, sprint membership, and epic link
- `get_project_issues` — list all issues in a project
- `get_transitions` — get available status transitions for an issue (useful for understanding workflow state)
- `get_worklog` — retrieve time tracking entries for an issue

### Not Used by This Skrpt

- Issue creation and update tools — this is a read-only triage
- Confluence tools — not relevant

## Rate Limiting

Atlassian Cloud rate limits vary by endpoint but are generally generous for read operations (several hundred requests per minute). The triage pipeline typically consumes 5–20 requests depending on the JQL result set size.

## Privacy Considerations

Issue titles, descriptions, assignee names, and comments are sent to your configured LLM provider for analysis. Ensure your organization's policies permit sending Jira data to third-party AI services.
