# BB ACP Profile Icons

A **BB-specific frontend plugin** that displays colored Amp and Devin brand marks for their ACP providers.

| Provider ID | Icon |
| --- | --- |
| `acp-amp` | Cream Amp wordmark on a dark green rounded tile |
| `acp-devin` | White Devin mark on a dark rounded tile |

## Install

Requires BB 0.42 or newer and Plugin SDK 0.4.47 or newer. Install into the BB instance serving the UI:

```sh
bb plugin install https://github.com/hxy91819/bb-plugin-acp-profile-icons --yes
```

Plugin ID: **`acp-profile-icons`**. The install builds the plugin from source. No credentials, external image requests, or configuration are needed.

The plugin decorates existing providers; configure `acp-amp` or `acp-devin` separately. For the Amp adapter with Remote Dial support, use [`hxy91819/amp-acp`, branch `local/aggregate`](https://github.com/hxy91819/amp-acp/tree/local/aggregate).

If a local copy of `acp-profile-icons` is already installed, keep a backup of its customizations before replacing its source. This public version includes the two brand icons listed above.

## Verify

```sh
bb plugin list --json
```

Check that `acp-profile-icons` is running, then open the model picker and confirm the Amp or Devin icon appears. The extension overrides icons in the frontend: `bb provider list` may still report a generic glyph and `logoUrl: null`.

To restore the default icons:

```sh
bb plugin disable acp-profile-icons
```

## How it works

`app.tsx` registers inline SVG components with `app.slots.experimental_providerIcon`. The SVGs are bundled locally and preserve their original colors. The backend entry is empty; this plugin does not change authentication, models, or agent execution.

For BB 0.42's Provider Usage panel, a scoped content stylesheet replaces monochrome logo masks with the same colored icons. It leaves React-owned elements intact and removes the stylesheet on disable or reload. Newer BB versions may no longer need that selector; the model picker uses the provider-icon extension directly.

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

Plugin code is MIT licensed. Amp and Devin brand marks belong to their respective owners. This is an independent plugin, not an official Amp or Devin product.
