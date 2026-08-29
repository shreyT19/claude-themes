# Colour keys

Every key Claude Code accepts in a theme's `overrides` block, pulled from the running binary (v2.1.251). There are 72.

Anything you leave out inherits from your `base` theme, so a good theme usually sets a few dozen, not all 72.

## The rules the loader actually enforces

- **A key that isn't on this list is dropped silently.** No warning, no error — your theme just loads without it. Typos die quietly, so check spelling against this page.
- **A value in the wrong format is dropped the same way.** Accepted formats:

  | Format | Example |
  | --- | --- |
  | 6-digit hex | `#bb9af7` |
  | 3-digit hex | `#b9f` |
  | `rgb()` | `rgb(187, 154, 247)` |
  | 256-colour | `ansi256(141)` |
  | Named ANSI | `ansi:magentaBright` |

- **`base` must be one of** `dark`, `light`, `dark-daltonized`, `light-daltonized`, `dark-ansi`, `light-ansi`. Anything else falls back to `dark`.
- **A theme file over 256KB is skipped.** You will not get near this.

## Naming conventions

A `Shimmer` suffix is the animated or brighter partner of its base key — `claude` / `claudeShimmer`. Set both or neither; a shimmer that clashes with its base reads as a flicker.

`_FOR_SUBAGENTS_ONLY` and `_FOR_SYSTEM_SPINNER` mean what they say. They're scoped to one surface, so they can be louder than the rest of the palette.

## The keys

### Claude & spinners

The assistant's own colour and the thinking spinner. `clawd_*` paint the mascot.

- `claude`
- `claudeShimmer`
- `claudeBlue_FOR_SYSTEM_SPINNER`
- `claudeBlueShimmer_FOR_SYSTEM_SPINNER`
- `clawd_body`
- `clawd_background`


### Modes & badges

The pills and labels for the mode you're in — auto-accept, plan mode, a running skill, fast mode, effort level.

- `autoAccept`
- `autoAcceptShimmer`
- `planMode`
- `skill`
- `fastMode`
- `fastModeShimmer`
- `effortUltra`
- `remember`
- `merged`
- `ide`


### Prompt & borders

The box you type into, and what a bash prompt or a selection looks like.

- `promptBorder`
- `promptBorderShimmer`
- `bashBorder`
- `suggestion`
- `selectionBg`


### Text & surfaces

Body text and the surfaces it sits on. `subtle` and `inactive` carry most of the greys.

- `background`
- `text`
- `inverseText`
- `subtle`
- `inactive`
- `inactiveShimmer`


### Status

Success, failure, warnings, and permission prompts.

- `success`
- `error`
- `warning`
- `warningShimmer`
- `permission`
- `permissionShimmer`


### Diffs

Edit previews. `Dimmed` is the quieter block fill, `Word` the intra-line highlight.

- `diffAdded`
- `diffAddedDimmed`
- `diffAddedWord`
- `diffRemoved`
- `diffRemovedDimmed`
- `diffRemovedWord`


### Message backgrounds

Per-message tints — your turns, bash output, memory notices, the composer sidebar.

- `userMessageBackground`
- `userMessageBackgroundHover`
- `composerSidebarBackground`
- `bashMessageBackgroundColor`
- `memoryBackgroundColor`


### Rate-limit bar

The usage meter: `fill` is consumed, `empty` is the track behind it.

- `rate_limit_fill`
- `rate_limit_empty`


### Transcript labels

The `You:` and `Claude:` labels.

- `briefLabelYou`
- `briefLabelClaude`


### Subagents

Reserved for subagent output only. Claude Code assigns one per agent, so give these eight visibly different hues.

- `red_FOR_SUBAGENTS_ONLY`
- `blue_FOR_SUBAGENTS_ONLY`
- `green_FOR_SUBAGENTS_ONLY`
- `yellow_FOR_SUBAGENTS_ONLY`
- `purple_FOR_SUBAGENTS_ONLY`
- `orange_FOR_SUBAGENTS_ONLY`
- `pink_FOR_SUBAGENTS_ONLY`
- `cyan_FOR_SUBAGENTS_ONLY`


### Rainbow ramp

A seven-stop ramp used for gradient effects, each with a brighter `_shimmer` partner.

- `rainbow_red`
- `rainbow_orange`
- `rainbow_yellow`
- `rainbow_green`
- `rainbow_blue`
- `rainbow_indigo`
- `rainbow_violet`
- `rainbow_red_shimmer`
- `rainbow_orange_shimmer`
- `rainbow_yellow_shimmer`
- `rainbow_green_shimmer`
- `rainbow_blue_shimmer`
- `rainbow_indigo_shimmer`
- `rainbow_violet_shimmer`


### Accents

Two standalone accents.

- `professionalBlue`
- `chromeYellow`


## Finding what a key paints

Set it to something violent — `#ff0000` — then run `/theme` and reselect your theme. The picker re-reads theme files from disk, so you can iterate without restarting Claude Code.

`/theme` also has a built-in editor that walks the full key list and writes the result to `~/.claude/themes/<slug>.json`. Building a theme there and committing the file it produces is usually faster than hand-writing JSON.
