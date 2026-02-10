# Skills

A collection of Claude Code skills for common development workflows.

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

