---
type: workflow
id: jira-ticket-triage
title: Jira Ticket Triage
description: "Fetches Jira tickets via MCP, runs priority triage and workload analysis, and generates an actionable triage report"
tags: [Production, Project Management]
connections:
  - target: fetch-jira-tickets
    type: uses
  - target: priority-triage
    type: uses
  - target: workload-analysis
    type: uses
  - target: triage-synthesis
    type: uses
  - target: language-polish
    type: uses
  - target: jira-mcp
    type: runs_on
  - target: llm-service
    type: runs_on
metadata:
  estimated_duration: "20-60 seconds"
  avg_tokens: 15000
  trigger: manual
output_step: "language-polish"
composite_steps:
  - "fetch-jira-tickets"
  - "priority-triage"
  - "workload-analysis"
  - "triage-synthesis"
  - "language-polish"
execution:
  - skill: "fetch-jira-tickets"
    step_type: "generation"
    prompt: "fetch-jira-tickets"
  - parallel:
    - skill: "priority-triage"
      step_type: "synthesis"
      prompt: "triage-priorities"
      context:
        voice_profile: "Neutral professional tone"
        triage_strictness: "Standard"
    - skill: "workload-analysis"
      step_type: "synthesis"
      prompt: "analyse-workload"
      context:
        voice_profile: "Neutral professional tone"
  - skill: "triage-synthesis"
    step_type: "synthesis"
    prompt: "synthesise-triage"
    context:
      voice_profile: "Neutral professional tone"
      audience_profile: "General professional audience"
      report_depth: "Standard"
  - skill: "language-polish"
    step_type: "content"
    prompt: "polish-triage"
    context:
      voice_profile: "Neutral professional tone"
      grammar_strictness: "Professional"
---

## Overview

This workflow produces an actionable triage report from your Jira board. It fetches tickets matching your criteria (sprint, project, or custom JQL), runs two parallel analysis passes (priority triage and workload analysis), synthesises the results into a report, and applies a final language polish.

The triage goes beyond Jira's built-in views — it considers staleness, blocker chains, workload imbalances, and recommends specific actions (escalate, schedule, defer, close) for each ticket.

## Pipeline Stages

### Stage 1: Fetch Tickets

**Input:** JQL query or project key

Using the Jira MCP service, fetch all matching tickets with full metadata, status transitions, and overdue detection.

**Output:** Structured ticket set.

### Stage 2: Parallel Analysis (Two Agents)

Two analysis agents run concurrently:

#### 2a. Priority Triage

Assesses each ticket's urgency considering priority, age, status, blockers, and deadlines. Recommends one of four actions: escalate, schedule, defer, or close. Configurable via the `triage_strictness` persona dial.

#### 2b. Workload Analysis

Analyses ticket distribution across team members. Flags overloaded individuals, unassigned critical items, blocker chains, and single points of failure. Suggests rebalancing actions.

### Stage 3: Triage Synthesis

Combines the priority triage and workload analysis into a single report. Leads with immediate actions, follows with workload health, and includes specific recommendations. Adapts to the target audience and report depth setting.

### Stage 4: Language Polish

Final cleanup: spelling, grammar, clarity, and Voice Profile alignment. Uses British English throughout.

**Output:** Publication-ready triage report.

## Error Handling

- If the Jira MCP server is unreachable, abort and report the connection error
- If the JQL query is invalid, report the error with a corrected suggestion
- If no tickets match the query, produce a brief "no matching tickets" message
- If the result set is very large (200+ tickets), the fetch step paginates automatically

## Inputs

| Name | Required | Description | Example |
|------|----------|-------------|---------|
| `{{input.jql}}` | No | JQL query. Default: open issues in current sprint. | `project = ENG AND sprint in openSprints()` |
| `{{input.project}}` | No | Jira project key. Used if no JQL provided. | `ENG` |

## Outputs

| Name | Description |
|------|-------------|
| Triage report | Formatted report with escalations, workload analysis, risks, and recommendations |

## Setup

Before running this workflow:

1. **Jira MCP server** — install and configure the `mcp-atlassian` MCP server in your skrptiq settings.
2. **API token** — generate a Jira API token at [id.atlassian.com](https://id.atlassian.com/manage-profile/security/api-tokens) and set it as `JIRA_API_TOKEN`.
3. **Instance URL** — set `JIRA_URL` to your Atlassian Cloud instance (e.g. `https://yourcompany.atlassian.net`).
4. **Username** — set `JIRA_USERNAME` to your Atlassian account email.

## Provider Notes

- Priority triage and workload analysis are analytical tasks — most models handle them well.
- Triage synthesis benefits from a model with strong reasoning for blocker chain analysis.
- The pipeline is moderate on token usage — no long-context requirements.

## Example Input

To test this workflow immediately after import:

```
Project: ENG
```

Or with a custom JQL query:

```
JQL: project = ENG AND status != Done AND sprint in openSprints() ORDER BY priority DESC
```
