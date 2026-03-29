---
name: consolidate-work-month
description: >-
  Consolidate all daily work log entries from the previous month into the monthly
  subdirectory. Moves YYYYMMDD.md files from work/{YYYY}/ into work/{YYYY}/{MM}/.
  Bridgeable as a /consolidate-work-month prompt in GitHub Copilot.
invocation: /consolidate-work-month
agent: work
---

# consolidate-work-month

Moves all daily work log files for the previous month from the root year directory
into a monthly subdirectory. No file content is modified during consolidation.

## Phase 1: Determine Target Month

1. Use the current date to identify the previous month.
2. Derive:
   - `{YYYY}`: year of the previous month
   - `{MM}`: two-digit month of the previous month
3. Source path: `work/{YYYY}/`
4. Target path: `work/{YYYY}/{MM}/`

## Phase 2: Identify Source Files

1. Scan `work/{YYYY}/` for files matching `{YYYY}{MM}DD.md` where `{MM}` matches the target month.
2. If no matching files are found, report: "No work log files found for {YYYY}-{MM}." Stop.
3. If the target directory already exists and contains files, warn:
   "Monthly directory {YYYY}/{MM}/ already exists with {count} files. Proceed?"
   Wait for confirmation before continuing.
4. If running on the 1st or 2nd of the new month, warn that consolidation may be premature
   and ask the user to confirm before proceeding.

## Phase 3: Create Target Directory

1. Create `work/{YYYY}/{MM}/` if it does not exist.

## Phase 4: Move Files

1. Move each source file from `work/{YYYY}/{YYYY}{MM}DD.md` to `work/{YYYY}/{MM}/{YYYY}{MM}DD.md`.
2. Preserve file contents exactly. Do not modify any content.
3. If a file already exists at the destination, skip it and record a warning.

## Phase 5: Validate

1. Confirm each source file no longer exists at the root year path.
2. Confirm each file is present in the target monthly directory.
3. Record any files that could not be moved.

## Phase 6: Report

Output a summary:

- Total files consolidated: {count}
- Source: `work/{YYYY}/`
- Destination: `work/{YYYY}/{MM}/`
- Date range: {first_date} to {last_date}
- Warnings or errors (if any)

## Edge Cases

- **Cross-year boundary**: If the previous month is December and the current year is a new year,
  use the prior year for both the source scan and target directory.
  Example: current date 2026-01-05 consolidates `work/2025/202512*.md` into `work/2025/12/`.
- **No files**: Do not create the target directory if there are no files to move.
- **Partial month**: Warn when running on the 1st or 2nd of the new month; do not auto-proceed.
- **Already consolidated**: If files already exist in the destination, warn and skip duplicates.

## Future State

- After consolidation, generate a monthly summary record in `work/{YYYY}/{MM}/` that
  aggregates key entries, completed TODOs, and carried-over deferred items across all daily logs.
