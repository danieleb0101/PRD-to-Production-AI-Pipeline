---
name: prd-to-backlog
description: Convert a PRD into Jira epics and INVEST-compliant stories ready for sprint planning.
version: 1.0.0
---

# PRD to Backlog

Convert a Product Requirements Document into a structured Jira backlog: one Epic and multiple sprint-sized stories.

## Context

Before starting, ask the user to provide:
- The PRD — as a Confluence page link or pasted Markdown (required)
- Tech stack context via `tech-stack.md` (recommended — include in conversation)

## Goal

Produce:
- **1 Epic** — outcome-focused, maps directly to the PRD goal
- **Multiple Stories** — INVEST-compliant, sprint-sized, with testable acceptance criteria

## Workflow

### 1. Parse PRD

Extract from the PRD:
- User flows and key interactions
- Functional requirements
- Edge cases and constraints

### 2. Create Epic

- Title: outcome-focused (what the user achieves, not what gets built)
- Description: summarise the PRD goal in 2–3 sentences
- Link to the PRD Confluence page

### 3. Decompose into Stories

Apply the INVEST criteria to each story:

| Criterion | Meaning |
|---|---|
| **I**ndependent | Can be built and shipped without blocking another story |
| **N**egotiable | Scope can be adjusted without losing the core value |
| **V**aluable | Delivers value to the user or the product |
| **E**stimable | Small enough to size accurately |
| **S**mall | Fits in a sprint |
| **T**estable | Has clear, verifiable acceptance criteria |

### 4. Story Template

Use this format for every story:

```
Title: <concise action — verb + subject>

As a [user type]
I want [action]
So that [benefit]

Description:
- Context: why this matters
- Scope: what is and isn't included

Acceptance Criteria:
Given <precondition>
When <action>
Then <expected result>

Technical Notes (if needed):
- APIs, DB changes, integrations

Dependencies:
- Blocked by: <story title or none>
- Blocks: <story title or none>
```

### 5. Order Stories

- Sequence stories by dependency and delivery value
- Flag any that are blocked by infrastructure or design work
- Highlight the critical path

## Constraints

- Each story must fit in a sprint (max 1 week of effort)
- Acceptance criteria must be testable — no vague language
- No story should span multiple unrelated concerns

## Output

- 1 Epic created in Jira
- Stories created under the Epic, ordered and linked
- Ready for `story-implementation`

## Pipeline

`create-prd` → `prd-to-backlog` → `story-implementation` → PR → Release
