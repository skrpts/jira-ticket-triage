---
type: skill
id: priority-triage
title: Priority Triage
description: "Assesses ticket urgency and recommends triage actions — escalate, schedule, defer, or close"
tags: [Production, Project Management]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Your writing style for triage recommendations"
    required: false
  triage_strictness:
    label: "Triage Strictness"
    description: "How aggressively to escalate — Relaxed, Standard, or Strict"
    default: "Standard"
    required: false
---

## Capability

Analyses each ticket's priority, age, status, and context to recommend a triage action. Goes beyond Jira's priority field — considers staleness, blocker chains, and business impact.

## When to Use

- As a parallel analysis step after ticket fetching
- When you need actionable triage recommendations, not just a status list

## What It Does

1. **Escalate** — tickets that need immediate attention: critical bugs, blockers, overdue items, SLA breaches
2. **Schedule** — tickets that are important but not urgent: medium-priority work, upcoming deadlines, dependencies
3. **Defer** — tickets that can wait: low priority, no deadline, no dependencies
4. **Close/archive** — tickets that are stale, duplicates, or no longer relevant

## Strictness Levels

- **Relaxed** — only escalates critical and blocker-priority tickets. Lets most items flow naturally.
- **Standard** — escalates critical, blockers, and overdue items. Flags items idle for more than a week.
- **Strict** — escalates anything that could become a problem. Flags items idle for more than 3 days. Recommends closing stale tickets aggressively.

## Outputs

Structured triage map: tickets grouped by recommended action (escalate, schedule, defer, close) with reasoning for each.
