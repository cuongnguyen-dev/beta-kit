---
name: implement
description: Do exactly one task from the plan, verify the result yourself (tests, device), then stop for the Dev to read the diff
argument-hint: [task-id] [plan-path]
disable-model-invocation: true
---

Step 3 of the workflow. One task per invocation.

Language: reply in the language the Dev writes in, and fill in templates and reports in that language unless the Dev asks otherwise.

Arguments: $ARGUMENTS
No task id: pick the first unticked task and say which. No plan path: find the plan in `plans/` whose name starts with the ticket key in the branch name; if none or several match, list `plans/` and ask the Dev.

## Do the task

1. Read the plan: deliverable, AC, approach, and this task. If it depends on an unfinished task, stop.
2. Stay inside the task. If the plan is wrong or incomplete, stop and ask the Dev; do not change direction yourself. When the Dev replaces a decision, update every part of the plan that depends on it in the same step and mark the old decision as replaced.
3. **Code task:** reuse existing code before writing new code; write unit tests with it. Do not add comments to source files about the design source, size conversions, ticket or your process (Figma node ids, scaling formulas, "added for BT-123"); record those in the plan. Comment code only where the project's own style does. Test the code that wires parts together (ViewModel, timers, observers), not only pure helpers: one test per event or state that must happen exactly once.
4. **Documentation task:**
   - Write at the level of detail the reader in the plan needs to make their decision. Leave out implementation detail they will not use; every extra sentence is one more thing that can be wrong.
   - Use the reader's words: name screens, buttons and features the way the team does or as they appear in the app; add one line of explanation when a label is unclear. If the reader is not a developer, put source labels and class names at the end of the sentence in one fixed pattern, so they can be stripped in one pass for the delivered copy.
   - Before adding content, search the document for the same topic; if it is already covered, edit it there.
   - Every claim about app behaviour carries one label in the text: run on device, read from code (with `file:line`), or not verified.
   - Write "there is no…" or "nothing uses…" only after searching the whole source tree.
   - Do not change code and do not copy secret values. Report bugs and hardcoded secrets instead.
   - Export to the delivery format only once the content is settled.

## Verify it yourself

1. **Code task:** run lint, build and unit tests from `CLAUDE.md`, each as its own command. Read each command's own success or failure line; never report counts from result files a failed run did not rewrite. If a check already failed before your change, record that baseline and report only new failures. If the same failure survives two fixes, stop and tell the Dev.
2. **Device:** run `adb devices`. If a device is connected, install the debug build and run the flow this task changed or described: drive it with `adb shell input`, locate controls with `uiautomator dump` (use coordinates read from a screenshot only when the dump fails), look at a new screenshot after each tap to confirm the screen changed as expected, read `logcat` when needed. Save screenshots to `plans/screenshots/` (git-ignored: they get attached to the PR, never committed).
   - For a UI task, compare each device screenshot with its design frame and list the differences (layout, spacing, text, colours, assets) before reporting. Reuse an existing asset only after checking it against the one in the design. Every image, icon and animation comes from Figma, exported with the Figma MCP tool. If the tool is not connected, the asset is not in the design, or it cannot export at the needed scale, stop and ask the Dev to connect it or provide the file (node ids, format, scale). Never draw, generate, approximate or rebuild an asset yourself, and never use a placeholder without the Dev's say.
   - Label something as run on device only for the exact steps you tapped and have a screenshot of. Seeing a button is not running the flow behind it.
   - If the device disagrees with the code you read, find out why (test data, config, account state) before changing anything, and state only what holds under the conditions you ran.
   - For time-dependent scenarios (alarms, notifications, token expiry), simulate with adb (send the intent or broadcast, change the clock if the Dev allows) before proposing to wait or skip.
   - Ask the Dev before uninstalling the app, clearing its data, granting or revoking permissions, or changing system settings on a real device. A yes covers that one action only. Restore settings afterwards.
   - Do not type passwords, sign in, or pay: ask the Dev to do that step, then continue.
   - Do not open the notification shade or capture other apps. If you capture personal content, delete the image and tell the Dev.
3. Open every file you produce for delivery (images, exports) and look at it before reporting. After a script writes files, check `git status` for paths you did not expect.
4. Mark temporary test code `TEMP <ticket>` and grep for it before every report.
5. If a tool, resource or access fails, tell the Dev. Do not look for credentials or another way in.
6. When the task's done condition is met, tick it in the plan. Every note you add to the plan (results, limits, notes for other teams) carries a label as in a documentation task, names the conditions you ran it under, and claims no more than you ran or read. If the Dev asks you to tick a task whose done condition is not met, write what is unmet next to the tick.

## Report, then stop

```
Task: <id> — <name> (code / documentation)
Changed: <files>
Build, unit tests: <command → result, pass / fail counts> or "n/a: <reason>"
On device: <scenario → result, screenshots> or "no device connected"
Not verified: <list>
Needs the Dev: <what — why I could not do it myself>
Plan needs updating: <...>
Found outside scope: <...>
```

Do not commit, push or open a PR, and do not ask about them: the Dev handles git. Do not start the next task.
