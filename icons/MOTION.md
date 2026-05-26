# Motion Contract

> Behavior specifications for Interlux icon motion profiles. Language and renderer neutral - implement in CSS, Zig, Swift, anywhere.

## Why a contract

The reference theme ships `motion.css` for web/HTML consumers. Native consumers (Liminal in Zig, future SwiftUI/Compose ports) implement the same behaviors against their own animation systems. This document is the source of truth for what each profile does so implementations stay consistent.

## Universal rules

Every motion profile MUST:

1. Run on opacity, transform, or color only. Never animate position, layout, or width/height attributes.
2. Loop infinitely unless the profile is explicitly one-shot.
3. Respect a "reduced motion" preference. When reduced motion is on, fall back to a static state defined per profile.
4. Be GPU-compositable (CSS) or use the rendering system's spring/tween primitive (Liminal Effects).
5. Be applicable to:
   - The whole icon (default)
   - Only an overlay badge
   - Only the glyph (excluding overlays)
   - Both simultaneously
   Target via the `--ilx-motion-target` variable or equivalent runtime parameter.

## Profile: `breathe`

Slow, ambient opacity oscillation. Conveys "alive but idle."

| Attribute               | Value                       |
| ----------------------- | --------------------------- |
| Duration                | 2.0s                        |
| Easing                  | ease-in-out                 |
| Direction               | alternate (oscillates back) |
| Opacity range           | 0.5 to 1.0                  |
| Reduced motion fallback | static at opacity 0.85      |

**Use for:** ambient presence, soft notifications, "I'm here" indicators.

## Profile: `pulse`

Combined opacity + scale oscillation. Conveys "active attention now."

| Attribute               | Value                            |
| ----------------------- | -------------------------------- |
| Duration                | 1.4s                             |
| Easing                  | ease-in-out                      |
| Direction               | alternate                        |
| Opacity range           | 0.7 to 1.0                       |
| Scale range             | 0.92 to 1.08                     |
| Transform origin        | center                           |
| Reduced motion fallback | static at opacity 1.0, scale 1.0 |

**Use for:** active events, "this is happening right now," current step in a sequence.

## Profile: `heartbeat`

Two quick pulses followed by a longer pause. Conveys "critical, demands attention."

| Attribute               | Value                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------ |
| Duration                | 1.5s                                                                                             |
| Easing                  | ease-out                                                                                         |
| Direction               | normal (not alternate, repeats from start)                                                       |
| Keyframes               | 0% scale(1.0); 14% scale(1.12); 28% scale(1.0); 42% scale(1.12); 56% scale(1.0); 100% scale(1.0) |
| Reduced motion fallback | single one-shot scale(1.0) -> scale(1.1) -> scale(1.0) on state entry only                       |

**Use for:** critical alerts, emergency conditions, "you need to act."

## Profile: `shimmer`

Subtle traveling opacity wave across the icon. Conveys "working / loading / syncing."

| Attribute               | Value                                                                                                    |
| ----------------------- | -------------------------------------------------------------------------------------------------------- |
| Duration                | 1.8s                                                                                                     |
| Easing                  | linear                                                                                                   |
| Direction               | normal (no alternate)                                                                                    |
| Implementation          | Linear gradient mask traveling left-to-right; or opacity wave on each child element with staggered delay |
| Opacity range           | 0.6 to 1.0 (per element)                                                                                 |
| Reduced motion fallback | static at opacity 0.85                                                                                   |

**Use for:** loading/syncing states, background processing, "calculating."

## Profile: `heat-warn`

Color shifts from text -> warning -> text over the cycle. Conveys "escalating, getting warmer."

| Attribute               | Value                                                            |
| ----------------------- | ---------------------------------------------------------------- |
| Duration                | 3.0s                                                             |
| Easing                  | ease-in-out                                                      |
| Direction               | alternate                                                        |
| Color keyframes         | 0% var(--ilx-text); 50% var(--ilx-warning); 100% var(--ilx-text) |
| Property animated       | stroke (and fill where applicable)                               |
| Reduced motion fallback | static at var(--ilx-warning)                                     |

**Use for:** events approaching a warning threshold, batteries getting low, deadlines closing in.

## Profile: `heat-error`

Color shifts from warning -> error -> warning faster. Conveys "critical, past warning."

| Attribute               | Value                                                                |
| ----------------------- | -------------------------------------------------------------------- |
| Duration                | 1.0s                                                                 |
| Easing                  | ease-in-out                                                          |
| Direction               | alternate                                                            |
| Color keyframes         | 0% var(--ilx-warning); 50% var(--ilx-error); 100% var(--ilx-warning) |
| Property animated       | stroke (and fill where applicable)                                   |
| Reduced motion fallback | static at var(--ilx-error)                                           |

**Use for:** critical conditions, emergencies, "act now."

## Profile: `glow-once`

One-shot brightness flash. Fires on state entry, then stops.

| Attribute               | Value                                                                         |
| ----------------------- | ----------------------------------------------------------------------------- |
| Duration                | 0.6s                                                                          |
| Easing                  | ease-out                                                                      |
| Direction               | one-shot, no loop                                                             |
| Implementation          | scale 1.0 -> 1.15 -> 1.0 AND opacity 0.6 -> 1.0 -> 1.0                        |
| Trigger                 | state entry only (CSS: applied via class-add; Liminal: spring fired on event) |
| Reduced motion fallback | no animation, set opacity to 1.0                                              |

**Use for:** "new event just arrived," confirmation feedback, transient acknowledgment.

## Motion targets

Each profile can be scoped to part of the composed icon stack:

| Target    | What gets animated                       | Use case                             |
| --------- | ---------------------------------------- | ------------------------------------ |
| `glyph`   | The base icon SVG only (overlays static) | Subtle "the bell itself is alive"    |
| `overlay` | Only the badge overlays (glyph static)   | "The dot is breathing, bell is calm" |
| `both`    | Glyph + overlays animate together        | Maximum attention                    |
| `none`    | No animation                             | Explicit static override             |

Default: `glyph`.

## Heat ramps - automated profile selection

For icons that should escalate over time (calendar approaching event, battery dying, network failing), expose a `--ilx-heat` variable (0.0 to 1.0). Implementations bind heat thresholds to profiles automatically:

| Heat range | Profile                 | Color binding             |
| ---------- | ----------------------- | ------------------------- |
| 0.0 - 0.3  | none                    | var(--ilx-text)           |
| 0.3 - 0.5  | breathe                 | var(--ilx-text)           |
| 0.5 - 0.7  | heat-warn               | (animation handles color) |
| 0.7 - 0.9  | pulse                   | var(--ilx-warning)        |
| 0.9 - 1.0  | heartbeat or heat-error | (animation handles color) |

The host computes `heat` from the underlying signal (event proximity, battery level inverted, time since failure) and updates the variable. The CSS layer maps thresholds to motion automatically. Native implementations do the same mapping in code.

## Composition with overlays

Motion profiles and overlays are orthogonal. Common combinations:

| Combo                                      | Meaning                              |
| ------------------------------------------ | ------------------------------------ |
| Base + `dot-badge` + breathe               | Subtle unread indicator, ambient     |
| Base + `warning-badge` + pulse             | Warning condition, demands attention |
| Base + `error-badge` + heartbeat           | Critical error, urgent               |
| Base + `count-badge` + glow-once on update | New notification just arrived        |
| Base + heat-warn (no overlay)              | Single icon escalating in place      |

## Implementation notes

### CSS path (web/HTML)

Profiles ship as classes in `motion.css`. Apply to the icon container:

```html
<div class="icon-host ilx-pulse" style="--ilx-motion-target: overlay">
  <img src="bell.svg">
  <img src="states/dot-badge.svg">
</div>
```

### Liminal path (Zig)

Profiles map to `liminal.widget.Animation` configurations against the SVG widget's draw properties. The Effects system handles `prefers-reduced-motion`. Map filename suffix or theme.skg metadata to profile names:

```zig
const profile = ilx.motion.profileFor(icon_name); // returns .breathe, .pulse, etc.
const anim = profile.toLiminalAnimation();
self.icon_widget.applyAnimation(anim);
```

### Other consumers

Any consumer implementing this spec follows the duration, easing, and property rules above. The visual result MUST be perceptually equivalent across implementations. Differences in implementation language don't excuse differences in user-visible behavior.

## Versioning

This contract is part of the Interlux Icon Spec. Breaking changes (renamed profiles, changed durations, removed targets) bump the spec major version. New profiles bump the minor.
