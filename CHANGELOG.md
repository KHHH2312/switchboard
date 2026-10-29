# Changelog

## 1.2.3 — 2026-10-29

- Fixed the ChatGPT desktop relaunch: the launch path was missing a backslash, so the app was
  never started after a profile restore. Launches now go through IApplicationActivationManager
  (with an explorer fallback) and are verified by watching for ChatGPT.exe for up to 15 s.
- Desktop restore reports `relaunched` and any failure explicitly; capture/switch toasts show it.

## 1.2.2 — 2026-10-29

- **ChatGPT / Codex desktop sessions**: Capture now also snapshots the desktop app's browser
  profile (cookies + storage) next to the account, and switching restores it and relaunches the
  app — so the app opens signed in as the same account as the CLI, not on the sign-in screen.
- Capture closes the desktop app briefly for a consistent profile copy, then reopens it.
- Codex switches no longer force a desktop relaunch when no desktop snapshot is stored.
- UI shows when a desktop session was captured; capture toast explains when it was skipped.

## 1.2.1 — 2026-10-29

- Single-instance behaviour: launching Switchboard again (desktop shortcut, Start menu, tray)
  brings the existing window to the front instead of starting a second copy.
- Desktop shortcut with the Switchboard mark.

## 1.2.0 — 2026-10-29

**Switchboard now rotates accounts for you, lives in the tray, and updates itself.**

### Highlights

- **Auto-rotate on low quota** — pick a floor (5 / 10 / 20 / 30%). When the live account crosses
  it, Switchboard moves to the account with the most 5-hour headroom, writes and verifies the
  new credential, restarts the client and tells you what happened. It keeps running while the
  window is closed.
- **One-click updates** — Settings → Updates checks the release feed, downloads the new build,
  verifies its SHA-256 against the published `SHA256SUMS.txt`, swaps it in after exit and
  relaunches. A tampered or incomplete download is discarded, never installed.
- **Signed releases** — the launcher is code-signed (SHA-256, DigiCert timestamp) and the
  signing certificate ships in the archive. SmartScreen still shows "unknown publisher" until
  an OV/EV certificate is used — the checksum remains the trust anchor.

### New

- **Tray presence**: keep Switchboard in the tray, start it with Windows, summon it anywhere
  with `Ctrl+Alt+S`; closing the window no longer stops auto-rotate.
- **Notifications**: low-quota warnings (deduplicated for 30 minutes) and every automatic
  rotation arrive as Windows toasts.
- **Usage history**: a 5-hour trend sparkline per model group in the inspector, built from the
  last 96 quota samples.
- **Vault export**: save an encrypted-vault snapshot as JSON for backups or moving machines
  (the dialog warns that it contains secrets).
- **Settings growth**: auto-rotate toggle and threshold, low-quota notifications, keep-in-tray,
  start-with-Windows — all applied instantly and stored with the vault.

### Security

- Update downloads are restricted to `github.com` and `*.githubusercontent.com`, size-capped,
  and only installed after the SHA-256 in the release's sums file matches.
- Update installs run through a one-shot scheduled task so they survive the app's process job;
  read-only install folders fail with an explicit message instead of a half-copy.
- No new network destinations: quota, sign-in and the update feed are the only calls, and the
  README now discloses the update check alongside them.

### Under the hood

- `npm run release` is the whole pipeline: sandbox suite → package → sign → zip → hash →
  GitHub release with assets → sync of the public documentation repo.
- `scripts/sign.ps1` supports a purchased certificate via `SWITCHBOARD_PFX` /
  `SWITCHBOARD_PFX_PASSWORD` or `SWITCHBOARD_CERT_THUMBPRINT`, and self-signs only when none
  is supplied.

### Upgrade notes

- Existing vaults are read as-is; settings gain the new keys with safe defaults
  (auto-rotate off, notifications on, keep-in-tray on).
- The portable build is still a folder — unzip over your current one, or use the in-app updater
  from 1.2.0 onward.

### Checksums

```
cbff9da868577a61615f46a7dedfad44a0c87cab87a6865962b6b60d9aa6247e  Switchboard-1.2.0-win32-x64.zip
c581f998366451e5570c872c3ca602bb0b455dac8eeef6b0f79bacb07ffd2372  Switchboard/Switchboard.exe
```

---

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
