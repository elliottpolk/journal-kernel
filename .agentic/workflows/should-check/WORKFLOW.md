---
name: should-check
description: "Finds vague uses of 'should' in prose, rewrites them using the NEED/WANT/MAY decision model from .agentic/core/DECISIONS.md, and reruns the scan to confirm cleanup."
invocation: /should
---

# should-check

A fast workflow for catching vague uses of "should" in prose and rewriting them into accountable decision language. Use it when notes, prompts, plans, or framing docs start drifting into passive recommendations instead of explicit ownership and priority.

## Inputs

- Target path: optional file or directory path.
- Default target selection:
  1. user-provided path
  2. active file
  3. workspace root
- Mode: `check-only` or `check-and-clean`
  - Default: `check-and-clean`

## Phase 1: Resolve Scope

1. Determine the target path using the input rules above.
2. If the target path does not exist, stop and report the missing path.
3. If the target is a directory, limit the scan to text files. Ignore binary files.
4. Report the resolved scope before scanning.

## Phase 2: Run the Should Scan

1. Read `.agentic/core/DECISIONS.md` before proposing any cleanup.
2. Use the platform's native workspace search or direct file reads to find standalone uses of `should`.
3. Prefer a case-insensitive, whole-word search so `should` is caught but unrelated substrings are not.
4. Record matches as `file:line:content` when line-oriented results are available. Otherwise record the file path and a short sentence excerpt.
5. If no matches are found, report that the target is clean and stop.

## Phase 3: Classify Matches

For each match, determine whether it is prose that should be cleaned up or a literal that should be preserved.

Treat these as literal by default:

- fenced code blocks
- inline code
- quoted source text
- examples that are explicitly teaching the NEED/WANT/MAY rule or discussing the word `should` itself
- file names, command names, or identifiers that intentionally contain `should`

If a match is ambiguous, ask before editing it.

## Phase 4: Clean Up Prose Matches

For each prose match:

1. Rewrite the full sentence or clause for clarity.
2. Use `.agentic/core/DECISIONS.md` to decide whether the statement is expressing:
	- `NEED` or `MUST` for a non-negotiable requirement or constraint
	- `WANT` or `PREFER` for a desired outcome with an explicit owner
	- `MAY` or `CAN` for an optional choice owned by the receiver
3. Name the owner whenever the original sentence leaves ownership implicit.
4. State the consequence, trade-off, or condition when the sentence is making a decision or recommendation.
5. Preserve the original meaning and tone while removing passive ambiguity.
6. Do not perform a mechanical one-word substitution. Rewrite enough of the sentence to make accountability and priority explicit.
7. If the cleanup would materially change quoted text or a source-faithful excerpt, stop and ask.

## Phase 5: Validate

1. Rerun the same standalone `should` scan against all touched files.
2. If prose matches remain, continue cleanup until either:
	- the file is clean
	- only literal matches remain
	- an ambiguous case needs user input
3. Report the final state:
	- files scanned
	- files changed
	- remaining literal matches
	- blocked ambiguous matches

## Rules

- MUST default to the active file when no path is provided and an active file exists.
- MUST read `.agentic/core/DECISIONS.md` before rewriting any match.
- MUST rewrite toward explicit NEED, WANT, or MAY language instead of preserving `should`.
- MUST ask before altering quoted source text or examples that intentionally discuss the word `should`.
- MUST rerun the scan after edits.
- Do not replace `should` with a different vague hedge such as `probably`, `ideally`, or `hopefully`.
- Do not force every sentence into `MUST`; preserve the original level of commitment.