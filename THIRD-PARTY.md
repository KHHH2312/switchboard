# Third-party assets

## Fonts (SIL Open Font License 1.1)

Bundled in the release under `resources/app/renderer/fonts/` (not in this repository):

| Family | Copyright | License file |
|---|---|---|
| Bodoni Moda | The Bodoni Moda Project Authors | `renderer/fonts/OFL-bodonimoda.txt` |
| Schibsted Grotesk | Schibsted / The Schibsted Grotesk Project Authors | `renderer/fonts/OFL-schibstedgrotesk.txt` |
| JetBrains Mono | The JetBrains Mono Project Authors | `renderer/fonts/OFL-jetbrainsmono.txt` |

Sources: <https://github.com/google/fonts/tree/main/ofl/bodonimoda>,
<https://github.com/google/fonts/tree/main/ofl/schibstedgrotesk>,
<https://github.com/google/fonts/tree/main/ofl/jetbrainsmono>.

## Runtime

Electron (MIT) is a build-time/dev dependency only; the portable build bundles the
Electron runtime as produced by the official distribution. @electron/fuses (MIT) is used
at build time to harden the launcher and is not bundled.
