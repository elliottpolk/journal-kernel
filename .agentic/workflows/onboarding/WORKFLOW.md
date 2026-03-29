---
name: onboarding
description: >-
  First-time setup for a new journal-kernel repo. Collects project identity,
  creates the project memory file, bridges agents and workflows to the user's
  AI platform, scaffolds the journal directory structure for the current year,
  and walks the user through what they now have.
invocation: /onboarding
agent: kernel
---

# onboarding

Guides a new adopter through first-time setup of their journal-kernel repo.
Run once after initializing from the journal-kernel template. Nothing is written without confirmation.

## Phase 1: Project Identity

Read the `project:` block in `.agentic/manifest.yml`.

- If all required fields (`name`, `description`, `author`, `repository`) are non-empty: confirm
  the values with the user and proceed.
- If any required field is empty: ask the user for the missing values one at a time, write them
  into the `project:` block, and confirm before proceeding.

## Phase 2: Project Memory

Create .agentic/memories/state/project.md with the following structure:

Last updated: {YYYY-MM-DD}

## Identity

- **Name**: {name}
- **Description**: {description}
- **Author**: {author}
- **Organization**: {organization}
- **Repository**: {repository}

## Purpose

{A 2-3 sentence description of what this second brain is for, derived from
the description and any additional context the user provided above.
Write this for a future agent reading it cold.}

## Notes

Ask the user to confirm before writing the file.

## Phase 3: Platform Bridging

Ask: Which AI platform(s) are you using? (GitHub Copilot, Claude, or both)

For each selected platform:

- **GitHub Copilot**: load and follow .agentic/skills/copilot-bridge/SKILL.md.
  Bridge all agents, skills, and workflows registered in manifest.yml.
- **Claude**: load and follow .agentic/skills/claude-bridge/SKILL.md.
  Bridge all agents, skills, and workflows registered in manifest.yml.

Confirm each output path as it is created. Do not proceed to Phase 4 until all
selected bridges are complete.

## Phase 4: Journal Directory Setup

Ask: What year do you want to set up? (default: current year)

Then scaffold the year structure by invoking:

1. The personal agent in **New Year Setup** mode for personal/{YYYY}/.
2. The work agent in **New Year Setup** mode for work/{YYYY}/.

Each agent handles its own template substitution and directory creation per
its IDENTITY.md. Confirm each directory was created before proceeding.

## Phase 5: Guided Tour

Once all phases are complete, present the following summary. Do not narrate what
just happened -- orient the user to what they now have and how to use it.

---

**Your second brain is ready.**

Here is what you have:

**Agents** (invoke by name in your AI platform):

| Agent | What it does |
|---|---|
| work | Start a daily work log, carry over tasks, summarize your day |
| personal | Start a monthly personal log, carry over tasks, summarize your month |
| kernel | Ask about the system, make changes to it, extend it |
| agent-foundry | Design and scaffold new agents |

**Workflows** (invoke by slash command):

| Command | What it does |
|---|---|
| /onboarding | This workflow. Run again if you add a new platform or need to re-bridge. |
| /spec | Write a feature spec interactively |
| /scaffold | Materialize a spec into files on a branch |
| /consolidate-work-month | Move last month's daily logs into a monthly subdirectory |

**Where things live:**

- work/{YYYY}/ -- daily work logs
- personal/{YYYY}/ -- monthly personal logs
- .agentic/ -- the kernel: agents, workflows, skills, memory

**Suggested next step:** Ask the work agent to start today's log, or ask personal to start this month's.

---

## Rules

- MUST verify manifest.yml project: block is complete before writing any file.
- MUST confirm project.md content with the user before writing.
- MUST complete bridging for all selected platforms before Phase 4.
- Do not skip phases. Each phase depends on the previous one being confirmed.
