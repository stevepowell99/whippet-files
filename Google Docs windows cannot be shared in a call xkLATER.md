# Google Docs windows cannot be shared in a call

- Google Workspace doc windows are still owned by the main window (`parent:` in `openWebEditorWindow`).
  - Windows leaves an owned window out of the taskbar, Alt+Tab and every screen-share picker (Meet, Teams, Zoom), so a Google Doc opened from Whippet cannot be shared.
  - gmist windows were made top-level for this reason on 19 November 2026 (`topLevel` in `WEB_EDITOR_KINDS`, commit `0007ea0`).
- Fix: add `topLevel: true` to `googleWorkspace`.
  - Cost: the window stops staying above Whippet.
  - The shortcut-raise parking (`minimiseWebEditorWindows`) then has no owned windows left to park, so remove it.
- Steve, 19 November 2026: "not for now".
