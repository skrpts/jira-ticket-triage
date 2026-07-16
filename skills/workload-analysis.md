---
type: skill
id: workload-analysis
title: Workload Analysis
description: "Analyzes ticket distribution across assignees — identifies overload, gaps, and imbalances"
tags: [Production, Project Management]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Your writing style for workload summaries"
    required: false
---

## Capability

Analyzes how tickets are distributed across team members. Identifies who is overloaded, who has capacity, and where work is unevenly distributed.

## When to Use

- As a parallel analysis step after ticket fetching
- When planning sprint adjustments or reallocating work
- Before standup to understand team capacity

## What It Does

1. **Per-assignee breakdown** — counts open tickets, in-progress work, and total story points per person
2. **Overload detection** — flags assignees with significantly more work than the team average
3. **Unassigned work** — counts tickets without an assignee and flags high-priority unassigned items
4. **Blocker chains** — identifies cases where one person's blocked ticket is blocking others
5. **Capacity gaps** — notes assignees with low ticket counts who could take on redistributed work

## Outputs

Structured workload report: per-assignee stats, overload flags, unassigned items, and rebalancing suggestions.
