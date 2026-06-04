# Memory Field Guide

How agents write, update, and organize memory files. Apply this before writing anything to `.agentic/memories/`.

## Platform Memory Hierarchy

Some platforms provide a vendor-specific memory store (e.g. GitHub Copilot's `/memories/` tool, Claude's memory). These are supplementary. The canonical stores are:

- **Shared:** `.agentic/memories/` (state and history)
- **Agent-specific:** `.agentic/agents/{name}/memories/`

**Reads:** Read from the appropriate `.agentic/` location first. Vendor memory MAY be read as a supplement. If the two conflict, `.agentic/` is ground truth.

**Writes:** Write to the appropriate `.agentic/` location. If the platform supports it and there is no cost to writing both, write to both; the vendor store can improve in-session recall on that platform. It MUST NOT substitute for either canonical store. If only one write is possible, write to the `.agentic/` location.

**What belongs in vendor memory (when used):** Short-form session aids, user preference hints, frequently reloaded facts. No decision records, no history, no state that affects other agents.

**What never belongs in vendor memory alone:** Decisions, history entries, org or people facts, initiative state, anything another agent or a future session would need.

## The Two Buckets

| Bucket | Path | Rule |
|---|---|---|
| State | `.agentic/memories/state/` | Mutable reference facts. Update in place when reality changes. |
| History | `.agentic/memories/history/` | Append-only narrative. Never edit past entries. |

## State: When and What to Write

Write to `state/` when you learn a fact that would be useful to **any** future agent session:

- Who someone is, what their role is, and how to engage them effectively
- Org structure, team boundaries, governance bodies
- Decisions that are now settled (not pending)
- System facts: what a tool does, how a process works, what the constraints are

Do **not** write to `state/` when:

- The fact is a pattern specific to how one agent reasons or has been corrected (write to that agent's `memories/` directory instead)
- The content is intended for a human to read and act on (write to a human-facing doc like a README or brief)
- The fact is ephemeral or only meaningful within a single session

## History: When and What to Write

Append to `history/` when:

- A decision was made and the reasoning should be preserved
- Something was tried and failed in a non-obvious way
- The current state came to be through a path a future agent would need to understand

Never modify past history entries. Append only. Use a date header for each entry:

```markdown
## 2026-03-27

What was decided and why. What was tried. What the open items are.
```

## File Management

### Naming Conventions

| Subject type | Convention | Example |
|---|---|---|
| Person | `{first-last}.md` | `ahmed-silwadi.md` |
| Org, team, or system | Short descriptive slug | `ets-operating-model.md` |
| Governance body | Short name slug | `garb.md` |
| History (monthly) | `{YYYY-MM}.md` | `2026-03.md` |
| History (topic thread) | `{topic-slug}.md` | `scm-initiative.md` |

### One file per entity, not per session

Do not create a new file for each interaction with a person or system. Add to the existing file. Create a new file only when the subject is genuinely distinct from everything that exists.

### Updating state files

State files reflect **current reality only**. When reality changes:

- Replace the outdated content in place
- Update the `Last updated:` field at the top
- Do not append a changelog of old values. History files serve that purpose.

## General vs. Agent-Specific Memory

Agent-specific memory (`.agentic/agents/{name}/memories/`) is for how that agent has been shaped over time: corrections, confirmed preferences, agent-scoped patterns. It is not for domain facts that any agent would need. See BEHAVIOR.md for the decision rule.

## Pattern Signals

A pattern signal is a lightweight agent-specific memory that captures a reusable shape the agent may need to recognize again later.

The key property is the pattern, not the topic. A pattern signal records structure, flow, pairing, framing, or another repeatable form that may recur across future work.

Pattern signals have a lifecycle:

- `candidate`: an initial signal worth watching
- `corroborated`: later work shows meaningful structural overlap with the candidate
- `promotable`: repeated overlap is now strong enough that a reusable template, workflow, or other formalization may be justified

### What to capture

When capturing a pattern signal:

- Write it to the relevant agent's `memories/` directory, not shared state, unless the pattern is truly useful to multiple agents.
- Keep it short. Capture only the minimum needed to recover the pattern later.
- Record what the agent should recognize, what aspect matters, and one or more anchor examples.
- Prefer naming the structural cue over naming the current subject matter.
- Capture the pattern as a `candidate` first, not as an established reusable method.
- A user-declared candidate pattern is sufficient reason to capture the memory, even if recurrence is not yet proven.
- When the user is the source of the signal, preserve that provenance briefly.
- Do not turn the memory into instructions, a workflow, or a policy unless the pattern has already proven stable.

### How a candidate becomes a pattern

Agents should watch for corroborating instances during normal work. The important question is not whether the next artifact has the same content. The question is whether the same underlying framing sequence, structure, or flow is showing up again.

Outputs may vary by topic, depth, or final artifact shape. Structural overlap is what matters.

### How to recognize a possible pattern

Agents should check for pattern-signal matches during normal work at natural checkpoints, for example when routing, framing, summarizing, or shaping a new artifact.

Do not run broad memory scans just to hunt for patterns. Recognition should happen opportunistically when the current work already resembles something stored in agent-specific memory.

When evaluating a possible match:

- Compare the current work against the stored structural cue, not only the topic.
- Treat one later overlap as corroboration, not as proof that a reusable method exists.
- Look for multiple instances of similar overlap before recommending extraction into a template or workflow.

### How to surface a recognized pattern

When a current thread appears to match a stored pattern signal:

- Surface it to the user as a possibility, not a fact.
- Name the remembered pattern briefly and explain the match in terms of structure or flow.
- Reference the anchor example when that would help the user evaluate the match quickly.
- Treat the memory as a nudge toward recognition, not an automatic decision to restructure the work.

When the pattern has moved beyond a single corroborating instance:

- Say that repeated structural overlap may now indicate an emerging reusable method.
- Call out that the next step may be to extract a template, workflow, or other formal support.
- Keep the recommendation proportional to the evidence. Suggest promotion only when the overlap appears stable enough to justify it.

### Threshold for surfacing

Use a conservative threshold to avoid false positives:

- `capture threshold`: low. A credible structural cue or explicit user declaration is enough to store a candidate pattern.
- `recognition threshold`: medium. Require meaningful structural overlap before mentioning a possible match.
- `promotion threshold`: high. Do not recommend a template or workflow from a single example or two closely related artifacts in the same thread.
- Do not surface a pattern based on topical similarity alone.
- Require a clear structural match to the stored pattern.
- Prefer at least one concrete anchor example in memory before treating the pattern as reusable.
- Prefer at least three structurally similar instances, or equivalent confidence across independent examples, before recommending formalization into a template or workflow.
- If the resemblance is weak, stay silent.
- Prefer missing a weak pattern over interrupting the user with a noisy one.

## Anti-Patterns

- **Writing futures as facts**: Do not write "will" or "plans to" as settled state. Use "as of {date}: pending" or record it in history.
- **Behavioral rules in state**: If it describes how an agent should act, it belongs in IDENTITY.md or BEHAVIOR.md, not state memory.
- **Human-addressed content**: If you would write "you should know..." or structure it as a briefing, it is not agent memory. Write it elsewhere.
- **Over-granularity**: One file per person or system is correct. One file per meeting or interaction is not.
- **Stale orphans**: If a person leaves, a project ends, or a system is decommissioned, mark the file inactive or delete it. Do not let dead state persist.
- **Mixing state and history in one file**: If you need to record both current facts and how they came to be, split them: update the state file and append to a history file.
