# Waterfall Chart Builder

A single-file, browser-based tool for building custom waterfall charts with a dark, gold-accented theme. Configure segments, reference lines, colours, and labels in a live editor, then export the result as a PNG or SVG.

![Waterfall chart builder](preview.png)

## Features

- **Live editor** — every change updates the chart instantly.
- **Segment types** — start, positive step, negative step, and end/total bars, with running totals calculated automatically. Leave an end bar's value blank to auto-fill the running total.
- **Reference lines** — add horizontal target lines at any value, each with its own colour and label. Labels sit at the left edge and nudge apart automatically so they never overlap.
- **Non-overlapping axis labels** — x-axis labels stagger across up to three rows when bars are close together, with dashed connectors back to the axis.
- **Full theming** — set start/positive/negative/end bar colours, background, and text colour. Title, subtitle, and an eyebrow label are all editable.
- **Export** — download a crisp PNG or a scalable SVG at any resolution.

## Usage

Open `index.html` in any modern browser. No build step, no server, no install.

Everything lives in one HTML file, so you can also just double-click it locally.

### Building a chart

1. **Chart** — set the title, subtitle, and eyebrow label.
2. **Colours** — pick colours for each bar type, the background, and text.
3. **Reference lines** — add target lines with a label, value, and colour.
4. **Segments** — add rows in order. Each row has a label, a value, a type, and an optional bar label (the text shown above the bar, e.g. `+1.72` or `25%`).
5. **Export** — set the width and height, then click **PNG** or **SVG**.

## Hosting

This repo is set up for **GitHub Pages**:

1. The chart file is named `index.html`, so it loads at the site root.
2. In **Settings → Pages**, set the source to **Deploy from a branch** → **main** / **/ (root)**.
3. The site publishes to `https://<username>.github.io/<repo>/`.

Any push to `main` redeploys automatically.

## Notes

Chart.js and the display fonts load from CDNs, so the hosted page needs an internet connection to render. To make it fully offline-capable, inline those dependencies into `index.html`.

## Built with

- [Chart.js](https://www.chartjs.org/) for the bar rendering
- Inter + DM Mono for typography
- Plain HTML, CSS, and JavaScript — no framework, no build tooling

## License

MIT — do whatever you like with it.
