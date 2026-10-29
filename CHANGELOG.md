# Changelog

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
