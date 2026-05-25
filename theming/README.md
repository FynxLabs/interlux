# Theming

> Dark gunmetal root and warm cream counterpart. One accent family per personality. Nothing fights for attention.

## Philosophy

Every FynxLabs tool shares the same structural logic. The base - dark or light - owns most of the screen. The accent color runs as a *family* - from dim structural tints through to the fully saturated active state - so every tool feels internally consistent rather than a neutral shell with a colored dot dropped in.

Each tool picks its own accent family and ships it as the face you see first. The face is 100% editable - what you get on day one is the starting point, never a lock-in. Users are expected to swap, override, and rewrite.

The family ships in two polarities - dark (**Dusk** / **Umbra** / **Ember**) and light (**Dawn** / **Lustra** / **Kindle**). Same accent families on both sides, same role contract; only the foundation flips.

The dark side is named for states of fading or low light - dusk, shadow, dying glow. The light side mirrors it with states of emerging or rising light - daybreak, polished gleam, first flame. Each pair is a true opposite within the same conceptual register.

## Shared values per polarity

Every dark theme uses the same `bg`, `panel`, `text`, and signal values; every light theme does the same on its side. This is a design discipline - each theme file redeclares them, but the values are identical across the trio.

## Three rules that never change

1. **Accent marks active and focused states only** - selections, focused borders, in-progress modules, "current step" indicators. Not decoration.
2. **Signals stay semantic across themes.** `success_tint` reads as green, `warning` reads as amber, `error` reads as red - the same across every theme in the same polarity. A theme's accent personality never changes what a signal means. Brick-orange Ember has the same green `success_tint` and amber `warning` as Dusk and Umbra, because signals carry meaning the user has to recognize instantly.
3. **Nothing in a UI competes.** If something feels loud, it should not exist.

## Foundation values

Shared per polarity. Every theme in the polarity uses these values for `bg`, `panel`, `text`, `success_tint`, `warning`, and `error`.

### Dark

Used by **Dusk**, **Umbra**, **Ember**.

| Role           | Hex       | What it's for                       |
| -------------- | --------- | ----------------------------------- |
| `bg`           | `#161618` | Root background - neutral dark gray |
| `panel`        | `#1e1e21` | Panel / card / box background       |
| `text`         | `#e0ddcf` | Primary text                        |
| `success_tint` | `#5a9470` | Success state - muted green         |
| `warning`      | `#c97e2a` | Warning state - amber               |
| `error`        | `#8d161e` | Error / destructive state - crimson |

### Light

Used by **Dawn**, **Lustra**, **Kindle**.

| Role           | Hex       | What it's for                       |
| -------------- | --------- | ----------------------------------- |
| `bg`           | `#f4f1e8` | Root background - warm cream        |
| `panel`        | `#ebe8df` | One step darker than `bg`           |
| `text`         | `#2a2c2e` | Primary text - deep slate           |
| `success_tint` | `#3d7858` | Success state - deepened green      |
| `warning`      | `#d4892f` | Warning state - amber               |
| `error`        | `#a01820` | Error / destructive state - crimson |

Breaking these values within a polarity is a loud move. Do it only when the tool has a real reason - and then the tool owns the consequences.

## Palette role contract

Every theme file declares all 13 roles. Tools implement them however their rendering layer needs - lipgloss styles in a TUI, CSS variables in a GUI, hex strings in a config generator. The **role names are the contract**.

**Foundation (shared per polarity):**

- `bg` - root background
- `panel` - one step up from `bg`, for cards and containers
- `text` - primary text
- `success_tint` - success signal, green
- `warning` - warning signal, amber
- `error` - error signal, crimson

**Accent family (where each tool expresses itself):**

- `accent` - active, focused, selected foreground
- `dim` - inactive text, separators, secondary labels
- `structure` - borders, inactive box backgrounds
- `selection_bg` - background for selected or highlighted items

**System icons (status indicators for tools that render them - wifi, battery, bluetooth, notifications, audio, etc.):**

- `icon_active` - icon's thing is on / connected / normal - the default visible state
- `icon_inactive` - icon's thing is soft-off (radio off, no connection)
- `icon_disabled` - icon's thing is hard-off (no device present, missing backend)

Icon roles are independent of the accent family on purpose: status icons need their own visual hierarchy distinct from "this thing is focused / selected." A wifi-connected indicator and a focused text field should not look the same.

## Tool personalities

Each FynxLabs tool ships its dark theme as its face, with a light counterpart in the same accent family. The theme name matches the owning tool when there is one. The tables below show each theme's accent family - foundation values come from the polarity tables above.

| Tool      | Dark      | Light      | Character                                                                    |
| --------- | --------- | ---------- | ---------------------------------------------------------------------------- |
| **dusk**  | **Dusk**  | **Dawn**   | Teal-gray accent - quiet, structural, the "no fights for attention" baseline |
| **umbra** | **Umbra** | **Lustra** | Purple accent - editorial, cooler than Dusk/Dawn                             |
| ember     | **Ember** | **Kindle** | Brick-orange accent - warm, firelight quality                                |

### Dusk

Owned by **dusk**, the X11 panel daemon. Dark polarity. Teal-gray accent that grows naturally out of the dark base.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#5a9470` |
| `dim`           | `#4d6a6a` |
| `structure`     | `#2e3035` |
| `selection_bg`  | `#1e3d2e` |
| `icon_active`   | `#e0ddcf` |
| `icon_inactive` | `#4d6a6a` |
| `icon_disabled` | `#2e3035` |

### Dawn

Light counterpart to Dusk. Light polarity. Same teal-green accent family, deepened for contrast against the cream foundation. Morning twilight to Dusk's evening twilight.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#3d7858` |
| `dim`           | `#6a7878` |
| `structure`     | `#d2cec0` |
| `selection_bg`  | `#d8e8d8` |
| `icon_active`   | `#2a2c2e` |
| `icon_inactive` | `#6a7878` |
| `icon_disabled` | `#d2cec0` |

### Umbra

Owned by **umbra**, the Linux installer. Dark polarity. Purple accent, editorial and cooler than Dusk. Neutral dark base - purple earns its place through accent roles only.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#7c6aaa` |
| `dim`           | `#555560` |
| `structure`     | `#2e2e32` |
| `selection_bg`  | `#241a38` |
| `icon_active`   | `#e0ddcf` |
| `icon_inactive` | `#555560` |
| `icon_disabled` | `#2e2e32` |

### Lustra

Light counterpart to Umbra. Light polarity. Same purple accent family, deepened for contrast against the cream foundation. Named for the Latin sense of polished, reflected light - the bright mirror to Umbra's cast shadow.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#5e4d8a` |
| `dim`           | `#6e6878` |
| `structure`     | `#d2cec0` |
| `selection_bg`  | `#e0d8ec` |
| `icon_active`   | `#2a2c2e` |
| `icon_inactive` | `#6e6878` |
| `icon_disabled` | `#d2cec0` |

### Ember

Owned by **ember**, the clipboard tool. Dark polarity. A warm brick-orange personality that reads distinctly different from Dusk and Umbra. Available for any other tool that wants to wear it.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#aa3d22` |
| `dim`           | `#5a4030` |
| `structure`     | `#2a2018` |
| `selection_bg`  | `#2e1a10` |
| `icon_active`   | `#e0ddcf` |
| `icon_inactive` | `#5a4030` |
| `icon_disabled` | `#2a2018` |

Ember's accent is brick-orange. Its `success_tint` is still green, `warning` still amber, `error` still crimson - same as Dusk and Umbra. Signals carry meaning; theme accent is personality.

### Kindle

Light counterpart to Ember. Light polarity. Same brick-orange accent family. Where Ember is the dying glow, Kindle is the first flame - the bright mirror to Ember's warm dim.

| Role            | Hex       |
| --------------- | --------- |
| `accent`        | `#a3361c` |
| `dim`           | `#8a6a55` |
| `structure`     | `#d2cec0` |
| `selection_bg`  | `#f0dac8` |
| `icon_active`   | `#2a2c2e` |
| `icon_inactive` | `#8a6a55` |
| `icon_disabled` | `#d2cec0` |

Same signal rule as Ember: `success_tint` green, `warning` amber, `error` crimson.

## Per-tool rendering maps

Tools that need to translate role names into third-party desktop configs (rofi, openbox, dunst, picom, terminal) keep those maps local in their own repo. Derive from the role names above in whatever shape the rendering layer wants.

## Colorblind variants

Deuteranopia / Protanopia and Tritanopia variants land here once the base personalities are stable and there's a real test rendering pipeline to validate against.
