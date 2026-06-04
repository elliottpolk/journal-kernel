---
name: em-dash-check
description: "Fast Unicode dash audit for prose files. Scans a target file or directory for em dash and en dash characters, rewrites affected prose without dash substitution, and reruns the check to confirm cleanup."
invocation: /em-dash
---

# em-dash-check

A fast workflow for catching and removing Unicode em dash and en dash characters from prose. Use it when a model-produced document starts leaning on dash-heavy sentence structure.

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

## Phase 2: Run the Dash Scan

1. Use the platform's native workspace search or direct file reads to find Unicode em dash and en dash characters.
2. Prefer a Unicode-aware search that can report file and line locations without depending on locally installed shell tools.
3. Search for code points `U+2014` and `U+2013`.
4. Record matches as `file:line:content` when line-oriented results are available. Otherwise record the file path and a short sentence excerpt.
5. If no matches are found, report that the target is clean and stop.

## Phase 3: Classify Matches

For each match, determine whether it is prose that should be cleaned up or a literal that should be preserved.

Treat these as literal by default:

- fenced code blocks
- inline code
- regex examples
- quoted source text
- typography documentation that is explicitly discussing dash characters

If a match is ambiguous, ask before editing it.

## Phase 4: Clean Up Prose Matches

For each prose match:

1. Rewrite the full sentence or clause for clarity.
2. Prefer one of these fixes:
	- split the sentence with a period
	- replace the pause with a comma
	- use a colon to introduce the next idea
	- use parentheses for a brief aside
	- use a semicolon only when two independent clauses genuinely need it
3. Preserve the original meaning and tone.
4. Do not replace the character with a hyphen or repeated hyphen characters.
5. If the cleanup would materially change quoted text, stop and ask.

## Phase 5: Validate

1. Rerun the same Unicode dash scan against all touched files.
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
- MUST use whole-sentence or whole-clause rewrites for cleanup.
- MUST ask before altering quoted source text or intentional typography examples.
- MUST rerun the scan after edits.
- Do not perform mechanical punctuation swaps that weaken clarity.
- Do not substitute a Unicode dash with a hyphen.
