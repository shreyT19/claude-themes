# claude-themes

<img src="https://cataas.com/cat/2T7yPn3J5qz54Ygy?width=300" alt="a cat wearing a party hat" align="right" width="220">

Fourteen colour themes for Claude Code. A few careful ports of well-known editor palettes, a few built from scratch at an hour when the terminal looked personally offensive.

Each one is a small JSON file — no build step, no dependencies, nothing to run. Claude Code reads them at startup and paints itself.

The cats are load-bearing. Do not remove the cats.

<br clear="right">

## The themes

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

The ten palette ports each set 54 of the 72 available keys — diffs, subagent colours, the rate-limit bar, per-message backgrounds. `blue-dark`, `violet-dark` and the two greens are lighter touches at 34–38. Fork `tokyo-night` if you want a full palette to start from.

## Install

<img src="https://cataas.com/cat/3Z6CcYkHotdUXQC9?width=260" alt="a kitten with its paws up, begging" align="right" width="190">

### Let Claude do it

The one that requires the least of you. Clone the repo anywhere and ask:

```sh
git clone git@github.com:shreyT19/claude-themes.git
cd claude-themes
claude
```

> use skills/theme-setup/SKILL.md to set me up with tokyo-night

The skill knows where the themes go, how the slug is derived, how to edit `settings.json` without flattening the rest of it, and what to check when a theme doesn't show up. It'll ask which theme you want if you don't name one.

<br clear="right">

### As a plugin

Brings the themes *and* the setup skill along, and updates with `/plugin update`:

```
/plugin marketplace add shreyT19/claude-themes
/plugin install claude-themes@claude-themes
```

Plugin themes are namespaced by plugin name, so they resolve as `custom:claude-themes:<file>`:

```json
{ "theme": "custom:claude-themes:tokyo-night" }
```

### By hand

Claude Code scans `~/.claude/themes/` for `*.json`. The scan is **flat** — files in subdirectories are ignored — so the files have to land directly in that folder:

```sh
git clone git@github.com:shreyT19/claude-themes.git /tmp/claude-themes
mkdir -p ~/.claude/themes
cp /tmp/claude-themes/themes/*.json ~/.claude/themes/
```

The slug is the filename minus `.json`, and the setting takes a `custom:` prefix:

```json
{ "theme": "custom:tokyo-night" }
```

Then run `/theme` and pick it, or restart Claude Code.

## Switching

`/theme` lists every custom theme it found alongside the built-ins. It re-reads the files from disk each time it opens, so a theme you just edited shows up without a restart.

## Writing your own

<img src="https://cataas.com/cat/48xLBZGSXgxZRMAB?width=260" alt="a sleepy kitten" align="right" width="190">

A theme is three fields:

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

Anything you leave out of `overrides` inherits from `base`, so a usable theme can be a handful of keys.

There are 72 keys in total. [`docs/color-keys.md`](docs/color-keys.md) lists all of them, grouped, along with the accepted colour formats and the valid `base` values.

<br clear="right">

### The one thing that will bite you

An override key Claude Code doesn't recognise is **dropped silently**. So is a value in a format it doesn't parse. No warning, no error — the theme loads, that one colour just isn't there, and you sit squinting at the terminal wondering why `promtBorder` did nothing.

<img src="https://cataas.com/cat/3mEJCz1Oj7l1E2tm?width=260" alt="a visibly furious cat" align="center" width="230">

Check spelling against `docs/color-keys.md`.

## Contributing

New themes welcome, especially light ones — the set is lopsided. See [`CONTRIBUTING.md`](CONTRIBUTING.md).
