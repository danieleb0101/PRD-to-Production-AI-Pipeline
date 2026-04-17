---
name: create-prd
description: 'Transform raw ideas into structured, evidence-based PRDs aligned with product strategy and ready for backlog breakdown. Use when: write prd, product requirements, feature spec.'
effort: high
---

# PRD Generator

Create a high-quality Product Requirements Document (PRD) from a raw product idea. The output is structured for alignment, prioritisation, and direct breakdown into Jira stories.

## Context

Before starting, ask the user to provide:
- The feature idea or problem to solve (required)
- Company context via `company.md` (recommended — include in conversation)

Do not invent data. If context is missing, ask clarifying questions before proceeding.

## Process

### 1. Context Discovery
- Identify the product area, target user persona, and relevant company priorities from `company.md`
- Ask what problem this solves and who it is for if not specified

### 2. Problem Framing
- State the problem clearly and concisely
- Separate validated pain points from assumptions
- Do not propose solutions yet

### 3. Evidence Extraction
- List what is known (user feedback, analytics, prior research)
- Label each item as **validated** or **assumed**
- Flag any gaps that need validation before building

### 4. Success Criteria
- Define leading indicators (e.g. click-through rate, feature adoption)
- Define lagging indicators (e.g. revenue, retention)
- Make criteria measurable

### 5. Dependency Mapping
- Identify feature dependencies (what must exist first)
- Identify team dependencies (design, infra, third-party)
- Identify the critical path

### 6. Solution Design
- Describe the proposed solution at the feature level
- Include user stories in `As a / I want / So that` format
- Define explicit non-goals (what is out of scope)

### 7. Risk Assessment
Evaluate across four dimensions:
- **Value risk** — will users want this?
- **Usability risk** — will users understand how to use it?
- **Feasibility risk** — can it be built with current stack?
- **Viability risk** — does it make business sense?

### 8. Structured PRD Output

Produce the PRD in this structure:

```
# [Feature Name]

## Problem
## Target Users
## Evidence
## Success Criteria
## Solution Overview
## User Stories
## Non-Goals
## Dependencies
## Risks
## Open Questions
```

## Output

- Structured PRD in Markdown
- Saved to Confluence as a new page (requires Atlassian connector)
- Ready for `/prd-to-backlog`

## Pipeline

`create-prd` → `prd-to-backlog` → `story-implementation` → PR → Release
