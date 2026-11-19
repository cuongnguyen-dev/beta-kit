# beta-claude-kit: setup and usage

## Install (once per app repo)

1. Install the plugin on your machine: see [README.md](README.md).
2. Copy [templates/settings.json](templates/settings.json) to `.claude/settings.json` in the app repo and commit it. If the repo already has that file, merge the `deny`, `ask` and `allow` lists and the two plugin keys into it instead of overwriting. The file holds the permission rules, which a plugin cannot ship, and registers the kit so Claude Code offers the plugin to teammates who open the repo.
3. `CLAUDE.md`: if the repo has none, run `/init` and then add the "Workflow" section from [templates/CLAUDE.md](templates/CLAUDE.md). Fill in the real build, debug-install, lint and test commands.
4. Add the project's own secret files to the `deny` list. If the repo's Gradle commands differ from the `allow` list (for example `./gradlew :app:assembleDebug`), change the list to match. Add `plans/screenshots/` and `plans/*-self-check.md` to `.gitignore`: plans are committed with the code; test screenshots (attached to the PR) and self-check reports are not.
5. Restart Claude Code. Typing `/` should list `beta-kit:ticket`, `implement`, `self-check` and `pr`.
6. Jira (optional): `claude mcp add --transport http atlassian https://mcp.atlassian.com/v2/mcp`, then run `/mcp` to sign in.
7. Figma (required for any ticket that changes UI): `claude mcp add --transport http figma https://mcp.figma.com/mcp` (or, with the Figma desktop app's MCP server enabled, `claude mcp add --transport http figma-desktop http://127.0.0.1:3845/mcp`), then run `/mcp` to sign in. Give Claude the Figma link of the frame with the ticket. Claude takes designs and assets only from Figma: when the server is not connected or an asset is missing it stops and asks you, and it never guesses or draws one.

## Use

| Step | Command | What the Dev does |
| --- | --- | --- |
| 1–2 | `/beta-kit:ticket PROJ-123` (turn on plan mode first) | Confirm the deliverable, take questions to the ticket owner, approve the plan |
| 3 | `/beta-kit:implement T01`, `T02`, ... | Read the diff and commit after each task |
| 4 | `/beta-kit:self-check` | Connect a device before running; do what Claude asks for |
| 5 | `/beta-kit:pr` | Re-read the diff and open the PR |

- Put the ticket key in the branch name (`feature/PROJ-123-short-name`); the skills use it to find the plan in `plans/`.
- Claude cannot run `git push` (denied in `settings.json`) and asks before uninstalling the app, clearing its data, changing permissions or system settings over adb. The Dev pushes and opens the PR.
- Small ticket (one sentence describes the whole change): do it directly, then `/beta-kit:self-check` and `/beta-kit:pr`.
- Run `/clear` when `/context` is past half, or when Claude starts forgetting conventions.
- Connect a device over adb and sign in to the app once yourself, so Claude can verify on it.

## After each ticket

Before clearing the session, ask Claude to list what went wrong or had to be redone, sorted into three groups: true for every project (change a skill), true for this project only (add to "Lessons" in `CLAUDE.md`), happened once (drop). Review the list before anything is edited. Items in the first group go to the kit as a pull request: see [CONTRIBUTING.md](CONTRIBUTING.md).
