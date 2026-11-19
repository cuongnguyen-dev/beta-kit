---
name: pr
description: Write the pull request description from the plan and the latest self-check result
argument-hint: [plan-path]
disable-model-invocation: true
---

Step 5 of the workflow. The Dev opens the PR; do not push or create a PR.

Language: reply in the language the Dev writes in, and fill in templates and reports in that language unless the Dev asks otherwise.

Plan: $ARGUMENTS (if empty, find the plan in `plans/` whose name starts with the ticket key in the branch name; if none or several match, list `plans/` and ask the Dev; a small ticket has no plan)

1. Take results from the self-check report, `plans/<KEY>-self-check.md`. Compare it with the code now (`git diff --stat <commit it records>`, `git status --porcelain`). If the report is missing, or files have changed other than `plans/` and the uncommitted files it lists, ask the Dev to run `/self-check` first and stop; do not fill results in from memory.
2. Fill in [pr-template.md](pr-template.md). Write "Main changes" for a reviewer who does not know this project. Where a section does not apply, write "n/a" with the reason.
3. Tick only boxes you have evidence for. Always leave "I have read and understood the whole diff" for the Dev.
4. List the files in `plans/screenshots/` the Dev should attach to the PR; they are not in git.
5. Print the description for the Dev to copy. Remind the Dev to re-read the diff, attach screenshots, link the Jira ticket and move its status.
