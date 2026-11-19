# <App name>

<!-- Keep this short. Do not record things the code already shows (SDK versions, class lists, test counts): they go stale. -->

## Commands
- Build: `<command>`
- Install debug build on a device: `<command>`
- Lint: `<command>`
- Unit tests: `<command>`

## Not visible from the code
- <architecture decisions, allowed libraries, analytics / feature-flag conventions>
- <team vocabulary: what the team calls each screen and feature>

## Workflow
- `/ticket <key>` → `/implement <task>` → `/self-check` → `/pr`. Plans live in `plans/`.
- Verify your own work, including on a device connected over adb. Ask the Dev only for what you cannot do, and say why.
- Never delete or disable a failing test. Never change signing config. Never add a library that is not in the plan.

## Lessons
<!-- Add an entry only when a mistake repeats and cannot be enforced by a test, lint rule or hook. Include a concrete example. -->
