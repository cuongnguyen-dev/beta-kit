---
name: ticket
description: Read a Jira ticket, confirm the deliverable with the Dev, ask clarifying questions, then draft the plan
argument-hint: [ticket-key or link]
disable-model-invocation: true
---

Steps 1–2 of the workflow. Do not write or change code or docs in this skill. Do not ask about branches, commits or pull requests: the Dev handles git and opens the PR.

Language: reply in the language the Dev writes in, and fill in templates and reports in that language unless the Dev asks otherwise.

Ticket: $ARGUMENTS

## 1. Understand the ticket

1. Read the ticket with the Jira tool if available; otherwise ask the Dev to paste it. Read linked tickets and their comments too.
2. Read the related code.
3. **Design.** Skip this step, and do not ask about design or Figma, when the ticket changes nothing the user sees (logic, data, tracking, build, documentation). If the ticket adds or changes anything the user sees, you need the design before planning. If the Dev says there is no design for it, follow the existing screens and record that in the plan. Ask the Dev for the Figma link if the ticket has none, and open it with the Figma MCP tool. If that tool is not connected or the call fails, stop and ask the Dev to connect it (`/mcp`); do not continue from the ticket text or from memory of similar screens. Check that you have, for each screen: every state (loading, empty, error, success), the final copy, and the assets and motion files. List what is missing and ask the Dev for it. Never guess a design and never draw or generate an image, icon or animation yourself.
4. If the whole change can be described in one sentence, tell the Dev no plan is needed and stop.

## 2. Confirm the deliverable with the Dev

State the following briefly, then stop and wait for the Dev to confirm or correct. Do not continue without confirmation.

- **Deliverable:** exactly what will exist when the ticket is done.
- **Reader and purpose:** who uses it and what they will decide or do with it. This sets the level of detail.
- **Format and location:** for code, the module or area of the app it lives in. For a document, where the reader will open it (Confluence, Google Docs, a link) and whether they have access; pick one source format that can go straight into that place.
- **Done means:** the ticket's acceptance criteria, copied verbatim, and which of them need someone else (review, approval, answers). If the ticket has no AC, ask the Dev.
- **Inputs still missing:** design (screens, states, copy, assets), test accounts, test hardware, access. For each one, say who provides it and ask the Dev for it now.
- **Out of scope.**
- **What I am guessing:** anything the ticket does not say that you inferred.

## 3. Questions for the ticket owner

Ask only what blocks the work. One decision per question, with 2–3 options and your recommendation. Stop and wait for the Dev to bring answers back. Record anything unanswered as an assumption.

## 4. Draft the plan

1. Follow [plan-template.md](plan-template.md) exactly; keep it short.
2. Map every AC to at least one task and one check. Constraint words such as "complete", "all", "no remaining" need a count or a comparison against the source of truth.
3. An AC about behaviour the user sees is checked on a device by default; skipping that needs a reason written in the plan.
4. Each task is at most half a day and has a done condition. Do not plan UI work from a guess: a task whose design or other input is still missing is marked blocked, with what it is waiting for.
5. When the Dev approves, write the plan to `plans/<KEY>-<short-name>.md` (if still in plan mode, ask the Dev to exit it), then tell the Dev to run `/beta-claude-kit:implement`.
