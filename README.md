# claude-themes

Custom Claude Code themes. Fourteen of them — a few ports of well-known editor palettes, a few built from scratch.

## Themes

| File | Name | Base |
| --- | --- | --- |
| `blue-dark` | Deep Blue | dark |
| `catppuccin-latte` | Catppuccin Latte | light |
| `catppuccin-mocha` | Catppuccin Mocha | dark |
| `dracula` | Dracula | dark |
| `everforest` | Everforest Dark | dark |
| `green-dark` | Evergreen Dark | dark |
| `green-light` | Sage Light | light |
| `gruvbox-dark` | Gruvbox Dark | dark |
| `kanagawa` | Kanagawa | dark |
| `nord` | Nord | dark |
| `rose-pine` | Rosé Pine | dark |
| `solarized-light` | Solarized Light | light |
| `tokyo-night` | Tokyo Night | dark |
| `violet-dark` | Violet Dusk | dark |

## Install

Clone into the themes directory Claude Code reads from:

```sh
git clone git@github.com:shreyT19/claude-themes.git ~/.claude/themes
```

Then pick one in `~/.claude/settings.json` — `custom:` followed by the filename without the extension:

```json
{
  "theme": "custom:tokyo-night"
}
```

## Writing a theme

Each file is a `name`, a `base` of `dark` or `light`, and an `overrides` map of colour keys to hex values. Anything left out falls back to the base theme, so a minimal theme is a handful of keys:

```json
{
  "name": "My Theme",
  "base": "dark",
  "overrides": {
    "claude": "#a78bfa",
    "promptBorder": "#5b4b8a",
    "background": "#1e1e2e"
  }
}
```

`tokyo-night.json` is the fullest example — diff colours, subagent colours, rate-limit bar, message backgrounds.
