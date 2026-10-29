# Changelog

## 1.2.0 — 2026-10-29

- Auto-rotate: when the live account drops below your threshold, Switchboard switches to the
  account with the most headroom and restarts the client for you (tray + settings).
- Tray app: keep running when the window closes, Ctrl+Alt+S to summon it, start-with-Windows,
  tray menu with auto-rotate and update check.
- Notifications: low-quota alerts and auto-rotate events as Windows toasts.
- Usage history: 5-hour trend sparkline per model group in the inspector.
- One-click updates: download, SHA-256 verify and self-install from the release feed.
- Vault export for backups; real values only, secrets warning shown.


## 1.1.0 — 2026-10-29

- In-app update check against the public release feed (Settings → Updates; silently on boot).
- Light, dark and follow-system themes; full settings sheet (auto-restart, quota refresh,
  Google client status, vault folder, quit).
- Command palette theme action; palette no longer offers the already-live account.
- Release automation: `node scripts/release.mjs` tests, packages, zips, hashes and publishes
  assets to GitHub.

## 1.0.0 — 2026-10-29

- First public release: Antigravity, ChatGPT/Codex, Claude Code, Qoder, Cursor and Gemini CLI
  account capture, verified switching, auto-restart, live vendor quota and Google sign-in
  through Chrome (PKCE loopback).
- DPAPI-sealed vault, hardened IPC/navigation, store-allowlisted file access.
