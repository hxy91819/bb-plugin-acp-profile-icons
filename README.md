# BB ACP Profile Icons

A **BB-specific frontend plugin** that displays recognizable icons for ACP providers.

| Provider ID | Icon |
| --- | --- |
| `acp-amp` | Full Amp wordmark, white on gray |
| `acp-devin` | Devin mark |
| `acp-codexl` | OpenAI mark |
| `acp-kiro` | Kiro mark |
| `acp-agy` | Antigravity mark |
| `acp-copilot` | GitHub Copilot mark |
| `acp-codebuddy` | CodeBuddy cat mark |
| `acp-dsh` | DeepSeek whale mark |

Other marks inherit BB's foreground color in light and dark themes; Amp uses a gray tile with white lettering.

## Install

Requires BB 0.42 or newer and Plugin SDK 0.4.47 or newer. Install into the BB instance serving the UI:

```sh
bb plugin install https://github.com/hxy91819/bb-plugin-acp-profile-icons --yes
```

Plugin ID: **`acp-profile-icons`**. The install builds the plugin from source. No credentials, external image requests, or configuration are needed.

The plugin decorates existing providers; configure those providers separately. For the Amp adapter with Remote Dial support, use [`hxy91819/amp-acp`, branch `local/aggregate`](https://github.com/hxy91819/amp-acp/tree/local/aggregate).

If a local copy of `acp-profile-icons` is already installed, keep a backup of its customizations before replacing its source.

## Verify

```sh
bb plugin list --json
```

Check that `acp-profile-icons` is running, then open the model picker and confirm the icons appear. The extension overrides icons in the frontend: `bb provider list` may still report a generic glyph and `logoUrl: null`.

To restore the default icons:

```sh
bb plugin disable acp-profile-icons
```

## How it works

`app.tsx` registers inline SVG components with `app.slots.experimental_providerIcon`. The SVGs are bundled locally; except for Amp's gray-and-white tile, they inherit BB's text color. The backend entry is empty; this plugin does not change authentication, models, or agent execution.

For BB 0.42's Provider Usage panel, a scoped content stylesheet supplies SVG masks (or Amp's two-color SVG image). It leaves React-owned elements intact and removes the stylesheet on disable or reload. Newer BB versions may no longer need that selector; the model picker uses the provider-icon extension directly.

## Icon design and verification

- Identify the provider and its brand before choosing artwork. Prefer the installed product's own assets or an attributable vector source; do not treat a host's placeholder icon as the brand logo. Record third-party sources in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
- Preserve the recognizable **complete** mark at its final display size. Do not crop a wordmark to one letter or approximate a detailed mark with only a few paths. Inspect the original SVG's filled areas and cutouts before changing its fill or adding a mask; a second mask can hide the artwork entirely.
- Use BB's monochrome foreground for ordinary marks. When a mark needs a background, keep the tile square in the 24×24 icon viewBox and use a neutral gray with legible lettering; do not substitute a black or colored tile for a gray one. Check optical size and spacing beside BB's built-in icons, not only the SVG dimensions.
- After editing, run `npm run typecheck`, `bb plugin build .`, and `bb plugin reload acp-profile-icons`. Open the **actual** model picker in both light and dark themes, capture and inspect its icons, and check the Provider Usage panel if its CSS icon path changed. Compilation or a screenshot that has not been inspected is not visual verification.

## Develop

```sh
git clone https://github.com/hxy91819/bb-plugin-acp-profile-icons.git
cd bb-plugin-acp-profile-icons
npm ci
npm run typecheck
bb plugin build .
bb plugin install . --yes
```

For an installed local checkout, rebuild and run `bb plugin reload acp-profile-icons` after changes. Use the target BB version's SDK declarations when adapting the experimental icon API.

## Attribution

Plugin code is MIT licensed. The Kiro, Antigravity, GitHub Copilot, and DeepSeek paths are adapted from [LobeHub lobe-icons](https://github.com/lobehub/lobe-icons) under its MIT license; the CodeBuddy cat path is adapted from the locally installed `@tencent-ai/codebuddy-code/dist/web-ui/pwa-icon.svg`. See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The CodexL mark comes from BB's Codex provider icon. Brand marks belong to their respective owners. This is an independent plugin, not an official product of those brands.
