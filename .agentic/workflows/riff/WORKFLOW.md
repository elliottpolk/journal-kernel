---
name: riff
description: >-
  Switches the conversation into discussion-first ideation mode. Preserves the current
  agent context, explores ideas as a researcher and domain expert, and defers edits or
  artifact creation until the user explicitly asks to capture something.
invocation: /riff
---

# riff

Lightweight interaction-mode workflow for thinking out loud with the current agent.

Use it when the user wants to explore, compare, pressure-test, or shape an idea without
turning the conversation into planning or execution.

## Inputs

- Optional topic, problem space, decision, or question to explore.
- Optional constraints, such as audience, time horizon, tradeoffs, or non-goals.

## Phase 1: Enter Riff Mode

1. Resolve the topic from the invocation argument or the latest user message.
2. If the topic is still unclear, ask one short question to identify what to riff on.
3. Treat the interaction as discussion-first ideation, not planning or execution.
4. Preserve the current agent and domain context. This workflow changes interaction mode only.
5. Do not edit files, create files, scaffold content, or draft durable artifacts unless the user gives an explicit capture signal.
6. If a topic is already clear, open with 2 to 4 angles, tensions, or lenses worth exploring rather than jumping straight to a recommendation.

## Phase 2: Run The Ideation Loop

1. Stay in researcher and expert mode.
2. Help the user think by doing one or more of the following in small chunks:
   - ask a sharp clarifying question
   - compare options or frames
   - surface assumptions, risks, and tradeoffs
   - connect the topic to adjacent patterns, examples, or precedent
   - challenge weak framing when a stronger interpretation is available
3. Keep responses concise and easy to steer.
4. Do not silently convert the discussion into a plan, task list, workflow, or document outline unless the user asks for that shift.
5. When useful, synthesize what seems to be emerging, but keep the synthesis provisional until the user confirms it.

## Phase 3: Capture Only On Explicit Signal

1. Treat phrases such as "let's capture that", "capture that", "write that up", "turn that into a note", or another clear equivalent as a capture signal.
2. Until a capture signal appears, remain in riff mode and do not materialize artifacts.
3. When a capture signal appears:
   - if the intended artifact and destination are clear, create it
   - if the destination is ambiguous, ask the smallest clarifying question needed before writing anything
4. After the capture step is complete, ask whether to return to riff mode or continue in normal execution mode.

## Rules

- MUST preserve the active agent context unless the user explicitly asks to switch agents.
- MUST NOT make edits or create artifacts before an explicit capture signal.
- MUST keep the conversation exploratory rather than prematurely converging on execution.
- SHOULD pressure-test ideas when the user is still shaping them.
- SHOULD prefer short, chunked responses over long monologues.