# Darz design system

The look and feel of darz.pages.dev, the site for our Darjeeling trip (20–25 Dec 2026). Read this before changing the page.

## Idea

**A day in the hills, from dawn to night.** The light theme is dawn on Tiger Hill: cool mist, tea-garden green and a little sunrise orange. The dark theme is night on Chowrasta: deep pine-blue, moonlit snow on Kanchenjunga and warm lamplight.

Each motif comes from the trip itself:

| Motif | Where it appears |
| --- | --- |
| Kanchenjunga ridge (3 layered ranges, snow on the highest) | Hero, section dividers, footer |
| Prayer flags (blue, white, red, green, yellow) | Strung across the hero, and on the active nav item |
| Tiger Hill sunrise / moon and stars | Hero sky: sun in light theme, moon and stars in dark |
| Drifting mist | Hero, slow horizontal drift |
| Toy train (DHR steam engine) | Train section icon, ticket stub |
| Hairpin road and Batasia-style loop | The NJP → Darjeeling route map |
| Tea leaf | Day markers on the itinerary |
| Railway ticket | The train section is a ticket with a perforated stub |

Keep motifs drawn in flat shapes with no gradients except the sky. Nothing photographic.

## Colour

All colours are tokens on `:root`. Components never use literal colours.

| Token | Light (dawn) | Dark (night) | Use |
| --- | --- | --- | --- |
| `--bg` | `#f2f6f3` | `#0e1513` | Page |
| `--surface` | `#ffffff` | `#16201c` | Cards |
| `--surface-2` | `#e9f0eb` | `#1c2823` | Insets, chips |
| `--fg` | `#15221b` | `#e3ece6` | Text |
| `--muted` | `#566960` | `#91a399` | Secondary text |
| `--line` | `#d8e2db` | `#26332d` | Borders |
| `--tea` | `#2c6a49` | `#84c8a1` | Accent: links, active, our pick |
| `--tea-soft` | `#dfede4` | `#1b3026` | Accent backgrounds |
| `--sun` | `#e07f3c` | `#f2a766` | Second accent, used sparingly: sun, lamp, "to fill" tags |
| `--sun-soft` | `#fbe6d6` | `#3a2817` | Background for `--sun` |
| `--warn` / `--bad` | `#9a6a12` / `#a3402f` | `#e2b866` / `#e8907e` | Budget pills only |
| `--sky-top` → `--sky-bottom` | `#d9e6ee` → `#f7ebdf` | `#0a1020` → `#16222b` | Hero sky gradient |
| `--ridge-far`, `--ridge-mid`, `--ridge-near` | `#c3d2d6`, `#9cb3ab`, `#6c8d7d` | `#18222a`, `#1d2a25`, `#25362e` | Mountain layers |
| `--snow` | `#ffffff` | `#cfdbe4` | Snow caps |
| `--flag-1…5` | blue, white, red, green, yellow | same hues, 15% darker | Prayer flags |
| `--ticket` | `#fbf7ee` | `#1d2721` | Train ticket paper |

Rules:

- `--tea` is the only accent for interactive things. `--sun` is decoration and placeholders, never a button.
- Text on `--tea-soft` or `--sun-soft` uses `--tea` or `--sun`'s darker partner, and must reach 4.5:1 contrast.
- Dark mode is designed, not inverted: snow stays light, ridges go darker than the sky.

## Theme switching

- Default follows the device (`prefers-color-scheme`).
- A sun/moon button in the top bar overrides it by setting `data-theme="light"` or `"dark"` on `<html>`. The choice is saved in `localStorage` (wrapped in try/catch, since it can fail).
- The switch animates with a circular reveal from the button using the View Transitions API. Browsers without it, or with reduced motion, switch instantly.
- CSS shape: light tokens on `:root`, dark tokens under `@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) }` and again under `:root[data-theme="dark"]`.

## Type

| Role | Font | Notes |
| --- | --- | --- |
| Display | Bricolage Grotesque 600/800 | Headings, hero, prices. `text-wrap: balance`. |
| Body | Source Sans 3 400/600/700 | Everything else, 16px base, 1.55 line height |
| Ticket | IBM Plex Mono 500 | Times, PNR, train numbers, prices in the ticket |

Scale: 0.78 (labels) · 0.9 · 1 · 1.15 · 1.5 · 1.9 · clamp(2.6–4.4rem) for the hero. Uppercase labels get 0.08em letter-spacing. Digits that line up use `tabular-nums`.

## Layout and mobile

Mobile first. Designed at 390px, then widened.

- One column, max 780px, side gutter `max(16px, safe-area inset)`.
- **Phones:** a bottom tab bar with 7 icon + label tabs (Plan, Train, Uphill, Stay, Places, Food, Pack). Each tap target is at least 44px tall. The bar pads for the home indicator with `env(safe-area-inset-bottom)`.
- **Tablets and desktop (≥ 760px):** the tabs move to a sticky top bar under the hero.
- The theme button sits top-right in a small fixed top bar on every size.
- No horizontal page scroll anywhere. Comparisons are stacked cards, not wide tables.
- Cards: 14px radius, 1px `--line` border, no shadow. Only "our pick" gets a `--tea` ring.

## Icons

One SVG sprite at `assets/icons.svg`, used with `<svg class="i"><use href="assets/icons.svg#i-name"/></svg>`.

- 24×24 grid, 2px padding, 1.75 stroke, round caps and joins.
- No colours inside the sprite: icons take `currentColor` from the page.
- Drawn for this trip: toy train, hairpin road, momo, tea cup, Batasia loop, monastery, cable car, prayer flags and so on.

## Motion

| Token | Value | Use |
| --- | --- | --- |
| `--ease` | `cubic-bezier(.2, .8, .2, 1)` | Almost everything |
| `--fast` | 160ms | Hover, press |
| `--base` | 280ms | Open and close, tab changes |
| `--slow` | 700ms | Hero entrance, reveals |

Moments:

1. **Hero entrance:** ridges rise in one after another, then the sun rises (or stars fade in at night). The flags sway gently and the mist drifts slowly, forever.
2. **Scroll reveal:** cards ease up 12px as they enter. Content is visible without JavaScript, and the effect only runs once the script has marked the page ready.
3. **Active tab:** a small pill slides between nav items.
4. **Details:** option cards open with a smooth height change where the browser supports it.
5. **Theme switch:** a circular reveal.
6. **Press feedback:** buttons and tabs scale to 0.97 on press.

All of it turns off under `prefers-reduced-motion: reduce`.

## Writing

- Plain, friendly, second person plural ("we", "us"). Short sentences.
- Unknown details use the `To fill` tag (`--sun` on `--sun-soft`) so gaps are easy to spot.
- Prices always say whether they're per person or for all 6, and whether they're checked or estimated.
