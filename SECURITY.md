# Security

Switchboard reads and writes the credential stores of other applications, so reports about
its own handling of those secrets are taken seriously.

## Reporting a vulnerability

Do not open a public issue for anything that exposes credentials, escapes the IPC boundary,
or can be triggered by a crafted provider file. Use GitHub's private vulnerability reporting
("Report a vulnerability" under the Security tab) and include:

- affected version / commit,
- a reproduction (steps or minimal script),
- what an attacker gains.

You will get an acknowledgement within a few days. Please give a fix and a release a
reasonable window before publishing.

## Threat model (what is in scope)

- The **renderer is untrusted**: provider data is interpolated into the DOM, so any way to
  execute script there is a vulnerability. IPC handlers validate their inputs; paths passed
  to `openPath` are constrained to the user profile and the vault directory.
- The **vault** is sealed with Windows DPAPI (CurrentUser) at
  `%APPDATA%\Switchboard\vault.dat`. The non-Windows fallback is AES-256-GCM with a key
  derived from the machine/user name — weaker, and only there for development parity.
- **Credentials never leave the machine** except to the vendor endpoints needed to refresh a
  token or read quota (Google, OpenAI, Anthropic, Cursor). There is no telemetry.
- **OAuth loopback**: the sign-in flow binds a random localhost port, uses a random `state`,
  completes in the system browser and validates `state` before exchanging the code.

## What is out of scope

- Anyone with an interactive session as the same Windows user can read the credential stores
  of the target applications directly; that is the environment this tool is built for.
- The Google OAuth client id/secret embedded in `src/oauth.js` together with the upstream
  flow is the client used by the open-source Antigravity Tools project; it ships inside every
  copy of that app and is not a confidential secret. Treat it as a public client identifier.

## Distribution caveats (v1)

- The portable build in `dist/` is **unsigned** and keeps Electron's default fused flags
  (RunAsNode enabled), and its `resources/app` is a plain folder. For a signed release, build
  with Electron Forge/Builder: asar packaging, `RunAsNode: false`, `EnableNodeCliInspectArguments:
  false`, and a code-signing certificate. Until then, publish checksums with the zip.
- The exe metadata (ProductName/Company) still reads "Electron" in v1; the window icon is
  Switchboard's own.
- No auto-updater: users update by downloading a new portable folder.
