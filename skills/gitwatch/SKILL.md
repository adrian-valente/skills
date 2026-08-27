---
name: gitwatch
description: Track and summarize git commits affecting a specific folder or file over a time period. Use when the user wants to understand what changed in a path, review recent commits, or get a changelog-style summary. Triggers include "what changed in src/", "show commits affecting utils.py", "gitwatch the api folder", "summarize changes to X in the last week", or any request to track/watch/monitor git history for a specific path.
---

# Gitwatch

Analyze git history for a specific path and provide a clear summary of changes.

## Arguments

| Argument | Default | Description |
|----------|---------|-------------|
| path | (required) | File or folder path to watch |
| duration | 24h | Time window (e.g., 24h, 7d, 2w, 1m) |
| branch | origin/main | Branch to analyze |

## Workflow

### 1. Parse duration

Convert duration to git date format:
- `24h` / `1d` → `--since="24 hours ago"`
- `7d` / `1w` → `--since="7 days ago"`
- `2w` → `--since="14 days ago"`
- `1m` → `--since="1 month ago"`

### 2. Fetch commits

```bash
git log <branch> --since="<duration>" --oneline --stat -- <path>
```

For the commit list (hash, author, date, subject):
```bash
git log <branch> --since="<duration>" --format="%H|%an|%ad|%s" --date=short -- <path>
```

For aggregate stats only (total files / insertions / deletions across the window):
```bash
git log <branch> --since="<duration>" --shortstat -- <path>
```
Sum the files changed / insertions / deletions lines to populate the Summary line. Aggregate totals are "lines touched across files," not a net diff against the start of the window — a file edited in N commits counts N times.

### 3. Analyze and summarize

If no commits found, report that the path had no changes in the time window.

For per-commit stats in the output table, query each commit individually:
```bash
git show --shortstat --format="%H %an %ad %s" --date=short <hash> -- <path>
```
Do NOT pair `git log --shortstat` stat lines with commits by eye. The output interleaves commit headers, blank lines, and stat lines, and misattribution is easy and has happened. `git log --shortstat` is only safe for the aggregate sum (where per-commit attribution doesn't matter).

For diff details:
```bash
git show <hash> -- <path>  # if diff details needed
```

### 4. Output format

```
## Summary

[A small summary with bullet points describing the overall nature of changes: what areas were affected,
what types of changes occurred (features, fixes, refactors), and the general direction, followed by (M files, +X/-Y)]

## Commits

[If ≤10 commits: list all with brief descriptions]
[If >10 commits: list the most significant ones based on impact/scope, note "X additional minor commits omitted"]

| Commit |  PR number (if any) | Author | Date       | Summary                         |
|--------|---------------------|--------|------------|---------------------------------|
| abc123 |      #396         | Alice  | 2024-01-15 | Added validation for user input (2 files, +15/-0) |
| def456 |      #512         | Bob    | 2024-01-14 | Fixed null pointer in parser (1 file, +3/-1)    |
```

### Significance criteria (when >10 commits)

Select most significant commits based on:
- Commits with substantial line changes
- Commits affecting core functionality vs peripheral files
- Commits with meaningful feature/fix descriptions vs minor tweaks
- Merge commits summarizing larger changes

Omit: typo fixes, formatting-only changes, minor refactors, dependency bumps.

