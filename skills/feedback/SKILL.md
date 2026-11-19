---
name: feedback
description: Turn something a skill got wrong into a pull request on beta-kit, without leaving the app repo
argument-hint: [what went wrong]
disable-model-invocation: true
---

Run after a ticket, or whenever a skill let you do the wrong thing. Sends one fix to the kit (`cuongnguyen-dev/beta-kit`) as a pull request from the Dev's fork. You prepare the change; the Dev pushes and opens the PR.

Language: reply in the language the Dev writes in. Write the skill change, the commit message and the pull request in English, the language of the kit.

Problem: $ARGUMENTS

## 1. Pick the problem

1. If no problem was given, list what went wrong or had to be redone in this session, sorted into three groups: true for every project, true for this project only, happened once. Only the first group goes to the kit. Offer the second group as entries for "Lessons" in this repo's `CLAUDE.md`; drop the third.
2. One problem per pull request. If there are several, ask the Dev which one to send first.
3. State which skill it was, what the skill said and what you did instead. Stop and wait for the Dev to confirm it is true for every project.

## 2. Prepare the change

1. The kit clone lives in `~/.beta-kit-fork`, outside the app repo. If it does not exist, create it:

   ```bash
   gh repo fork cuongnguyen-dev/beta-kit --clone=false
   gh repo clone "$(gh api user -q .login)/beta-kit" ~/.beta-kit-fork
   ```

   If `gh` is missing or not signed in, tell the Dev and stop.
2. Start from the kit's current `master`: `git -C ~/.beta-kit-fork fetch upstream`, then `git -C ~/.beta-kit-fork switch -c fix/<short-name> upstream/master`. If the clone has uncommitted changes, stop and ask the Dev.
3. Read `CONTRIBUTING.md` in the clone and follow "Rules for a skill change". Edit the files in the clone, never the installed copy under `~/.claude/plugins/`.
4. Run `claude plugin validate ~/.beta-kit-fork`.
5. Show the Dev the diff. When the Dev approves, commit it in the clone.

## 3. Write the pull request description

Fill in `.github/pull_request_template.md` from the clone and save the result to a temporary file outside both repos.

- The kit repo is public. Leave out the app's name, code, class names, ticket text and user data; describe the situation in general terms.
- "Tried on": write "Not tried" and why, unless the Dev re-ran the scenario with `claude --plugin-dir ~/.beta-kit-fork`.
- Tick only boxes you have evidence for.

## 4. Hand over

Print the description, then one command for the Dev to run with the `!` prefix:

```bash
git -C ~/.beta-kit-fork push -u origin fix/<short-name> && gh pr create --repo cuongnguyen-dev/beta-kit --base master --head <login>:fix/<short-name> --title "<title>" --body-file <description file>
```

Do not push or create the pull request yourself. Tell the Dev the fix reaches everyone after the kit owner merges it and they run the update commands.
