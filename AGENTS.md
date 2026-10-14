# SUPER DESKTOP plugin: Reminder

This folder is a SUPER DESKTOP plugin (QML). SUPER DESKTOP reloads it on
screen every time a file here is saved.

Before changing anything, read `.claude/skills/super-desktop-plugin/SKILL.md`
and follow its loop:

1. edit the QML or plugin.json;
2. `super-desktop plugin check reminder --json` and fix every error and warning;
3. `super-desktop plugin snapshot reminder --out /tmp/reminder.png --json` (add
   `--surface <id>` for each surface), then open the PNG and look at it;
4. `super-desktop plugin logs reminder` for runtime errors.

Keep the plugin id `reminder` (it must equal the folder name). You may change
the slots, add surfaces and files, and rewrite the starting QML entirely.

## What it does

A bell in the top bar (`Toolbar.qml`, surface `main`). Left click opens the
`add` popup (`AddPopup.qml`: when and what; OK saves), right click the `list`
popup (`ListPopup.qml`: click a reminder to remove it). Reminders are saved in
`sd.values.reminders` as `[{id, at, text, fired, seen}]`; `reminders.js` reads
them (a saved list is a Qt list, not a JS array) and picks text colors that
keep a 4.5:1 contrast in every Omarchy theme.

The bell checks every 5 s. A due reminder gets `fired: true, seen: false`: a
notification, `ding-dong.wav` (made by `tools/make-ding-dong.py`) through
pw-play/paplay/aplay, again every minute up to 5 times, and the bell shakes
over a moving gradient until it is clicked (`seen: true`).

Needs SUPER DESKTOP 1.6.0 or newer (the `popup` slot and `tooltip`).
