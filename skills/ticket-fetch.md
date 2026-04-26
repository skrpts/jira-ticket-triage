---
type: skill
id: ticket-fetch
title: Ticket Fetch
description: "Retrieves Jira tickets via MCP using JQL — issues, statuses, assignees, and priorities"
tags: [Production, Project Management]
connections:
  - target: jira-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
---

## Capability

Fetches issues from Jira using JQL queries via the MCP server. Collects issue metadata, status, assignee, priority, labels, sprint membership, and story points for downstream analysis.

## When to Use

- As the first step in a Jira triage or reporting pipeline
- When you need a snapshot of project or sprint status

## What It Does

1. **Search issues** — calls `search` with the configured JQL query to retrieve matching issues
2. **Gather metadata** — for each issue: key (e.g. PROJ-123), title, status, assignee, priority, labels, story points, epic link, and sprint name
3. **Check transitions** — calls `get_transitions` for in-progress and blocked tickets to understand available next steps
4. **Identify overdue** — flags issues past their due date or sitting in a status for an unusually long time

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.jql}}` | No | JQL query. Default: open issues in the current sprint. | `project = ENG AND sprint in openSprints()` |
| `{{input.project}}` | No | Jira project key. Used if no JQL provided. | `ENG` |

## Outputs

Structured ticket set: list of issues with key, title, status, assignee, priority, labels, points, sprint, epic, and overdue flag.
