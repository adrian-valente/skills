---
name: pr-summary
description: Generate comprehensive summaries of pull request changes by comparing the current branch to main, or by analyzing an existing PR by number. Use when the user asks to summarize changes, preview what's in a PR, or understand what work has been done. Triggers include "summarize my changes", "what's in this PR?", "give me a PR summary", "summarize PR 123", "what did I change on this branch?", or any request with a PR number like "#456" or "PR 789".
---

# PR Summary

Generate a comprehensive, well-structured summary of changes for a pull request, either from the current branch or from an existing PR by number.

## Workflow

Follow these steps to create an effective PR summary:

### 1. Determine Summary Mode

**If user provides a PR number** (e.g., "summarize PR 123", "#456"):
- Use GitHub CLI to fetch the PR information
- Proceed to step 2 using PR-specific commands

**If user requests current branch summary:**
- Use local git commands
- Proceed to step 2 using local commands

### 2. Gather Context

**For existing PR (with PR number):**

```bash
# Get PR metadata and description
gh pr view <PR_NUMBER>

# Get PR diff
gh pr diff <PR_NUMBER>

# Get list of commits
gh pr view <PR_NUMBER> --json commits --jq '.commits[].messageHeadline'

# Get files changed
gh pr diff <PR_NUMBER> --name-only
```

**For current branch:**

```bash
# Determine scope first
# If on main: use `git show HEAD` and `git log -1`
# If on feature branch: use commands below

# Get branch status
git status

# Get commit history for the branch
git log main..HEAD --oneline

# Get detailed diff
git diff main...HEAD

# Identify files changed
git diff main...HEAD --name-status
```

### 3. Analyze Changes

Review the diff output to understand:

- **Scope**: What areas of the codebase are affected?
- **Purpose**: What is the primary goal of these changes?
- **Impact**: What functionality is added, modified, or removed?
- **Patterns**: Are there common themes across the changes?

### 4. Generate Structured Summary

Create a summary with the following sections:

**## PR Metadata** (only for existing PRs)
- PR number and title
- Author
- Status (open/closed/merged)
- Target branch

**## Summary**
- 2-4 bullet points capturing the essence of the changes
- Focus on what and why, not how
- Use clear, concise language

**## Changes by Category**

Organize changes into logical categories, such as:
- **Features**: New functionality added
- **Bug Fixes**: Issues resolved
- **Refactoring**: Code improvements without behavior changes
- **Tests**: Test additions or modifications
- **Documentation**: Documentation updates
- **Infrastructure**: Build, CI/CD, or tooling changes

For each category, list specific changes with file references where helpful.

**## Files Changed**

Provide a high-level overview:
- Total number of files changed
- Key files modified (highlight the most significant ones)
- Any new files created or files deleted

**## Notes** (optional)

Include any additional context:
- Breaking changes
- Migration steps needed
- Follow-up work required
- Dependencies or related PRs

### 5. Format for Clarity

- Use markdown formatting for readability
- Include code references in the format `file_path:line_number` where relevant
- Keep bullet points concise but informative
- Use appropriate heading levels (##, ###)

## Example Output Structure

**For existing PR:**

```markdown
## PR Metadata
- **PR**: #123 - Add user authentication
- **Author**: @developer
- **Status**: Open
- **Target**: main

## Summary
- Implement user authentication flow with JWT tokens
- Add login and registration endpoints
- Include password hashing and validation

## Changes by Category

### Features
- Add JWT authentication middleware (src/middleware/auth.ts:15)
- Implement login endpoint with email/password validation (src/routes/auth.ts:42)
- Add user registration with password hashing (src/routes/auth.ts:89)

### Tests
- Add authentication middleware tests (tests/auth.test.ts)
- Add login/registration endpoint tests (tests/routes/auth.test.ts)

### Infrastructure
- Add jsonwebtoken and bcrypt dependencies (package.json)

## Files Changed
- 8 files modified
- 3 new files created
- Key files: src/middleware/auth.ts, src/routes/auth.ts, tests/auth.test.ts

## Notes
- Breaking change: All protected routes now require JWT authentication
- Migration: Existing sessions will need to be invalidated
```

**For current branch:**
(Same structure but omit the "PR Metadata" section)

## Best Practices

- **Be concise**: Aim for clarity over comprehensiveness
- **Focus on impact**: Emphasize what changes mean for users/developers
- **Group logically**: Organize changes by functionality, not by file
- **Highlight important details**: Call out breaking changes, migrations, or follow-ups
- **Use active voice**: "Add feature X" not "Feature X was added"
