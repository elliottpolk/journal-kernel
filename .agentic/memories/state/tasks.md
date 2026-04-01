# Task Backlog

Deferred tasks captured across agent sessions. Managed by the `task-backlog` skill.

---

## Open

- [ ] Generalize the personal agent to support configurable journal cadence (daily or monthly) via the manifest
  - agent: kernel
  - created: 20260401
  - source: User session — reviewing personal agent IDENTITY.md
  - context: The personal agent is currently hardcoded to monthly logs at `personal/{YYYY}/{YYYYMM}.md`. The goal is to make the journal cadence (daily vs. monthly) a configurable setting in `manifest.yml` under the agent's entry (e.g., a `settings.cadence` field). A daily cadence would mirror the pattern used by the work agent (`YYYYMMDD.md`), while monthly would remain the current behavior. The agent's IDENTITY.md, file conventions, mode inference, and template selection should all respond to this setting.

## Done

## Dropped
