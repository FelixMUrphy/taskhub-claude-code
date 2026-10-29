# TaskHub notifications for Claude Code

A tiny [Claude Code](https://code.claude.com) plugin that shows a **Windows notification** when Claude Code

- **finishes a response** — the toast shows the project folder and the first line of the answer;
- **needs your permission** or an answer to a question.

Start a long task, switch to something else, and come back when the toast pops up. Notifications are delivered by
[**TaskHub**](https://github.com/FelixMUrphy/TaskHub), a small Windows tray app for long-running jobs and AI agents.

## Install
1. Install TaskHub 2.1.0 or newer: [download the setup.exe](https://github.com/FelixMUrphy/TaskHub/releases/latest).
2. In Claude Code:
   ```
   /plugin marketplace add FelixMUrphy/taskhub-claude-code
   /plugin install taskhub-notify@taskhub
   ```
3. Restart Claude Code. Done.

No scripts, no PowerShell, nothing to configure: the plugin only contains two
[HTTP hooks](https://code.claude.com/docs/en/hooks) (`Stop` and `Notification` for `permission_prompt|elicitation_dialog`)
that post the event to TaskHub at `http://127.0.0.1:47800/api/hooks/claude`.

### Without the plugin
Add the same hooks to `~/.claude/settings.json` yourself — see
[TaskHub API → Claude Code hooks](https://github.com/FelixMUrphy/TaskHub/blob/main/API.md#claude-code-hooks).

## How it behaves
- TaskHub never answers with a decision, so the hooks **cannot block or change** what Claude Code does.
- If TaskHub is not running, the hook simply fails silently and Claude Code continues.
- Toasts follow TaskHub's settings: *Mute notifications* in the tray menu silences them, and there are no pop-ups while
  the TaskHub panel is open.
- Different port? Edit the URL in `hooks/hooks.json` of the installed plugin.

## Privacy
Everything stays on your computer. The hook input (event name, working folder, the last assistant message) is sent only
to `127.0.0.1` (TaskHub on your own machine) and is not stored or forwarded anywhere; TaskHub only uses it to show the
notification. No telemetry.

## License
[PolyForm Shield 1.0.0](LICENSE.md), same as TaskHub.
