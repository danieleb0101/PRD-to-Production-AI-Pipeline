---
name: story-implementation
description: Implement a Jira story end-to-end — branch, code, PR, and Jira status updates.
version: 1.0.0
---

# Story Implementation

Deliver a Jira story from **To Do** to **In Review**: create the branch, implement the code, open a PR, and update Jira.

## Context

Before starting, ask the user to provide:
- The Jira story — key (e.g. `VLM-42`) or direct link (required)
- Tech stack context via `tech-stack.md` (recommended — include in conversation)
- Access to the codebase (required for implementation)

## Workflow

### 1. Understand the Story

- Read the story title, description, and all acceptance criteria
- Identify the exact scope — what is in and what is out
- If any AC is ambiguous, ask for clarification before writing code
- Move the Jira story to **In Progress**

### 2. Create Branch

Branch format:

```
<JIRA-KEY>-short-description
```

Examples:

```
VLM-42-add-booking-widget
feature/VLM-42-add-booking-widget
```

The Jira key in the branch name is required — it triggers the release automation.

### 3. Implement

- Follow the existing architecture and patterns in the codebase
- Respect constraints in `tech-stack.md` (plugins, SEO, multilingual, caching)
- Do not expand scope beyond the story's AC
- Commit incrementally with clear commit messages

### 4. Validate Acceptance Criteria

Go through each AC one by one:
- Implement the behaviour described
- Mark each criterion as satisfied before moving on
- All AC must be met before opening a PR

### 5. Testing

- Write unit tests for any new logic
- Add integration tests if the change touches external integrations
- Perform manual validation against the AC in a test environment

### 6. Open Pull Request

**Title:** Same as the Jira story title

**Description:**

```
## What
<What was built — 1–3 sentences>

## Why
<Link to Jira story and brief rationale>

## Testing
<How to verify — steps or test cases>
```

### 7. Link PR to Jira

- Add the PR link to the Jira story
- Confirm the branch name contains the Jira key (required for automation)

### 8. Update Jira

Move the story to **In Review** once the PR is open.

> After the PR is merged and deployed, the release workflow automatically transitions the story to Done and creates a GitHub Release.

## Constraints

- All AC must be satisfied before the PR is opened
- Tests must pass
- Branch name must include the Jira key

## Output

- PR opened on GitHub
- Jira story in **In Review**
- Release automation ready to trigger on merge

## Pipeline

`create-prd` → `prd-to-backlog` → `story-implementation` → PR → Release
