---
name: post-review
description: Go over a PR review comments and address them.
---

# Post-review
Go to the PR of the branch you are on, make sure to understand what it is about and gather relevant context, then retrieve all the open comments on it using the `gh` CLI tool, and present each one by one to the user along with a suggestion on how to address it. At this point ask the user to tell you how they want to deal with it. Do not do any modification or run anything by yourself until the user told you what he wants to do with a comment, even if it seems obvious.

Once we went through all comments, ask the user if they want to push, answer and resolve. If he says yes, you should make a commit with all edits, push it and then answer the comments that were resolved with only "done in <commit_hash>" and resolve them. Do not write another answer, and importantly do not answer to any of the comments that were ignored or not resolved in this round, leave that to the user.