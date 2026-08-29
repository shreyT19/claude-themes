---
name: theme-setup
description: Installs, switches, and debugs Claude Code custom themes from the claude-themes collection. Use when setting up these themes, changing the active theme, or a custom theme is missing from the /theme picker.
---

# theme-setup

Get a theme from this collection onto the user's machine and selected, without disturbing the rest of their config.

The whole job is four facts: where the theme files are, where the config directory is, what the **slug** is, and what `settings.json` says. Establish each one before writing anything.

## Step 1 — Resolve the two directories

The config directory is `$CLAUDE_CONFIG_DIR` when that variable is set and absolute, otherwise `~/.claude`. Themes live in `<config-dir>/themes/`, settings in `<config-dir>/settings.json`.

The source files are `themes/*.json` in this repo. If the skill was reached through an installed plugin rather than a checkout, the themes are already loaded — skip to Step 3 and use the plugin form of the slug.

```sh
CONFIG_DIR="${CLAUDE_CONFIG_DIR:-$HOME/.claude}"
ls "$CONFIG_DIR/themes/" 2>/dev/null
```

**Done when** you hold an absolute path to the source `themes/` directory and to the config directory, and you know which of the two install shapes applies.

## Step 2 — Pick the theme

Read `README.md` for the table of names and bases. When the user has already named a theme, match it against the filenames in `themes/` and use it. When they haven't, show them the list and ask — offer the dark/light split, since that is usually what they are actually choosing on.

**Done when** you have exactly one slug that matches a real file in `themes/`.

## Step 3 — Put the file where the loader looks

For a checkout, copy the file in. The scan is flat, so it goes directly in `themes/`, not a subdirectory:

```sh
mkdir -p "$CONFIG_DIR/themes"
cp themes/<slug>.json "$CONFIG_DIR/themes/"
```

Copy the whole set instead when the user wants to browse in the picker: `cp themes/*.json "$CONFIG_DIR/themes/"`.

For a plugin install, the files are already registered and this step is a no-op.

**Done when** `<config-dir>/themes/<slug>.json` exists and `jq -e . ` on it succeeds — or the plugin is listed by `claude plugin list`.

## Step 4 — Point settings.json at it

The setting value is `custom:` plus the slug. A plugin theme carries its plugin name too:

| Install shape | Value |
| --- | --- |
| Copied into the themes directory | `custom:<slug>` |
| Shipped by a plugin | `custom:<plugin-name>:<slug>` |

Back the file up, then edit in place so every other setting survives:

```sh
cp "$CONFIG_DIR/settings.json" "$CONFIG_DIR/settings.json.bak"
jq --arg t "custom:<slug>" '.theme = $t' "$CONFIG_DIR/settings.json" > /tmp/settings.json \
  && mv /tmp/settings.json "$CONFIG_DIR/settings.json"
```

Create `settings.json` with `{"theme": "custom:<slug>"}` when it is missing.

**Done when** `jq -r .theme "$CONFIG_DIR/settings.json"` prints the exact value from the table above, and the rest of the file is byte-identical to the backup apart from that key.

## Step 5 — Hand it back

Tell the user to run `/theme` and select the theme, or restart Claude Code. The picker re-reads theme files from disk each time it opens, so no restart is needed to make a newly copied file appear.

**Done when** the user has been told the theme's display name, the slug now in `settings.json`, and where the backup went.

## How the loader resolves a theme

Worth holding while you work, because each of these has a failure that looks like nothing happening:

- The scan of `<config-dir>/themes/` is **flat** and matches `*.json` only. A theme in a subdirectory is invisible.
- The **slug is the filename** minus `.json`. Renaming the file renames the theme.
- Plugin themes are slugged `<plugin-name>:<filename>`, and the setting still needs the `custom:` prefix in front of that.
- `base` must be one of `dark`, `light`, `dark-daltonized`, `light-daltonized`, `dark-ansi`, `light-ansi`. Anything else loads as `dark`.
- An unrecognised key in `overrides`, or a value in an unaccepted format, is **dropped without a warning**. The theme still loads, minus that colour.
- Theme files over 256KB are skipped.

## Troubleshooting

| Symptom | Cause to check first |
| --- | --- |
| Theme missing from `/theme` | File is in a subdirectory, or has an extension other than `.json` |
| `settings.json` value ignored | `custom:` prefix missing, or plugin name missing from a plugin theme's slug |
| Theme loads but one colour is wrong | Key name typo, or a value format the loader rejects — check against `docs/color-keys.md` |
| Everything reverted to default | `settings.json` is no longer valid JSON; restore the `.bak` and redo Step 4 |
| Wrong light/dark contrast | `base` is misspelled and fell back to `dark` |

## Authoring a new theme

For building or editing a theme rather than installing one, the key reference is [`docs/color-keys.md`](../../docs/color-keys.md) — all 72 keys, grouped, with the accepted colour formats. [`CONTRIBUTING.md`](../../CONTRIBUTING.md) covers naming, testing, and what makes a theme worth merging.

`/theme` also has a built-in editor that writes a valid file to `<config-dir>/themes/<slug>.json`, which is the fastest route to a new theme.
