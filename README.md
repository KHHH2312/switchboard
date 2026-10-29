<div align="center">

<img src="docs/icon.png" width="88" alt="Switchboard">

# Switchboard

### Every AI coding account. One switchboard.

Capture the live sign-in of every coding agent on your PC, watch the quota burn down, and
move to the next account in one verified click — no re-login, no browser dance, no guesswork.

[![Download for Windows](https://img.shields.io/badge/Download-Windows%20x64-FF5A2B?style=for-the-badge&logo=windows11&logoColor=white)](https://github.com/KHHH2312/switchboard/releases/latest)
[![Latest release](https://img.shields.io/github/v/release/KHHH2312/switchboard?style=flat-square&color=FF5A2B)](https://github.com/KHHH2312/switchboard/releases)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%2F11%20x64-EDE7DA?style=flat-square)](#requirements)
[![License](https://img.shields.io/badge/license-MIT-EDE7DA?style=flat-square)](LICENSE)

<img src="docs/main.png" alt="Switchboard" width="100%">

</div>

---

## Why people keep it pinned

**Your quota is a countdown, not a mystery.** Five-hour and weekly windows for every model
group, straight from the vendor's own API, with reset timers — so you switch *before* the
agent stalls, not after.

**Switching is verified, not hoped for.** Every write is read back and the account identity
compared. If verification fails you get an error, not a silent half-switch. Auto-restart
relaunches the client on the new account; Qoder and Cursor are closed first because they
cache auth in memory.

**Sign in with the Chrome you already use.** The Google flow runs in your system browser with
PKCE and a loopback listener — the account already signed in there is one click away.

**One table for six clients.** Antigravity, ChatGPT/Codex, Claude Code, Qoder, Cursor and
Gemini CLI. Plans (PRO / ULTRA / PLUS / MAX / FREE), last-used, key search, keyboard-first
(`Ctrl+K`), light & dark themes, and a DPAPI-sealed vault. No telemetry.

## Features

| | |
|---|---|
| **Verified switching** | Credential written, read back, identity-checked, app restarted. Failed switches are reported as failures. |
| **Live usage** | Google Cloud Code quota, OpenAI `wham/usage`, Anthropic OAuth usage, Cursor dashboard — 5-hour + weekly windows with reset times. |
| **Plan aware** | PRO / ULTRA / PLUS / MAX / FREE badges; filters that only show plans you actually have. |
| **Browser sign-in** | Google OAuth with PKCE in Chrome/Edge; accounts land in the vault with quota ready. |
| **Antigravity Tools import** | Bring existing accounts in from `~/.antigravity_tools` with one click. |
| **Private by design** | Vault sealed with Windows DPAPI; renderer sandboxed; navigation denied; IPC frame-gated; clipboard auto-wipes copied credentials; no telemetry. |
| **Instrument-panel UI** | Bodoni Moda + Schibsted Grotesk + JetBrains Mono, segmented LED meters, light/dark/system themes, `prefers-reduced-motion` respected. |

## Requirements

- Windows 10 or 11, x64
- The client you want to manage (Antigravity, Codex, Claude Code, Qoder, Cursor, Gemini CLI)
- Google sign-in (optional) needs your own OAuth desktop client — this project ships none:
  see [Google sign-in setup](#google-sign-in-setup)

## Install

1. Download the latest **Switchboard-x.y.z-win32-x64.zip** from
   [Releases](https://github.com/KHHH2312/switchboard/releases/latest).
2. Verify it against `SHA256SUMS.txt` attached to the same release:
   ```powershell
   Get-FileHash .\Switchboard-*-win32-x64.zip -Algorithm SHA256
   ```
3. Unzip anywhere and run **Switchboard.exe** (no installer, no admin rights).
4. Switchboard checks for updates on launch; you can also check from **Settings → Updates**.

> Builds are portable and currently **unsigned**, so SmartScreen may ask once. The checksum is
> the trust anchor until code signing lands.

## Using it

1. Pick a client in the top bar.
2. **Sign in with Google** (Antigravity) or **Capture current** to snapshot what the client is
   signed into right now.
3. Sign into the other account in that client, capture again.
4. Press the switch — the app writes, verifies, restarts the client if you left auto-restart on.
5. `Ctrl+K` for the command palette; `1`–`6` jumps between clients; the sun icon flips theme.

<div align="center">
<img src="docs/theme-light.png" alt="Light theme" width="49%">
<img src="docs/settings.png" alt="Settings" width="49%">
</div>

## Supported clients

| Client | Credential store | Usage |
|---|---|---|
| Antigravity | Windows Credential Manager `gemini:antigravity` (+ CLI token mirror) | Google Cloud Code quota |
| ChatGPT · Codex | `%USERPROFILE%\.codex\auth.json` | `chatgpt.com/backend-api/wham/usage` |
| Claude Code | `~/.claude/.credentials.json` + `~/.claude.json` | Anthropic OAuth usage |
| Qoder | `%APPDATA%\com.qoder.app.stable\auth.v1.dat` | — |
| Cursor | `%APPDATA%\Cursor\User\globalStorage\state.vscdb` | Cursor dashboard service |
| Gemini CLI | `~/.gemini/oauth_creds.json` + `google_accounts.json` | — |

## Google sign-in setup

This distribution repository ships **no OAuth credentials**. To enable the Google button,
create a *Desktop app* OAuth client in the
[Google Cloud Console](https://console.cloud.google.com/apis/credentials), then drop:

```json
{ "client_id": "YOUR_ID.apps.googleusercontent.com", "client_secret": "YOUR_SECRET" }
```

into `%APPDATA%\Switchboard\google.json` (create the folder if needed) and restart.
Scopes requested: `openid`, `cloud-platform`, `userinfo.email`, `userinfo.profile`, `cclog`,
`experimentsandconfigs` — the same set Antigravity itself uses.

## Security

- Credentials never leave your machine except to the vendor whose quota/token is being read:
  Google, OpenAI, Anthropic, Cursor. **No telemetry.**
- Vault: `%APPDATA%\Switchboard\vault.dat`, sealed with Windows DPAPI (current user).
- The UI runs in a sandboxed renderer; navigation and window-open are denied; IPC rejects any
  frame that isn't the app's own document; file-opening is restricted to registered credential
  stores.
- Found a flaw? Please use the private channel in [SECURITY.md](SECURITY.md), not a public issue.

## FAQ

**Does it store my passwords?** No — it stores the same OAuth/refresh tokens the clients
already keep on disk, encrypted with your Windows user key.

**What happens if a switch fails?** You get an explicit error. Nothing is reported as switched
until the credential reads back with the expected identity.

**Does it work if the client is running?** Yes for most; Qoder and Cursor are closed and
relaunched around a switch because they rewrite auth on exit. Auto-restart can be disabled.

**Which Windows versions?** 10 and 11, x64. ARM is not built yet.

**Is it open source?** This repository distributes the application and its documentation; it
does not host the application source.

## Changelog

See [CHANGELOG.md](CHANGELOG.md).

## License

MIT — see [LICENSE](LICENSE). Third-party assets in [THIRD-PARTY.md](THIRD-PARTY.md).
"Antigravity", "ChatGPT", "Claude", "Cursor", "Qoder" and "Gemini" are trademarks of their
respective owners; this project is not affiliated with them.
