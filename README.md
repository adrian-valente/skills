# Skills

Some personal skills I use

## Available Skills

### [gitwatch](./skills/gitwatch/SKILL.md)

```
/gitwatch path/to/folder 1 week [origin/main]
```

Will fetch all commits on the requested folder for the requested duration on origin/main (or other branch), and write a quick summary of what the main changes are, and if there aren't too many commits a small summary of what each of them did.

**Arguments:**
- `path` (required): File or folder path to watch
- `duration`: Time window (default: 24h, supports: 24h, 7d, 2w, 1m)
- `branch`: Branch to analyze (default: origin/main)

### [pr-summary](./skills/pr-summary/SKILL.md)

```
/pr-summary #375
```

Generate comprehensive summaries of pull request changes by comparing the current branch to main, or by analyzing an existing PR by number. You can also ask to generate an ASCII diagram of the architecture introduced by the PR!

### [merge-main](./skills/merge-main/SKILL.md)

```
/merge-main
```

Merge `origin/main` into the current branch. Identifies the PR the branch belongs to, understands its aims, then fetches main and resolves conflicts while preserving the PR's changes and avoiding regressions from the merged commits. Runs tests, commits, and pushes.

### [post-review](./skills/post-review/SKILL.md)

```
/post-review
```

Walks through the open review comments on the current branch's PR one by one, presenting each with a suggested fix and asking how you want to handle it. Once all are triaged, commits the chosen edits, pushes, and answers + resolves the addressed comments.

