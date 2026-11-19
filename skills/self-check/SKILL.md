---
name: self-check
description: Check the whole ticket before opening a PR - tests, an independent review by subagent, a run on the device, and each acceptance criterion
argument-hint: [plan-path]
disable-model-invocation: true
---

Step 4 of the workflow. Verify everything you can yourself; ask the Dev only for what you cannot do, and say why. Change nothing in this skill except the report file.

Language: reply in the language the Dev writes in, and fill in templates and reports in that language unless the Dev asks otherwise.

Plan: $ARGUMENTS (if empty, find the plan in `plans/` whose name starts with the ticket key in the branch name; if none or several match, list `plans/` and ask the Dev; a small ticket has no plan)

## 1. Scope

- Look at the branch diff against the base branch (work it out from git; ask only if you cannot tell): code ticket, documentation ticket, or both.
- **First run** on a ticket (no `plans/<KEY>-self-check.md` yet): check everything. **Later runs:** read that report; check only what changed since the commit it records and the issues it reported. Do not start over.

## 2. Checks

1. **Code ticket:** run lint, build and the full unit test suite from `CLAUDE.md`, each as its own command, reading results and baselines as the implement skill says.
2. **Independent review by subagent** (fresh context; give it only the plan path and read access to the repo, none of your notes):
   - Code ticket: which deliverables and AC are missing from the diff; changes outside the plan; logic errors.
   - Documentation ticket: statements that are wrong or broader than the code supports; contradictions between files; behaviour claims without a label (device / code / not verified). Re-check `file:line` references and re-run searches for the statements the reader will rely on; not every line.
   - Do not change the working tree, including temporary test code, while a subagent is reviewing.
   - Do not dismiss a subagent finding; report it as is. If you re-checked and the subagent is wrong, put your evidence next to it and let the Dev decide.
3. **Device:** `adb devices`. If a device is connected, install the debug build and run every scenario in the plan's checks plus every "not verified" item that can be run; save screenshots to `plans/screenshots/`. Follow the device rules in the implement skill.
4. **Secrets:** search the diff for keys, tokens, passwords and user data.

## 3. Report

Order by impact, not as a flat list:

```
Ticket type, run number: <...>
Checked at: commit <git rev-parse HEAD> · uncommitted: <files from git status --porcelain, or "none">
Lint / build / unit tests: <result> or "n/a: <reason>"

Acceptance criteria:
  AC1 <verbatim> → met / not met / needs someone else — <evidence or what is missing>

Must fix (blocks an AC, or wrong content the reader will rely on): <list>
Should fix (contradictions, outside the plan): <list>
Minor (line numbers off, wording): <count only>

On device: <scenario → reproduced / not reproduced / could not run, screenshots>
Secrets: <...>
Needs the Dev: <what — why I could not do it myself>
```

Save the report to `plans/<KEY>-self-check.md`, replacing the previous one (no ticket key: use the branch name). It is git-ignored; `/beta-claude-kit:pr` and the next run read it.

## 4. When to stop

An AC is "met" only when the behaviour itself was exercised in this run; seeing a label or a request go out is not enough. Mark an AC "needs someone else" only when neither you nor the Dev can verify it by any available means, including test configuration, and name who will do it and when. A task ticked with an unmet done condition counts as not met.

The ticket is ready for the Dev to open a PR when "Must fix" is empty and every AC is "met" or "needs someone else". The "Minor" list does not have to reach zero. State this conclusion at the end of the report.
