# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a single-page static website for "La Villa Fleurie", a fictional Michelin-starred Creole gastronomic restaurant in Saint-Gilles-les-Bains, Réunion Island. The entire site lives in `index.html` — all CSS and JavaScript are inlined. There is no build system, package manager, test suite, or linter.

## Development

- No build or install step. Open `index.html` directly in a browser, or serve it locally (e.g. `python3 -m http.server`) to preview changes.
- The only external dependencies are Google Fonts (Manrope + Lora) and an embedded Google Maps iframe; everything else is self-contained, including SVG artwork embedded as data URIs.

## Architecture of index.html

The file is organized into clearly delimited blocks (marked with `/* ============ NAME ============ */` comments in CSS and `<!-- ============ NAME ============ -->` in HTML):

- **`<head>`**: French-language SEO meta tags and a Schema.org `Restaurant` JSON-LD block. Keep these in sync when changing restaurant details (address, phone, hours).
- **CSS design tokens**: All colors and fonts are CSS custom properties on `:root` (e.g. `--ivoire`, `--vert`, `--or`, `--font-titre`, `--font-corps`). Use these tokens instead of hardcoding colors. French names are used throughout for classes and variables (`.plat`, `.avis-card`, `--or-clair`).
- **Sections**: nav, hero, histoire, menu, chef, galerie, réservation, avis/presse, événements, contact, footer. Dark sections use the `.dark` class; scroll-reveal animation uses `.reveal` (with `.d1`/`.d2`/`.d3` delay modifiers) driven by an IntersectionObserver.
- **Placeholder imagery**: There are no photo assets. The `.ph` class (with variants `ph-a`–`ph-d`) renders branded gradient placeholders with an inline SVG orchid motif where photos would go.
- **Inline `<script>` at the bottom** handles all behavior:
  - Nav scroll state and mobile burger menu.
  - Scroll-reveal via IntersectionObserver.
  - The menu ("La Carte") is data-driven: dish data lives in the `menuData` object (categories `entrees`, `plats`, `desserts`, `accords`) and is rendered into `#menuGrid` by `renderMenu()`. To change dishes or prices, edit `menuData`, not the HTML.
  - The reservation form is front-end only: on valid submit it hides the form and shows the `#resaOk` confirmation. No data is sent anywhere.

## Conventions

- All user-facing content is in French; keep new copy in French and match the existing elegant, gastronomic tone.
- Preserve the existing accessibility patterns: `aria-label`/`aria-expanded` on interactive elements, `role="tab"`/`aria-selected` on menu tabs, `:focus-visible` styling, and the `prefers-reduced-motion` media query.
- Responsive breakpoints are at 960px and 820px (mobile nav); test layout changes against both.
