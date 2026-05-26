# Interlux Icon Spec

> The contract any icon theme must follow to be considered an Interlux icon theme.

## Philosophy

Same idea as the rest of Interlux. There's a shared structural language every theme follows. Within that language, themes express personality through style choices: stroke weight, caps, fill style, level of detail. A user swapping between `interlux-icons` and a hypothetical `interlux-thin` should see the same set of glyphs, same meanings, same canvas size, different visual character.

This document defines the rules. `interlux-icons/` is the reference implementation.

## Scope

Interlux Icons is a renderer-neutral icon system. The reference implementation ships SVG for the icons themselves and CSS for the motion layer, which makes it directly consumable by web/HTML hosts. The system is designed to also be implementable in native code (Liminal in Zig, future ports to Swift/Compose/anything else) by following the contracts in this spec and [MOTION.md](MOTION.md).

Liminal/dusk is the primary consumer in the FynxLabs ecosystem, but nothing in the spec is Liminal-specific. The same theme should produce visually equivalent results in any compliant consumer.

## Core principles

1. **One canvas size.** All icons designed on a 16x16 viewBox. Scaling is the consumer's job.
2. **One color contract.** Use Interlux palette roles via CSS variables. Never hardcode hex without a CSS variable fallback.
3. **Same semantic name across themes.** No silent omissions. If a theme doesn't ship a variant, consumers fall back to the default theme.
4. **No competing detail.** Icons render at 16px. Detail that disappears at 16px does not belong in the file.
5. **Signal stability.** Warning is amber. Error is red. Success is green. Across every theme, every polarity.
6. **Composition over multiplication.** Standard state indicators (warning, error, unread, count, etc.) live as reusable overlays in `states/`. Hosts composite them on base icons at runtime. Avoid pre-rendering every base+state combination as separate files.

## Categories

An Interlux icon theme is a directory containing a `theme.skg` manifest and one subdirectory per category. The reference theme (`interlux-icons/`) ships 15 categories. The contract sorts them into three tiers.

### Tier 1: required

Every Interlux theme MUST ship these. They are the panel status set plus the composable overlay primitives. Missing any of them is a spec violation - the theme is not Interlux-compliant.

```
<theme-name>/
  theme.skg                              # required manifest
  states/
    warning-badge.svg                    # ! amber
    error-badge.svg                      # X red
    shield-badge.svg                     # shield accent (VPN active)
    dot-badge.svg                        # solid accent dot (unread)
    count-badge.svg                      # numbered accent badge
    silent-badge.svg                     # Z dim badge (DND)
  network/
    network-wired.svg                    # hub style
    network-wired-connecting.svg
    network-wired-warning.svg
    network-wired-disconnected.svg
    network-wired-vpn.svg
    network-wired-vpn-connecting.svg
    network-wired-vpn-connected.svg
    network-ethernet.svg                 # RJ45 port style alternative
    network-wireless.svg
    network-wireless-connecting.svg
    network-wireless-warning.svg
    network-wireless-disconnected.svg
    network-vpn-shield.svg
    network-vpn-shield-connecting.svg
    network-vpn-shield-connected.svg
    network-vpn-tunnel.svg
    network-vpn-tunnel-connected.svg
  audio/
    volume-muted.svg
    volume-low.svg
    volume-medium.svg
    volume-high.svg
  system/
    power.svg
    clock.svg
    bell.svg                             # default notification icon (outlined)
    bell-filled.svg                      # filled style alternative
    envelope.svg                         # alternative notification (messages)
    envelope-filled.svg
    bubble.svg                           # alternative notification (chat)
    bubble-filled.svg
    calendar.svg                         # default, no events
    calendar-event.svg                   # static accent dot (has events)
    calendar-now.svg                     # animated pulse (event live)
    calendar-heat.svg                    # parameterized heat ramp
    calendar-warning.svg
    calendar-error.svg
  bluetooth/
    bluetooth-active.svg
    bluetooth-connected.svg
    bluetooth-connecting.svg
    bluetooth-disabled.svg
    bluetooth-warning.svg
  battery/
    battery-empty.svg
    battery-low.svg
    battery-medium.svg
    battery-full.svg
    battery-charging.svg
    battery-warning.svg
    battery-dynamic.svg
```

#### Battery orientation

The required `battery-*` set defaults to **horizontal** orientation (terminal on the right). Themes MAY ship a parallel **vertical** set (`battery-vertical-empty.svg`, `battery-vertical-low.svg`, ..., `battery-vertical-charging.svg`, ..., `battery-vertical-dynamic.svg`) for hosts that prefer a portrait-axis battery glyph (e.g., tall panel slots, mobile-style status bars). When shipped, the vertical set MUST mirror the horizontal set's state contract one-for-one and use the same color bindings. The reference theme ships both orientations - hosts choose whichever fits their layout.

#### Battery fill style

The reference theme renders both orientations with a dim base path (`fill-opacity="0.3"` over the full body, terminal included) and a solid path on top covering the charged portion. Horizontal batteries split left (charged) / right (empty); vertical batteries split bottom (charged) / top (empty). State colors (warning amber, critical/error red, success green for charging) bind through `--ilx-warning`, `--ilx-error`, and `--ilx-success`. Themes MAY use a different fill style (outline + meter bar, segmented bars, etc.) as long as the level and state are unambiguous at 16px.

### Tier 2: recommended

These categories carry the rest of the Interlux first-party vocabulary - UI primitives, file ops, view modes, hardware, media, shapes, user. The reference theme ships all of them. Themes SHOULD ship them; if omitted, consumers fall back to `interlux-icons` for that category.

| Category | Purpose | Reference theme count |
|---|---|---|
| `ui/` | Plus, minus, x, check, arrows, chevrons, info, question, refresh, more-horizontal/vertical, external-link - outlined + filled pairs | 38 |
| `file/` | File, folder, folder-open, copy, cut (outlined + filled), paste, save, trash, edit, download, upload | 12 |
| `view/` | Eye / eye-slash, lock / unlock, zoom in/out, fullscreen, collapse, expand, grid, list, kanban | 12 |
| `hardware/` | Disk + interior, monitor, GPU, USB, USB drive, port plates (HDMI, DisplayPort, USB-C) | 11 |
| `media/` | Play, pause, stop, prev, next, skip-back/fwd, rewind, fast-forward, shuffle, repeat, repeat-one, music-note - outlined + filled pairs | 22 |
| `peripherals/` | Keyboard, mouse, speaker (front / side / mute / volume up / down) | 8 |
| `user/` | User, user-circle, user-filled, users, accessibility, privacy, privacy-mask | 7 |
| `shape/` | Circle, square, triangle, diamond, heart, star - outlined + filled pairs (status dots, ratings, decoration) | 12 |
| `windowlist/` | Windowlist variants (single window, tiled, default) | 3 |

### Tier 3: optional / future

Reserved category names. Themes MAY ship them; the spec defines the slot but not the contents yet. Once the reference theme adds a category here, it graduates to Tier 2.

- `display/` - brightness variants, monitor selection, screen lock
- `input/` - keyboard variants, mouse variants, mic, mic-muted
- `connectivity/` - airplane mode, hotspot, USB plug, removable media
- `workspace/` - workspace indicators (count variants 1-9 + overflow), layout switchers

### Theme completeness

A theme is **minimum-compliant** if it ships every Tier 1 file. A theme is **complete** if it ships every Tier 1 and Tier 2 file. The reference theme `interlux-icons/` is complete; see its directory for the canonical file inventory of each category.

## Color contract

All color references use CSS variables prefixed `--ilx-*`, mapped to Interlux palette roles. Each variable MUST have a hardcoded fallback so the SVG renders correctly outside an Interlux-aware host.

### Standard role variables

| Variable | Maps to palette role | Used for |
|---|---|---|
| `--ilx-text` | `text` | Default icon stroke and fill |
| `--ilx-bg` | `bg` | Background-aware cutouts (badge separators) |
| `--ilx-accent` | `accent` | Active or focused state highlight |
| `--ilx-dim` | `dim` | Inactive, secondary, or de-emphasized parts |
| `--ilx-success` | `success_tint` | Positive states |
| `--ilx-warning` | `warning` | Warning states |
| `--ilx-error` | `error` | Error / disabled / disconnected states |

### Pattern

```xml
<path stroke="currentColor" />
<rect fill="var(--ilx-error, #8d161e)" />
```

The default stroke and fill use `currentColor` so the SVG inherits color via the CSS cascade. Hosts set `color:` on the icon container, typically bound to a palette role:

```css
.icon-host {
  color: var(--ilx-text);  /* default text role for the glyph */
}
```

Role-specific colors (error red, warning amber, accent green) use `var(--ilx-*)` directly with hex fallback. This separation matters: `currentColor` enables CSS color animation (heat-warn, heat-error profiles), while role-specific variables are theme-controlled.

### Special-purpose variables

| Variable | Range | Used by | Purpose |
|---|---|---|---|
| `--ilx-battery-level` | 0.0 to 1.0 | Battery icons | Drives fill bar via scaleX transform |
| `--ilx-battery-fill` | color | Battery icons | Override fill color for state binding |
| `--ilx-calendar-heat` | 0.0 to 1.0 | calendar-heat.svg | Drives heat indicator opacity |
| `--ilx-calendar-heat-color` | color | calendar-heat.svg | Override heat indicator color (text -> warning -> error as event approaches) |
| `--ilx-warning-mode` | color \| badge \| both | Warning variants | Picks warning rendering mode |

## Outlined vs filled style

Some icons (notification metaphors like bell, envelope, bubble) ship in both outlined and filled variants. This is a style preference, not a state indicator. Hosts choose based on their design language:

- **Outlined** - lighter visual weight, the default for most interlux themes
- **Filled** - heavier weight, useful for emphasis or design languages that prefer solid icons

Filled variants always exist as separate files (e.g. `bell.svg` + `bell-filled.svg`), never as a CSS toggle. This keeps icons file-loadable without depending on host CSS interpretation.

The base name (e.g. `bell.svg`) defaults to outlined. The `-filled` suffix marks the filled alternative.

## State composition with overlays

The `states/` directory contains six standalone overlay badges, each positioned at the top-right of a 16x16 canvas. Hosts composite these on top of any base icon at runtime.

| Overlay | Indicates | Color binding |
|---|---|---|
| `warning-badge.svg` | Warning condition | `--ilx-warning` |
| `error-badge.svg` | Error / failed state | `--ilx-error` |
| `shield-badge.svg` | VPN active | `--ilx-accent` |
| `dot-badge.svg` | Unread / has activity | `--ilx-accent` |
| `count-badge.svg` | Numbered unread count | `--ilx-accent` |
| `silent-badge.svg` | Silent / Do Not Disturb | `--ilx-dim` |

### Composition example

```html
<div class="icon-host" style="position:relative">
  <img src="system/bell.svg">
  <img src="states/dot-badge.svg" style="position:absolute;inset:0">
</div>
```

Or in Liminal:

```zig
Stack {
  Svg.fromFile("system/bell.svg"),
  Svg.fromFile("states/dot-badge.svg"),
}
```

### Notes on count-badge

`count-badge.svg` ships with a default digit "3" as a placeholder. Hosts that want to show an actual count should either:

1. Substitute the `<text>` content programmatically when loading the SVG
2. Replace the badge entirely with a dynamically-generated one

The count is only legible at 24px and up. For 16px panels, prefer `dot-badge.svg`. At 16px the count circle becomes a colored blob - the count number is unreadable. The dot communicates "you have unread items" without lying about quantity.

## State variants

Standard states across applicable icons:

- **default** - normal operating state
- **active** - feature is on (e.g. bluetooth-active)
- **connected** - feature is on AND has a paired peer (e.g. bluetooth-connected)
- **connecting** - in-progress, animated breathing pulse
- **disabled** - feature is off (use error-badge overlay or pre-built disabled variant)
- **disconnected** - lost connection (use error-badge overlay or pre-built variant)
- **warning** - degraded but functional (use warning-badge overlay or pre-built variant)
- **charging** - battery-specific, fill animates upward

Some states are common enough to ship as pre-built variants (battery-warning, bluetooth-disabled). Others are best composed via overlays. Both patterns are valid; use whichever is clearer for your case.

## Animation

Two animation systems work together:

### Baked-in animations (per icon)

When animation IS the icon's meaning - the icon doesn't exist without it - the animation is baked into the SVG via CSS keyframes. Examples:

- `battery-charging.svg` - rising fill (the charging IS the animation)
- `network-vpn-tunnel.svg` - bouncing ball (the data flow IS the animation)
- `calendar-now.svg` - pulsing event indicator
- `*-connecting.svg` variants - breathing pulse during connection establishment

Baked animations run independently and don't require external CSS. They use CSS keyframes only, no SMIL, all on transform/opacity, all respect `prefers-reduced-motion`.

### Composable motion profiles (per host)

For ambient motion applied to otherwise-static icons (breathing notifications, heat ramps, attention pulses), the theme ships `motion.css` with 7 composable profiles:

| Profile | Behavior | Use case |
|---|---|---|
| `breathe` | Slow opacity oscillation | Ambient presence |
| `pulse` | Scale + opacity | Active attention |
| `heartbeat` | Double-pulse with pause | Critical urgency |
| `shimmer` | Opacity wave | Loading / syncing |
| `heat-warn` | Color cycles text -> warning | Escalating to warning |
| `heat-error` | Color cycles warning -> error | Critical color cycle |
| `glow-once` | One-shot flash | New event arrived |

Hosts apply these as CSS classes on the icon container:

```html
<div class="icon-host ilx-pulse">
  <img src="bell.svg">
  <img src="states/dot-badge.svg">
</div>
```

Native consumers (Liminal in Zig, others) implement the same behaviors against their own animation systems. The full contract for each profile - durations, easings, ranges, reduced-motion fallbacks - lives in [MOTION.md](MOTION.md).

### Heat-aware icons

Some icons accept an `--ilx-heat` variable (0.0 to 1.0) representing escalation. Hosts compute heat from underlying signals:

- Calendar: `(event_time - now) / threshold_window`
- Battery: `1.0 - battery_level`
- Network: `time_since_last_success / failure_threshold`

The host updates `--ilx-heat` and optionally applies escalating motion classes based on heat thresholds. See MOTION.md "Heat ramps" section for the recommended threshold mapping.

## Customization within icons

Some icons have customization points beyond the standard `--ilx-*` variables:

### Battery charging bolt

The bolt in `battery-charging.svg` uses two color slots:
- **fill**: defaults to `var(--ilx-bg)` for knockout-against-fill
- **stroke**: defaults to `var(--ilx-text)` for outline definition

Common overrides:

```xml
<!-- Solid yellow bolt -->
fill="#fabd2f" stroke="none"

<!-- Solid accent color -->
fill="var(--ilx-accent)" stroke="none"
```

### Calendar heat

`calendar-heat.svg` is fully parameterized:

```css
.calendar-host {
  --ilx-calendar-heat: 0.55;
  --ilx-calendar-heat-color: var(--ilx-warning);
}
```

Recommended progression:
- 0.0 (no events) - icon stays in default state
- 0.3 (event today) - color: `text`, opacity 0.3
- 0.55 (within the hour) - color: `warning`, opacity 0.55
- 0.8 (within 15 min) - color: `warning`, opacity 0.8
- 1.0 (now / imminent) - color: `error`, opacity 1.0

The host computes the heat value from `(event_time - now) / threshold_window` and updates the variables. The CSS `transition` smooths the visual change.

### VPN connected states

All `*-vpn-connected` variants use `--ilx-accent`. Override at host level to change the "VPN active" color globally without editing each file.

## Visual mass / optical sizing

Icons share the same 16x16 viewBox but their visual mass varies based on shape. A wifi icon fills almost the full canvas with thick arcs. A battery (a horizontal rectangle) naturally occupies less vertical space.

Compensate by sizing shapes to feel visually equivalent, not literally equivalent:

- Wide/short shapes (battery, rectangular icons): expand to fill more canvas. Battery rectangle uses 12x8 within 16x16.
- Round shapes (clock, power, bell): natural geometric center with padding.
- Tall shapes (wifi arcs, bluetooth): use most of the vertical space.

The goal is icons that feel the same weight when placed in a row at 16px. Test with a panel preview before shipping.

## Design specs (reference theme baseline)

| Spec | Value |
|---|---|
| Canvas | viewBox="0 0 16 16" |
| Default stroke | 1.25 |
| Heavy stroke | 1.5 (cap nubs) |
| Caps | round |
| Joins | round |
| Padding | ~1px from edges |
| Disabled opacity | 0.5 |
| Connecting pulse | opacity 0.4 to 1.0 |
| Badge center | (13.25, 2.75) |
| Badge radius | 2.5 |
| Dot-badge center | (13, 3), radius 2 |

## Authoring a new theme

1. Create `interlux/icons/<your-theme-name>/`.
2. Copy `theme.skg`, edit metadata.
3. Ship every required file.
4. Use only `--ilx-*` variables for color.
5. Validate at 16px first.
6. Test composition with overlays.
7. Don't break signal semantics. Warning is amber. Error is red.

## Color binding in consumers

```css
.icon-host {
  --ilx-text: var(--palette-text);
  --ilx-bg: var(--palette-bg);
  --ilx-accent: var(--palette-accent);
  --ilx-dim: var(--palette-dim);
  --ilx-success: var(--palette-success-tint);
  --ilx-warning: var(--palette-warning);
  --ilx-error: var(--palette-error);
  --ilx-warning-mode: both;
}
```

## Versioning

Spec follows semver. Breaking changes bump major. Additions bump minor. The "Categories" Tier 3 list is reserved-but-empty; promoting a name out of Tier 3 (i.e., the reference theme ships its files) is a minor bump.
