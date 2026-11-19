# Contributing to beta-claude-kit

Skills improve from real tickets. When a skill let Claude do the wrong thing, fix the skill so the next ticket does not hit it again.

## What belongs in the kit

Use the list from "After each ticket" in [README.md](README.md):

| The problem is | Where the fix goes |
| --- | --- |
| True for every project | This repo, as a pull request |
| True for one project only | "Lessons" in that app repo's `CLAUDE.md` |
| Happened once | Nowhere |

## Make the change

1. Clone the repo and create a branch. Do not push to `master`.

   ```bash
   git clone git@github.com:cuongnguyen-dev/beta-claude-kit.git
   cd beta-claude-kit
   git switch -c fix/self-check-skips-device
   ```

2. Edit the skill under `skills/<name>/SKILL.md` (or its template next to it).
3. Try it on a real ticket. From the app repo, start Claude Code with your clone instead of the installed plugin:

   ```bash
   claude --plugin-dir /path/to/beta-claude-kit
   ```

   After further edits, run `/reload-plugins` in that session.
4. Check the plugin still loads: `claude plugin validate /path/to/beta-claude-kit`.
5. Push the branch and open a pull request. Fill in the template.

## Rules for a skill change

- One problem per pull request.
- Say what happened: the ticket, what the skill told Claude, what Claude did instead.
- Change an existing sentence before adding a new one. Add a sentence only when it would have prevented a failure that really happened; skills that grow stop being followed.
- Keep project names, class names and ticket-specific detail out of skills.
- If the change renames a command, a file the skills write, or a permission rule, update `README.md` and `templates/` in the same pull request.

## Review and release

The kit owner reviews and merges. Every commit on `master` is a new version; Devs get it with the update commands in [README.md](README.md). If the change needs something done in app repos (a new `.gitignore` line, a new permission rule), say so in the pull request so it can be announced.
