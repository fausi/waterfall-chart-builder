---
name: waterfall-chart
description: >-
  Build custom waterfall charts (bridge charts) as a self-contained, interactive
  HTML file with a dark, gold-accented theme. A waterfall chart shows how an
  initial value is changed by a sequence of positive and negative steps to reach
  a final total — ideal for financial bridges, metric decompositions, budget
  variance, KPI walk-throughs, and quarter-over-quarter progressions. Use this
  skill whenever the user asks for a "waterfall chart", "bridge chart", "walk
  chart", or wants to visualize cumulative step-by-step changes with running
  totals, checkpoint bars, and target/reference lines — even if they don't say
  the words "waterfall" (e.g. "show how we get from 13% to 40% across these
  initiatives", "chart the bridge from last quarter's revenue to this quarter's",
  "visualize each driver's contribution to the total"). Also use it when the user
  wants an editable chart-builder tool rather than a one-off static image.
---

# Waterfall Chart Builder

This skill produces a single, self-contained HTML file: an interactive waterfall
chart builder with a live editor on the left and a rendered chart on the right.
The user can adjust everything in-browser and export the result as PNG or SVG.

## What a waterfall chart is

A waterfall (or "bridge") chart starts from a baseline value and applies a series
of increments and decrements, each shown as a floating bar, arriving at one or
more running totals. Bars come in four types:

- **start** — the opening baseline bar, drawn from zero.
- **positive** — an upward step; the bar floats from the current running total up
  by its value.
- **negative** — a downward step; the bar floats down from the current running
  total by its value.
- **end** — a checkpoint/total bar drawn from zero to the current running total
  (or to an explicit value if one is given). Running total continues afterward.

## Workflow

1. **Start from the template.** Copy `assets/template.html` to the working
   directory. It is a complete, working builder — do not rewrite it from scratch.
   Your job is to swap in the user's data and (if requested) adjust theming.

2. **Read the user's data** into the segment model. Each segment is:
   `{ label, value, type, note }` where `type` is one of
   `start | positive | negative | end`, and `note` is the short text drawn above
   the bar (e.g. `+1.72`, `25%`). For `end` bars, set `value: null` to auto-fill
   the running total, or a number to force a specific height.

3. **Edit the `segs` array** near the top of the `<script>` block (search for
   `let segs = [`). Replace the sample data with the user's. Keep the shape
   exactly. Example:
   ```javascript
   let segs = [
     { label:'Baseline',     value:120, type:'start',    note:'120'  },
     { label:'New sales',    value:45,  type:'positive', note:'+45'  },
     { label:'Churn',        value:18,  type:'negative', note:'-18'  },
     { label:'End of Q',     value:null,type:'end',      note:'147'  },
   ];
   ```

4. **Edit the `targets` array** (search for `let targets = [`) for horizontal
   reference lines. Each is `{ label, value, color }`. Remove it entirely (empty
   array `[]`) if the user wants no reference lines.

5. **Set the title, subtitle, and eyebrow** by editing the three `value="..."`
   attributes on the `#cTitle`, `#cSub`, and `#cEyebrow` inputs in the HTML.

6. **Adjust default colours** only if the user asks, by editing the `value="..."`
   on the colour `<input type="color">` elements (`#cStart`, `#cPos`, `#cNeg`,
   `#cEnd`, `#cBg`, `#cText`). The default palette is a dark theme with a gold
   (`#F5A623`) accent and a red (`#E2504A`) negative colour.

7. **Save to the outputs directory and present the file.** Do not paste the full
   HTML into chat — deliver the file.

## Rules that keep the chart correct

These are already implemented in the template; preserve them if you edit the code.

- **Reference lines are drawn manually**, as full-width horizontal strokes inside
  the chart plugin — NOT as Chart.js line datasets. Line datasets start at the
  first category center (not the axis) and produce phantom lines. Keep the chart
  to two bar datasets only (an invisible base + the visible bars).
- **The y-axis maximum** accounts for both the tallest bar and the highest
  reference line, plus 5 units of headroom, so bar-top labels never clip:
  `Math.ceil(max(dataMax, refMax) / 5) * 5 + 5`.
- **X-axis labels never overlap.** They are rendered as a custom SVG overlay (the
  built-in Chart.js x ticks are disabled) and staggered across up to three rows
  by a greedy algorithm, with dashed connectors from staggered rows back to the
  axis.
- **Reference-line labels** sit at the left edge just above each line and are
  nudged apart vertically when they would collide, with a dashed tick back to the
  line when nudged.
- **PNG and SVG exports** reproduce all of the above (title, eyebrow, bars,
  notes, reference lines, staggered labels) so downloads match the preview.

## Common data shapes

**Financial bridge** (revenue/profit walk): start = opening figure, positive =
gains/new revenue, negative = costs/churn/losses, end = closing figure. Notes are
usually the signed deltas and the totals.

**Metric decomposition**: start = baseline metric, steps = each driver's
contribution, end = final metric. Add reference lines for targets or SLAs.

**Cumulative progression across periods**: interleave `end` checkpoint bars
between groups of steps to show intermediate totals (e.g. an end-of-quarter bar
partway through).

## Notes on interpreting user data

- If the user gives running totals but not step deltas, compute the deltas
  (each step = this total − previous total) and set the step `value` to the
  delta; put the total or delta in the `note` as they prefer.
- `note` is free text and is not required to equal the computed running total —
  but if a note is meant to show a total, make sure it matches, or the chart will
  look inconsistent.
- If the user wants a plain static image rather than an editable tool, still
  produce this file — they can export PNG/SVG from it and ignore the controls.
- The template is fully self-contained: Chart.js is inlined and it uses system
  fonts, so the generated file works offline with no external network calls.
