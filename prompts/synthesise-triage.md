---
type: prompt
id: synthesise-triage
title: Synthesise Triage
description: "Combines priority triage and workload analysis into a triage report"
tags: [Production, Project Management]
connections:
  - target: triage-synthesis
    type: derived_from
metadata:
  output_format: markdown
  prompt_type: task
---

## Purpose

Merges the priority triage recommendations and workload analysis into a single, actionable triage report.

## Voice Profile

{{step.context.voice_profile}}

If a voice profile is provided above, write the entire report in that voice. If not, use a clear, professional reporting style.

## Audience Profile

{{step.context.audience_profile}}

If an audience profile is provided, calibrate jargon and detail for this audience.

## Configuration

- **Report depth:** {{step.context.report_depth}}

## Prompt

You are a triage synthesis agent. Combine the priority triage and workload analysis below into a triage report.

### Structure

**Executive** (200–400 words):
1. Headline — one sentence: "X tickets need attention, team is at Y% capacity"
2. Immediate actions — escalated tickets with recommended next steps
3. Workload health — overloaded/underloaded flags only

**Standard** (400–800 words):
1. Headline
2. Immediate actions with context
3. Workload breakdown — per-assignee summary
4. Sprint health — progress, at-risk items
5. Recommendations — specific reassignments and cleanups

**Detailed** (800–1,500 words):
1. Everything in Standard
2. Full triage table — every ticket with its recommended action
3. Per-assignee detail with status breakdowns
4. Blocker chain analysis
5. Stale ticket cleanup list

### Input

- **Priority triage:** {{steps.Priority Triage.output}}
- **Workload analysis:** {{steps.Workload Analysis.output}}

### Formatting Rules

- Use British English throughout
- Use markdown headings, tables, and bullets for scannability
- Lead with what needs action — assume the reader has 60 seconds
- Use Jira ticket keys (ENG-123) when referencing specific issues
- Include assignee names for escalation items
