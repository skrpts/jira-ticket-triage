---
type: skill
id: triage-synthesis
title: Triage Synthesis
description: "Combines priority triage and workload analysis into a stakeholder-ready triage report"
tags: [Production, Project Management]
connections:
  - target: llm-service
    type: runs_on
context_params:
  voice_profile:
    label: "Voice Profile"
    description: "Your writing style for the report — tone, vocabulary, and formality"
    required: false
  audience_profile:
    label: "Audience Profile"
    description: "Who will read this report — adjusts detail level and jargon"
    required: false
  report_depth:
    label: "Report Depth"
    description: "How detailed the report should be — Executive, Standard, or Detailed"
    default: "Standard"
    required: false
---

## Capability

Merges the priority triage recommendations and workload analysis into a single, actionable triage report. Leads with what needs attention now, follows with workload health, and includes specific action recommendations.

## When to Use

- After the parallel triage and workload analysis steps have completed
- As the main synthesis step in a triage pipeline

## What It Does

1. **Immediate actions** — tickets to escalate or unblock, listed with assignee and recommended action
2. **Workload health** — team capacity overview, overloaded individuals, rebalancing suggestions
3. **Sprint health** — progress against goals, at-risk items, velocity indicators
4. **Upcoming risks** — approaching deadlines, dependency chains, stale tickets
5. **Recommendations** — specific next steps: reassign ticket X, escalate Y, close Z

## Report Depth Levels

- **Executive** — immediate actions + workload health only. 200–400 words.
- **Standard** — everything above plus sprint health and risks. 400–800 words.
- **Detailed** — full report with per-ticket detail and comprehensive recommendations. 800–1,500 words.

## Outputs

Formatted triage report in markdown, ready to share or use in standup.
