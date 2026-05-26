# Interlux

The FynxLabs design language. Shared roots, own personality, nothing competes.

Interlux is the visual identity every FynxLabs tool grows from - foundation colors, palette role names, signal semantics, the icon vocabulary, and the rules any UI in the family follows. Each tool ships its own theme as its face (**Dusk**, **Umbra**, **Ember** in dark; **Dawn**, **Lustra**, **Kindle** in light), but they're all wearing Interlux underneath.

This is **reference**, not a shared module. Nothing here is imported by code. Each tool (dusk, umbra, tenebrux, ember, etc...) ships its own theme on its own terms - same roots, own personality. Icons follow a spec that any theme can satisfy and any tool can consume. Read the docs here to know *what* the shared values, vocabulary, and rules are, then make the thing yours.

**Scope:** this directory is for things that are true across every FynxLabs tool. Brand identity, foundation colors, palette roles, the icon vocabulary, and the rules that keep every tool's UI feeling like part of the same family. Tool-specific feature design (wizard flows, installer pages, CLI syntax, module contracts) does **not** belong here - it lives in each tool's own `docs/`.

## What lives here

| Path                  | What                                                                                  |
| --------------------- | ------------------------------------------------------------------------------------- |
| [`theming/`](theming) | Foundation colors, palette roles, themes, and rules                                   |
| [`icons/`](icons)     | Icon-theme contract, motion contract, and `interlux-icons` (the default icon theme)   |

**Live preview:** the icon showcase is deployed at [fynxlabs.github.io/interlux](https://fynxlabs.github.io/interlux/) - every icon at every size, motion profiles, panel composition.

## The shape

- **Same roots.** Foundation colors (`bg`, `panel`, `text`) and system signals (`success_tint`, `warning`, `error`) are shared across the family. A user walking from umbra into dusk into tenebrux should feel the same house, not three.
- **Own personality.** Each tool picks its own accent family and ships it as the out-of-box theme. The personality is the tool's face. Themes are fully editable - the OOB is a starting point, never a cage.
- **One icon vocabulary.** Every tool draws from the same set of glyphs with the same meanings, the same canvas, and the same signal colors. Themes can change style (stroke weight, caps, fill) but never which icons exist or what they mean.
- **Nothing competes.** If something feels loud, it should not exist.

## Two layers, one language

Interlux ships as two parallel contracts:

- **[`theming/`](theming)** is the color contract. Thirteen palette roles, two polarities (dark / light), one shared foundation per polarity, an accent family per tool. Signals (`success_tint` / `warning` / `error`) carry stable meaning across every theme.
- **[`icons/`](icons)** is the visual-vocabulary contract. One canvas (16x16), one color contract (`--ilx-*` variables bound to palette roles), composable state overlays, seven motion profiles. The reference theme `interlux-icons` is shipped here; new icon themes drop in by following [`icons/SPEC.md`](icons/SPEC.md).

The two layers compose. An icon never knows what color it is - the host (tool's renderer) binds `--ilx-*` to the active palette role and the icon recolors automatically when the user switches themes. The signal-stable rule lives in both layers: `error` is crimson in every theme; `--ilx-error` resolves to that crimson in every theme. Users learn signals once.

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

## License

[MIT](LICENSE), with one exception: [`icons/interlux-icons/system/linux.svg`](icons/interlux-icons/system/linux.svg) is derived from the HashiCorp Design System and remains under MPL-2.0. Other third-party glyphs are also MIT-compatible or public domain; see the third-party section of [LICENSE](LICENSE) and the [icons attribution table](icons/README.md#attribution) for the full list.
