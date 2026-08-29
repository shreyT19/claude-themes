# Contributing

<img src="https://cataas.com/cat/2tKejk7oauPg3Yt4?width=280" alt="two cats hugging" align="right" width="200">

New themes are welcome. Light themes especially — there are three of them against eleven dark ones, which is not a balanced diet.

<br clear="right">

## Adding a theme

**1. Name the file.** Lowercase kebab-case, `.json`, in `themes/`. The filename becomes the slug users type, so `themes/tokyo-night.json` is selected as `custom:tokyo-night`. Keep it short and obvious.

**2. Write it.** Three fields — `name`, `base`, `overrides`:

```json
{
  "name": "Tokyo Night",
  "base": "dark",
  "overrides": {
    "claude": "#bb9af7",
    "background": "#24283b"
  }
}
```

`name` is the label in the `/theme` picker. `base` is one of `dark`, `light`, `dark-daltonized`, `light-daltonized`, `dark-ansi`, `light-ansi`; anything else silently falls back to `dark`. Every key you omit inherits from the base.

**3. Test it.** Copy into your themes directory and reselect:

```sh
cp themes/your-theme.json ~/.claude/themes/
```

Then `/theme` → pick it. The picker re-reads from disk, so you can edit and reselect in a loop without restarting.

**4. Add a row** to the table in `README.md`, alphabetically.

**5. Open a draft PR.** Conventional commit style — `feat: add nord light theme`.

## Or let the built-in editor do it

`/theme` has an editor that walks the whole key list with live preview and writes the result to `~/.claude/themes/<slug>.json`. Build it there, then copy the file into `themes/` and commit it. This is usually faster and less error-prone than hand-writing JSON, and it can't produce an invalid key.

## What makes a theme worth merging

**Cover the load-bearing keys.** A theme that sets `claude` and `background` and nothing else is a tint, not a theme. Look at `themes/tokyo-night.json` for the full spread: diff colours, subagent colours, per-message backgrounds, the rate-limit bar.

**Give the eight `_FOR_SUBAGENTS_ONLY` colours genuinely distinct hues.** Claude Code hands one to each running subagent to tell them apart. Six tasteful variations of the same slate defeat the point.

**Keep shimmer pairs related.** A `Shimmer` key is the brighter partner of its base key. If `claudeShimmer` isn't recognisably a lighter `claude`, it reads as a flicker rather than a pulse.

**Check contrast on a light theme against a light terminal**, and dark against dark. It's easy to build a light theme that only works because your terminal background is doing the heavy lifting.

**Port faithfully.** If you're naming it after an existing palette, take the hexes from the upstream spec rather than eyeballing a screenshot.

## Gotchas the loader will not tell you about

These all fail silently. Nothing logs, nothing errors, the theme just loads slightly wrong:

- **An unrecognised override key is dropped.** One typo, one missing colour. Check names against [`docs/color-keys.md`](docs/color-keys.md).
- **A malformed value is dropped.** Accepted: `#rrggbb`, `#rgb`, `rgb(r, g, b)`, `ansi256(n)`, `ansi:<name>`. Note that `rgb()` wants that exact spacing — `rgba()` and bare `bb9af7` are both rejected.
- **Subdirectories are invisible.** The scan of `~/.claude/themes/` is flat. This is why the JSONs in this repo live in one directory with no grouping by base.
- **Files over 256KB are skipped.** You will never hit this, but now you know.

## Repo layout

```
.claude-plugin/     plugin + marketplace manifests
themes/             the theme files — flat, one JSON each
skills/theme-setup/ the skill that installs and switches themes
docs/               colour key reference
```
