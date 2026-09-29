# BB ACP Profile Icons

A **BB-specific frontend plugin** that displays recognizable icons for ACP providers.

| Provider ID | Icon |
| --- | --- |
| `acp-amp` | Amp letterform cropped from its wordmark |
| `acp-devin` | Devin mark |
| `acp-codexl` | OpenAI mark |
| `acp-kiro` | Kiro mark |
| `acp-agy` | Antigravity mark |
| `acp-copilot` | GitHub Copilot mark |
| `acp-codebuddy` | CodeBuddy mark |
| `acp-dsh` | BB's existing DeepSeek Harness mark |

All marks inherit BB's foreground color in light and dark themes; this plugin adds no colored tiles.

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

`app.tsx` registers inline SVG components with `app.slots.experimental_providerIcon`. The SVGs are bundled locally and inherit BB's text color. The backend entry is empty; this plugin does not change authentication, models, or agent execution.

For BB 0.42's Provider Usage panel, a scoped content stylesheet supplies the same monochrome SVG masks. It leaves React-owned elements intact and removes the stylesheet on disable or reload. Newer BB versions may no longer need that selector; the model picker uses the provider-icon extension directly.

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

Plugin code is MIT licensed. The Kiro, Antigravity, GitHub Copilot, and CodeBuddy paths are adapted from [LobeHub lobe-icons](https://github.com/lobehub/lobe-icons) under its MIT license; see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The CodexL mark comes from BB's Codex provider icon. Brand marks belong to their respective owners. This is an independent plugin, not an official product of those brands.
