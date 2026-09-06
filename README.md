# School EdTech Implementation Roadmap

A single-file, interactive HTML page that walks a school through a practical, teacher-first framework for adopting education technology — from diagnosing real problems through piloting, measuring, scaling, and periodically retiring tools that no longer earn their place.

No build step, no dependencies, no server required. Open `edtech-roadmap-v2.html` in any modern browser and it works.

## Contents

- [What's inside](#whats-inside)
- [Getting started](#getting-started)
- [File structure](#file-structure)
- [Feature guide](#feature-guide)
- [Customizing the content](#customizing-the-content)
- [Browser support](#browser-support)
- [Accessibility](#accessibility)
- [Known limitations](#known-limitations)
- [Changelog](#changelog)

## What's inside

The page bundles six interactive tools around a single piece of instructional content (the six-stage roadmap):

| Section | Purpose |
|---|---|
| **Six-Stage Roadmap** | An accordion walking through Diagnose → Select → Pilot → Measure → Scale → Review, each stage tied to a rough timeline. |
| **90-Day Action Plan** | A checklist broken into five phases (Audit, Prioritise, Pilot, Measure, Decide) with a live progress bar. |
| **EdTech Scorecard** | Ten weighted sliders (0–10) that roll up into a total score and a plain-language verdict. |
| **The Leadership Rule** | A small calculator enforcing "for every new process added, remove or simplify one" — shows whether you're adding or reducing complexity. |
| **Readiness Diagnostic** | A 5-question quiz that produces a readiness score out of 15 and tailored advice. |
| **Case Study Simulator** | A dropdown of four intervention types, each showing sample impact metrics (time saved, outcome change, satisfaction change). |

## Getting started

1. Download `edtech-roadmap-v2.html`.
2. Double-click it, or open it via **File → Open** in any browser.
3. That's it — everything runs client-side in vanilla HTML/CSS/JS. No installation, no internet connection needed after the initial download.

To host it (e.g. for a staff-facing intranet page), just upload the single `.html` file to any static file host, SharePoint, or Google Drive-linked site — no server-side processing is required.

## File structure

This is intentionally a **single self-contained file**:

```
edtech-roadmap-v2.html
├── <style>   — all CSS, using custom properties (--primary, --gray-500, etc.) for theming
└── <script>  — all JavaScript, wrapped in an IIFE to avoid polluting the global scope
```

There are no external assets, fonts, or scripts to manage. If you need to embed it elsewhere, copy the whole file — it isn't designed to be split apart.

## Feature guide

### Stage tracker & accordion
Click any numbered circle in the top tracker to jump to that stage; the matching accordion panel opens and the page scrolls to it. You can also click a stage's header directly — clicking an **open** stage collapses it, clicking a **closed** one opens it and closes the others. The tracker's "done" highlighting always reflects whichever stage is currently open.

### 90-Day Action Plan
Check off tasks as you complete them. The counter and progress bar update automatically. Use **Complete all** / **Reset all** to bulk-toggle every checkbox — handy for demos or starting a new term.

### Scorecard
Drag any slider to rate a technology on that criterion; the total (out of 100) and verdict update live. **Reset to defaults** restores the original sample values; **Randomise scores** is mostly useful for demoing what different verdict bands look like.

### Leadership Rule calculator
Use the **+ / −** buttons to record how many new digital processes you're introducing versus how many existing ones you're removing or simplifying. The net indicator turns green (streamlining), amber (balanced), or red (adding complexity).

### Readiness Diagnostic
Answer all five questions (each button is worth 0, 1, or 3 points) to get an automatic readiness score and advice. **Reset** clears all answers.

### Case Study Simulator
Pick an intervention type from the dropdown to see illustrative time-saved, outcome-improvement, and satisfaction figures. Hover — or tab to and focus — any metric to see a one-line explanation of what it measures.

## Customizing the content

Everything is plain HTML/CSS/JS, so most edits are copy-paste-and-tweak:

- **Roadmap text**: edit the `<div class="stage-content" id="stage-N">` blocks directly.
- **90-day tasks**: edit the `<li><label><input type="checkbox" data-plan="...">...` lines; update `planTotal`'s default text (`12`) if you change the number of checkboxes — it's recalculated automatically at runtime regardless, but the initial HTML value is just a placeholder.
- **Scorecard criteria**: edit the `.score-item` blocks and update the `defaults` array in `resetScorecard()` in the `<script>` to match.
- **Case study numbers**: edit the `caseData` object near the bottom of the `<script>`.
- **Colors/theme**: everything is driven by the CSS custom properties in `:root` (`--primary`, `--success`, `--danger`, etc.) — change them once and the whole page re-themes.

No build tools are involved, so changes take effect the moment you save and reload the file.

## Browser support

Built with standard, widely supported features: CSS custom properties, flexbox/grid, `:focus-visible`, `aria-live` regions, and vanilla ES6 JavaScript (arrow functions, `const`/`let`, template literals). Works in current versions of Chrome, Edge, Firefox, and Safari. No polyfills are included, so very old browsers (IE11 and earlier) are not supported.

## Accessibility

- Roadmap stages and tracker steps are keyboard-operable (`Tab`, `Enter`, `Space`).
- `aria-expanded`, `aria-label`, and `role="button"`/`role="group"` are used throughout to describe interactive state to screen readers.
- Score, plan-progress, leadership-rule, and diagnostic sections each include a visually hidden `aria-live="polite"` region so screen reader users get spoken updates without excessive chatter (slider announcements fire on release, not on every drag tick).
- Case study metrics are focusable and show their explanatory tooltip on both hover and keyboard focus.
- `prefers-reduced-motion` is respected — animations and transitions collapse to near-zero duration for users who've requested reduced motion at the OS level.

## Known limitations

- All interactive state (checkboxes, slider values, diagnostic answers) lives in memory only — refreshing or closing the page resets everything. There's no `localStorage`/`sessionStorage` persistence or backend, by design, so this file can be opened from anywhere without wiring up storage.
- The Case Study Simulator uses illustrative sample figures, not live data from any real school — treat it as a discussion aid, not a validated benchmark.
- Content (roadmap stages, scorecard criteria, etc.) is in English only; there's no built-in localization.

## Changelog

**v2 (current)**
- Removed unwanted auto-scroll on initial page load.
- Restored true accordion behavior (clicking an open stage collapses it) and kept the tracker nav in sync in all cases.
- Added keyboard focus support and accurate tooltip copy for the Case Study metrics.
- Debounced the scorecard's screen-reader live-region updates to fire on slider release rather than every drag tick.
- Added `aria-live` regions across the plan, scorecard, leadership rule, and diagnostic sections.
- General cleanup: removed dead markup/CSS, consolidated repeated inline styles into reusable `.highlight-box` classes, wrapped script in an IIFE.

**v1**
- Initial interactive build: six-stage accordion roadmap, 90-day checklist, scorecard, leadership rule calculator, readiness diagnostic, and case study simulator.
