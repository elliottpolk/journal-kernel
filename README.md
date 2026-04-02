# journal-kernel

A second-brain template repo built on the [agentic kernel](https://github.com/elliottpolk/agentic-kernel). Use this as a starting point for your own AI-assisted journaling and knowledge management setup.

## What it is

This repo provides a pre-configured agentic kernel for personal and work documentation: daily work logs, configurable personal logs, initiative tracking, planning structures, and end-of-period summaries. Agents handle the repetitive parts (carryover, formatting, scaffolding) so you can focus on capturing what matters.

It is not a notes app. It is a structured, agent-assisted writing environment that lives in a git repo.

## Agents

| Agent | Does what |
|---|---|
| `work` | Creates daily work logs, carries over tasks, writes summaries, scaffolds initiatives |
| `personal` | Creates personal logs on a configurable daily or monthly cadence, carries over tasks, writes summaries, sets up new years |
| `kernel` | Answers questions about the system, governs changes to it |
| `agent-foundry` | Designs and scaffolds new agents when you want to extend the system |

## Workflows

The personal agent defaults to `monthly`. Set `settings.cadence` on the `personal` agent entry in `.agentic/manifest.yml` to `daily` if you want personal logs to use `YYYYMMDD.md` files instead.

| Workflow | Invocation | Does what |
|---|---|---|
| `spec` | `/spec` | Interactive feature spec creation |
| `scaffold` | `/scaffold` | Materializes a confirmed spec into files on a feature branch |
| `consolidate-work-month` | `/consolidate-work-month` | Moves daily work logs into a monthly subdirectory |

## Getting started

**Requires:** [akctl](https://github.com/elliottpolk/akctl). Install via Go:

```bash
go install github.com/elliottpolk/akctl/cmd/akctl@latest
```

Then initialize your repo:

```bash
akctl init --kernel.source github.com/elliottpolk/journal-kernel
```

Once initialized:

1. Fill in the `project:` block in `.agentic/manifest.yml` with your own details.
2. Create `.agentic/memories/state/project.md` to give agents context about you and your setup.
3. Bridge the kernel to your platform:
   - GitHub Copilot: use the `copilot-bridge` skill
   - Claude: use the `claude-bridge` skill
4. Start a log.

## Platform support

Agents and workflows are bridged to GitHub Copilot (`.github/`) and Claude (`.claude/`) via the bridge skills. All canonical definitions live in `.agentic/`. The platform files are thin wrappers that reference them.

## Built on

[elliottpolk/agentic-kernel](https://github.com/elliottpolk/agentic-kernel): the platform-agnostic foundation this repo extends.
