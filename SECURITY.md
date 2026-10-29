# Security

Switchboard reads and writes the credential stores of other applications, so reports about
its own handling of those secrets are taken seriously.

## Reporting a vulnerability

Do not open a public issue for anything that exposes credentials, escapes the IPC boundary,
or can be triggered by a crafted provider file. Use GitHub's private vulnerability reporting
("Report a vulnerability" under the Security tab) and include:

- affected version,
- a reproduction (steps or minimal script),
- what an attacker gains.

You will get an acknowledgement within a few days. Please give a fix and a release a
reasonable window before publishing.

## Threat model (what is in scope)

- The **renderer is untrusted**: provider data is interpolated into the DOM, so any way to
  execute script there is a vulnerability. The renderer runs sandboxed with context isolation;
  navigation, window-open, webviews and permission requests are denied. IPC handlers reject
  frames that are not the app's own document.
- Paths passed to the file-opening IPC are constrained to the vault directory or to a
  credential store that a registered provider actually detected; the renderer cannot name an
  arbitrary file.
- The **vault** is sealed with Windows DPAPI (CurrentUser) at
  `%APPDATA%\Switchboard\vault.dat`. The non-Windows fallback is AES-256-GCM with a key derived
  from the machine/user name — weaker, and only there for development parity.
- **Credentials never leave the machine** except to the vendor endpoints needed to refresh a
  token or read quota (Google, OpenAI, Anthropic, Cursor). There is no telemetry.
- **OAuth loopback**: the sign-in flow binds a random localhost port, uses a random `state`,
  completes in the system browser and validates `state` before exchanging the code. The OAuth
  client is bring-your-own (`%APPDATA%\Switchboard\google.json`); no client credentials ship
  with the build.
- **Copied secrets** are wiped from the clipboard after 30 seconds if unchanged.
- **Updates** download only from `github.com` / `*.githubusercontent.com` release assets, verify
  the archive against the release's `SHA256SUMS.txt` before extraction, and install through a
  one-shot scheduled task. The build is code-signed with a self-signed certificate; Windows
  treats it as an unknown publisher, so the SHA-256 checksum remains the trust anchor.
- The packaged launcher disables Electron's Node runtime entry points (`RunAsNode`,
  `NODE_OPTIONS` environment variable, `--inspect` arguments) before signing.

## Residual risks

- Anyone with an interactive session as the same Windows user can read the credential stores
  of the target applications directly; that is the environment this tool is built for.
- The package is a portable folder, not an installer: `resources/app` ships as plain files, so
  the application code is readable inside the build. No user secrets are included.
- `OnlyLoadAppFromAsar` cannot be enabled while the app ships as a folder; an attacker who can
  already write to the install directory can replace app files.
- Update authenticity follows GitHub: if the release account or its assets are compromised, a
  malicious build can still present a matching checksum. Verify releases independently if that
  is in your threat model.
