# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**ElectroCalc** is a static, single-page Spanish-language electronics calculator website. No build system, no framework, no dependencies — just `index.html`, `css/styles.css`, and three SVG circuit diagrams in `img/`.

To develop: open `index.html` directly in a browser. There are no build, lint, or test commands.

## Architecture

Everything lives in a single `index.html`:

- **HTML** — Three `<section class="calc-section">` blocks (`#divisor`, `#ohm`, `#led`) plus a sticky nav, a 260px sidebar (ad placeholders), and a footer.
- **CSS** (`css/styles.css`) — Dark-mode-only design system using CSS custom properties (`--bg-dark`, `--bg-card`, `--bg-input`, `--accent`, etc.). Two-column grid layout (`main` + `.sidebar`) collapses to single-column below 768px.
- **JavaScript** — Inline `<script>` at the bottom of `<body>`, `'use strict'`. No modules, no bundler.

## Calculator Logic

All three calculators follow the same pattern:
1. Read inputs with `getPositive(id, unitId)` (returns `null` on invalid/non-positive values).
2. Show inline errors via `showError(errorId, msg)` / `clearError(errorId)`.
3. Populate result DOM nodes, then toggle visibility with `showResult(resultId)` / `hideResult(resultId)`.

**Divisor de Voltaje** — Inputs support unit selectors (V/mV, Ω/kΩ/MΩ). Formula: `Vout = Vin × R2 / (R1 + R2)`.

**Ley de Ohm** — Dynamic inputs rendered by `renderOhmInputs()` based on `ohmCalc` state (`'V'|'I'|'R'|'P'`). Mode is switched by `setOhmCalc()`. Always derives all four quantities (V, I, R, P) and displays them in a 2×2 grid.

**Resistencia para LED** — Color preset buttons call `setVf(v)`. Includes E24 series nearest-value lookup (`nearestE24(ohms)` using the `E24` constant array). Shows safety warnings for `If > 30mA` and resistor power `> 250mW`.

## Key Helpers

| Function | Purpose |
|----------|---------|
| `fmt(n, decimals)` | Formats numbers with M/k suffixes |
| `fmtOhm(r)` | Formats resistance as Ω / kΩ / MΩ |
| `nearestE24(ohms)` | Finds closest E24 standard resistor value |
| `getPositive(id, unitId)` | Reads and validates a positive number from an input, multiplied by its unit select |
| `getOhmVal(id)` | Like `getPositive` but allows zero (used by Ohm calc) |

## CSS Design Tokens

Defined on `:root` — change these to retheme the entire site:

```
--bg-dark   #1a1a2e   Page background
--bg-card   #16213e   Calculator card background
--bg-input  #0f3460   Input field background
--accent    #00d4ff   Primary highlight color
--error     #ff4d4d   Error messages
--warn      #ffd700   Warning messages
--success   #00ff99   E24 result highlight
```

## Extending the Site

To add a new calculator:
1. Add a `<section class="calc-section" id="new-id">` in `<main>` following the same structure as the existing sections.
2. Add a nav link `<li><a href="#new-id">Name</a></li>`.
3. Add the section ID to the `sections` array in the scroll-highlight JS block.
4. Add an SVG circuit diagram in `img/` and reference it at the bottom of the section.
