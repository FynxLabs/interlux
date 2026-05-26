# Icons

> One canvas, one color contract, swappable themes. Same idea as Adwaita / Papirus / Breeze - except the palette is Interlux and the signals never drift.

This is the icon layer of Interlux. Sibling to [`../theming/`](../theming): theming defines the palette roles (`text`, `accent`, `error`, etc.) and signal semantics; icons consume those roles through CSS variables and ship as drop-in themes any Interlux-aware tool can load.

**Live preview:** [fynxlabs.github.io/interlux](https://fynxlabs.github.io/interlux/) - every icon at every size, motion profiles, panel composition. Published from [`index.html`](index.html) via GitHub Pages.

| Path                                | What                                                                                   |
| ----------------------------------- | -------------------------------------------------------------------------------------- |
| [`SPEC.md`](SPEC.md)                | The icon-theme contract. Required categories, color variables, composition rules.      |
| [`MOTION.md`](MOTION.md)            | Motion-profile contract. Behavior specs for the seven shared profiles.                 |
| [`interlux-icons/`](interlux-icons) | Default theme. Reference implementation of the spec - ~218 icons across 15 categories. |
| [`index.html`](index.html)          | Self-contained browser showcase (all SVGs inlined). Deployed as the Pages site.        |

## The shape

- **One contract, many themes.** SPEC defines what an icon theme must ship and what signals must mean. Style (stroke weight, caps, fill, level of detail) is the theme's personality.
- **Palette-aware.** Icons consume Interlux palette roles via `--ilx-*` CSS variables. Switch palette (Dusk -> Umbra -> Ember) and the icons recolor with it. No second copy of the theme per palette.
- **Signal-stable.** `--ilx-error` is crimson, `--ilx-warning` is amber, `--ilx-success` is green - across every theme, every polarity. Users learn signals once.
- **Tool-agnostic.** SVG + CSS for web/HTML hosts. Native consumers (Liminal in Zig, future SwiftUI/Compose ports) implement the same contracts against their own renderer.
- **Composable state.** Six overlay badges in `states/` composite on top of any base icon at runtime instead of pre-rendering every base+state combination.

## How it fits with the rest of Interlux

- [`../theming/`](../theming) owns palette roles and signal values. The icon variables (`--ilx-text`, `--ilx-accent`, `--ilx-error`, ...) map 1:1 to those roles.
- Tools (dusk, ember, anything else) load icons through their rendering layer and bind palette roles to the variables at render time.
- The icon itself never knows what color it is. The host tells it.

## Authoring a new theme

Read [`SPEC.md`](SPEC.md). Short version:

1. New directory under `icons/<your-theme-name>/`.
2. Ship every required file at the right path.
3. Use only `--ilx-*` CSS variables for color, with Interlux-valid hex fallbacks.
4. Validate at 16px first.

A theme can express personality through stroke weight, caps, fill style, level of detail. It can NOT change which icons exist, what they mean, or what color signals carry.

## Attribution

Several glyphs are derived from third-party icon sets, sourced via [SVG Repo](https://www.svgrepo.com/) and re-fitted to the Interlux color contract (`currentColor` + `--ilx-*` variables). Paths are copied verbatim or simplified for the 16x16 canvas; underlying geometry is the original author's. Every other glyph in `interlux-icons/` is original to this project.

> **Licensing note:** the repo is [MIT](../LICENSE), but `system/linux.svg` is derived from the HashiCorp Design System and remains under MPL-2.0 (it is not relicensed). The full carve-out is documented in [`LICENSE`](../LICENSE).

| Glyph(s)                                                                                                                            | Author                                                                                 | License       | Source                               |
| ----------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------------- | ------------------------------------ |
| `file/cut.svg`, `file/cut-filled.svg`                                                                                               | [Ionicons](https://github.com/ionic-team/ionicons?ref=svgrepo.com)                     | MIT           | [SVG Repo](https://www.svgrepo.com/) |
| `system/stats-ram.svg`, `user/accessibility.svg`                                                                                    | [VMware Clarity](https://github.com/vmware/clarity-assets?ref=svgrepo.com)             | MIT           | [SVG Repo](https://www.svgrepo.com/) |
| `system/linux.svg`                                                                                                                  | [HashiCorp Design System](https://github.com/hashicorp/design-system/?ref=svgrepo.com) | MPL           | [SVG Repo](https://www.svgrepo.com/) |
| `hardware/usb.svg` (3-prong USB logo)                                                                                               | [Zondicons / Steve Schoger](https://www.zondicons.com/?ref=svgrepo.com)                | Public Domain | [SVG Repo](https://www.svgrepo.com/) |
| `peripherals/speaker.svg`                                                                                                           | [Icooon Mono](https://icooon-mono.com/?ref=svgrepo.com)                                | Public Domain | [SVG Repo](https://www.svgrepo.com/) |
| `network/network-ethernet.svg`, `hardware/disk-interior.svg`, `hardware/gpu.svg`, `hardware/usb-drive.svg`, `peripherals/mouse.svg` | (uncredited / public domain)                                                           | Public Domain | [SVG Repo](https://www.svgrepo.com/) |
