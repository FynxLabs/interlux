# Interlux

The FynxLabs design language. Shared roots, own personality, nothing competes.

Interlux is the visual identity every FynxLabs tool grows from - foundation colors, palette role names, signal semantics, and the rules any UI in the family follows. Each tool ships its own theme as its face (**Dusk**, **Umbra**, **Ember** in dark; **Dawn**, **Lustra**, **Kindle** in light), but they're all wearing Interlux underneath.

This is **reference**, not a shared module. Nothing here is imported by code. Each tool (dusk, umbra, tenebrux, ember, etc...) ships its own theme on its own terms - same roots, own personality. Read the docs here to know *what* the shared values and rules are, then make the thing yours.

**Scope:** this directory is for things that are true across every FynxLabs tool. Brand identity, foundation colors, palette roles, and the rules that keep every tool's UI feeling like part of the same family. Tool-specific feature design (wizard flows, installer pages, CLI syntax, module contracts) does **not** belong here - it lives in each tool's own `docs/`.

## What lives here

| Path                  | What                                                  |
| --------------------- | ----------------------------------------------------- |
| [`theming/`](theming) | Foundation colors, palette roles, themes, and rules   |

## The shape

- **Same roots.** Foundation colors (`bg`, `panel`, `text`) and system signals (`success_tint`, `warning`, `error`) are shared across the family. A user walking from umbra into dusk into tenebrux should feel the same house, not three.
- **Own personality.** Each tool picks its own accent family and ships it as the out-of-box theme. The personality is the tool's face. Themes are fully editable - the OOB is a starting point, never a cage.
- **Nothing competes.** If something feels loud, it should not exist.

## The family

**Dark side** - states of fading or low light.

| Theme     | Owner     | Accent          |
| --------- | --------- | --------------- |
| **Dusk**  | dusk      | teal-gray       |
| **Umbra** | umbra     | purple          |
| **Ember** | ember     | brick-orange    |

**Light side** - states of emerging or rising light.

| Theme      | Counterpart | Accent          |
| ---------- | ----------- | --------------- |
| **Dawn**   | Dusk        | teal-green      |
| **Lustra** | Umbra       | purple          |
| **Kindle** | Ember       | brick-orange    |

Each pair is a true mirror: Dusk ↔ Dawn (the two thresholds of day), Umbra ↔ Lustra (shadow cast vs. light cast, both Latin), Ember ↔ Kindle (fire dying vs. fire being born).
