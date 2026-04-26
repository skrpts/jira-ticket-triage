---
type: prompt
id: triage-priorities
title: Triage Priorities
description: "Assesses ticket urgency and recommends triage actions"
tags: [Production, Project Management]
connections:
  - target: priority-triage
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Analyses each ticket's priority, age, status, and context to recommend a triage action.

## Voice Profile

{{step.context.voice_profile}}

If a voice profile is provided above, write triage recommendations in that voice. If not, use clear, direct language.

## Configuration

- **Triage strictness:** {{step.context.triage_strictness}}

## Prompt

You are a triage agent. Assess each ticket below and recommend an action.

### Triage Actions

**Escalate** — needs immediate attention:
- Critical or Blocker priority
- Overdue (past due date)
- Blocking other tickets
- Idle in the same status for too long (Standard: 7+ days, Strict: 3+ days)
- SLA breach risk

**Schedule** — important but not urgent:
- High or Medium priority with approaching deadline (within 5 days)
- Dependencies that need to be resolved before other work can proceed
- Items flagged for the current sprint but not yet started

**Defer** — can wait:
- Low priority, no deadline, no dependencies
- Nice-to-have improvements
- Items in backlog with no sprint assignment

**Close** — should be cleaned up:
- Stale tickets (no update in 30+ days, Standard; 14+ days, Strict)
- Duplicates of other active tickets
- Tickets for features that have been descoped or superseded

### Input

- **Tickets:** {{steps.previous.output}}

### Output Format

```
triage:
  escalate:
    - key: "ENG-123"
      reason: "Blocker priority, idle 5 days, blocking ENG-145"
      recommended_action: "Reassign to available developer or pair on it"
  schedule:
    - key: "ENG-134"
      reason: "Due in 3 days, not yet started"
  defer:
    - key: "ENG-167"
      reason: "Low priority, no deadline"
  close:
    - key: "ENG-089"
      reason: "No update in 45 days, feature descoped"
```
