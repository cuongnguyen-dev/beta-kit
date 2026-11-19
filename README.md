# beta-claude-kit

Team Beta's workflow kit for Claude Code: four skills that take an Android ticket from Jira to a pull request description.

| Step | Command | What it does |
| --- | --- | --- |
| 1–2 | `/beta-claude-kit:ticket PROJ-123` | Reads the ticket, confirms the deliverable with the Dev, drafts the plan |
| 3 | `/beta-claude-kit:implement T01` | Does one task from the plan and verifies it (tests, device) |
| 4 | `/beta-claude-kit:self-check` | Checks the whole ticket: tests, independent review, device, each acceptance criterion |
| 5 | `/beta-claude-kit:pr` | Writes the PR description from the plan and the self-check report |

Typing `/ticket` is enough for Claude Code to suggest the full name.

## Install (once per machine)

```bash
claude plugin marketplace add cuongnguyen-dev/beta-claude-kit
claude plugin install beta-claude-kit@beta-claude-kit
```

Then set up each app repo once: see [SETUP.md](SETUP.md).

## Update

```bash
claude plugin marketplace update beta-claude-kit
claude plugin update beta-claude-kit@beta-claude-kit
```

Or turn on auto-update: `/plugin` → Marketplaces → beta-claude-kit. Every commit on `master` is a new version.

## Improve the kit

Found something a skill gets wrong on every project? Open a pull request: see [CONTRIBUTING.md](CONTRIBUTING.md).
