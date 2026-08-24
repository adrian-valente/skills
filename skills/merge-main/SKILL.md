---
name: merge-main
description: Merge main into the current branch while respecting some rules.
---

# Merge main
You are on a branch and want to merge main into it. The first step is to identify to what PR this branch belongs. For this you can use `gh pr view`, for example `gh pr view --json number,title,baseRefName,body,changedFiles` (but feel free to ask for more information). If the base is not main, you should ask the user for directions. Otherwise, continue by checking the full diff with `gh pr diff`. Make sure to understand exactly what the PR's aims are, if you are unsure feel free to explore the files until the changes are well understood.

Then, fetch `origin/main` and start the merge in the current branch. You should solve conflicts while preserving the changes that are being introduced by the PR. Make sure you understand the spirit of the PRs merged in main that triggered the conflicts and to not introduce regressions with respect to them as well. If you are unsure or there is ambiguity about a given conflict, ask the user for directions. Once the merge is complete, run the relevant tests if they are not too long, commit and push to origin. Make sure that there are no hidden conflicts with main, for example through a reference to something that has been deleted or moved. Make sure you don't reintroduce something that was removed in main.