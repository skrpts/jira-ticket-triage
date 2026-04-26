---
type: prompt
id: analyse-workload
title: Analyse Workload
description: "Analyses ticket distribution across team members"
tags: [Production, Project Management]
connections:
  - target: workload-analysis
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Analyses how tickets are distributed across assignees to identify overload, gaps, and rebalancing opportunities.

## Voice Profile

{{step.context.voice_profile}}

If a voice profile is provided above, write workload summaries in that voice.

## Prompt

You are a workload analysis agent. Analyse the ticket distribution below.

### What to Compute

**Per assignee:**
- Total open tickets (count)
- Total story points assigned
- Tickets by status (todo, in progress, blocked, in review)
- Overdue ticket count

**Team-level:**
- Average tickets per person
- Average points per person
- Standard deviation (are workloads balanced or skewed?)

### What to Flag

- **Overloaded** — assignee with 50%+ more points than team average
- **Underloaded** — assignee with 50%+ fewer points than team average
- **Unassigned critical** — high/critical priority tickets with no assignee
- **Blocker chains** — person A is blocked, and their ticket is blocking person B
- **Single points of failure** — one person assigned to all tickets in a critical area

### Input

- **Tickets:** {{steps.previous.output}}

### Output Format

```
workload:
  team_average:
    tickets: 5.2
    points: 18
  assignees:
    - name: "Jane Smith"
      tickets: 8
      points: 28
      status: overloaded
      breakdown: { todo: 2, in_progress: 3, blocked: 1, in_review: 2 }
      overdue: 1
  flags:
    - type: "unassigned_critical"
      tickets: ["ENG-190", "ENG-192"]
    - type: "blocker_chain"
      description: "Jane's ENG-145 blocks Bob's ENG-156"
  recommendations:
    - "Reassign ENG-190 (Critical, unassigned) — Jane has capacity after ENG-145 unblocks"
```
