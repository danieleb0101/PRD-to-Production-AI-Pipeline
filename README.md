# PRD-to-Production-Pipeline

An AI-powered delivery pipeline that takes a product idea all the way to a production release. Claude handles planning and reasoning; Confluence, Jira, and GitHub handle execution through event-driven automation.

## Pipeline Overview

```
Product Idea
    ↓
[1] create-prd          → Structured PRD saved to Confluence
    ↓
[2] prd-to-backlog      → Epic + Stories created in Jira
    ↓
[3] story-implementation → Code committed, PR opened, Jira updated
    ↓
    Merge to main
    ↓
GitHub Actions          → Deploy to production, close Jira story, create release
```

---

## Requirements

### Claude Plugins (MCP Connectors)

Install from the [Claude Plugins Marketplace](https://claude.com/plugins):

- **Atlassian** — required for Confluence and Jira integration
- **GitHub** — required for PR creation and branch management

### External Services

- Jira project — hosts the backlog and tracks story status
- Confluence space — stores PRDs
- GitHub repository — hosts code and runs Actions

### GitHub Secrets

Add the following in **Repository → Settings → Secrets and variables → Actions**:

| Name | Description |
|---|---|
| `SSH_HOST` | Production server hostname |
| `SSH_USER` | SSH username |
| `SSH_PRIVATE_KEY` | SSH private key for deployment |
| `JIRA_BASE_URL` | e.g. `https://yourcompany.atlassian.net` |
| `JIRA_EMAIL` | Jira account email |
| `JIRA_API_TOKEN` | Jira API token ([generate here](https://id.atlassian.com/manage-profile/security/api-tokens)) |

---

## Skills

Skills are packaged Claude instructions that guide Claude through each pipeline step. They are distributed as `.skill` files and, once installed, trigger automatically when Claude recognises the context.

### Installation

Each skill is pre-packaged in the [`dist/`](dist/) folder. To install:

1. Clone this repo
2. Open any `.skill` file in Claude Code:
   - [`dist/create-prd.skill`](dist/create-prd.skill)
   - [`dist/prd-to-backlog.skill`](dist/prd-to-backlog.skill)
   - [`dist/story-implementation.skill`](dist/story-implementation.skill)
3. Claude Code will prompt you to install — confirm and the skill is ready

The context files (`company.md`, `tech-stack.md`) are bundled inside each `.skill` file — no need to paste them manually.

> The `skills/` directory contains the skill source files and `dist/` the packaged output. If you modify a skill source, re-package it with the [skill-creator](https://claude.com/plugins) and replace the file in `dist/`.

### Prerequisites for each session

1. Ensure the Atlassian and GitHub MCP connectors are active
2. Skills trigger automatically — or invoke them explicitly as shown below

---

### 1. `create-prd`

Turns a raw product idea into a structured PRD and saves it to Confluence.

**Context to include:**
- [`skills/create-prd/context/company.md`](skills/create-prd/context/company.md) — company profile, audience, and product philosophy

**How to invoke (Claude Code):**
```
/create-prd

[Paste company.md content here]

Feature idea: <describe your idea>
```

**Output:** Structured PRD saved as a Confluence page, ready for `prd-to-backlog`.

---

### 2. `prd-to-backlog`

Reads a PRD and creates a Jira Epic with INVEST-compliant stories.

**Context to include:**
- [`skills/prd-to-backlog/context/tech-stack.md`](skills/prd-to-backlog/context/tech-stack.md) — platform architecture, plugins, and constraints

**How to invoke (Claude Code):**
```
/prd-to-backlog

[Paste tech-stack.md content here]

PRD: <link to Confluence page, or paste PRD content>
```

**Output:** 1 Epic + scoped stories created in Jira, ready for sprint planning.

---

### 3. `story-implementation`

Implements a single Jira story end-to-end: branch, code, PR, and Jira status updates.

**Context to include:**
- [`skills/story-implementation/context/tech-stack.md`](skills/story-implementation/context/tech-stack.md) — stack, plugins, and development constraints

**How to invoke (Claude Code):**
```
/story-implementation

[Paste tech-stack.md content here]

Story: <Jira issue key or link, e.g. VLM-42>
```

**Output:** PR opened on GitHub, Jira story moved to In Review.

---

## GitHub Actions

Two workflows automate the post-merge delivery loop. No manual steps required after merging a PR.

### [`deploy.yml`](.github/workflows/deploy.yml) — Deploy to Production

Triggers on push to `main`. Connects to the production server via SSH and pulls the latest code.

Required secrets: `SSH_HOST`, `SSH_USER`, `SSH_PRIVATE_KEY`

### [`release-changelog.yml`](.github/workflows/release-changelog.yml) — Close Jira + Create Release

Triggers automatically after a successful deployment. For each merged PR it:

- Extracts the Jira issue key from the branch name
- Transitions the Jira story to Done
- Posts a deployment comment on the PR
- Creates a GitHub Release with auto-generated changelog

Required secrets: `JIRA_BASE_URL`, `JIRA_EMAIL`, `JIRA_API_TOKEN`

See [`.github/workflows/README.md`](.github/workflows/README.md) for full documentation.

> **Branch naming requirement:** Branches must include the Jira issue key, e.g. `VLM-2-add-login` or `feature/VLM-2-add-login`. The release workflow uses this to link deployments back to Jira.

---

## Context Files

Context files give Claude project-specific knowledge. Include them in the conversation when running a skill.

| File | Used by | Purpose |
|---|---|---|
| `skills/create-prd/context/company.md` | `create-prd` | Company profile, target audience, strategic priorities |
| `skills/prd-to-backlog/context/tech-stack.md` | `prd-to-backlog` | WordPress stack, plugins, and architectural constraints |
| `skills/story-implementation/context/tech-stack.md` | `story-implementation` | Same tech stack with implementation-specific guidance |
