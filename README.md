# TGI Studio

A self-contained, browser-based tool for analyzing and visualizing preclinical oncology tumor measurement data. Built for PDX and xenograft efficacy studies, TGI Studio turns raw tumor measurements into publication-ready growth curves, waterfall plots, TGI bar charts, survival curves, and more — entirely in the browser, with no installation, server, or data upload required.

The companion **Import Studio** module (built in) cleans messy, unstructured lab Excel files and feeds them straight into the analysis workflow.

---

## Highlights

- **Runs anywhere** — a single HTML file. Open it in any modern browser; nothing is uploaded and no internet connection is needed once loaded.
- **Built-in data cleaning** — Import Studio handles raw, irregular Excel files and converts them to the analysis format without leaving the app.
- **Twelve analysis views** — tumor volume, TVC%, waterfall, TGI, body weight, survival, response heatmaps, data tables, and a study summary.
- **Full styling control** — adjust plot size, fonts, axis thickness, line and symbol sizing, gridlines, and error-bar mode, then save it all with one click.
- **One-click export** — download every plot (dark and light themes) plus the edited data as a single ZIP, or export just the Excel file.

---

## Getting started

1. Download `TGI_Studio.html`.
2. Open it in Chrome, Edge, Firefox, or Safari (double-click, or drag into a browser tab).
3. Load data using one of three options:
   - **Demo Data** — explore the app immediately with synthetic data.
   - **Import Clean Data** — upload a structured `.xlsx`/`.csv` or paste tab-separated cells from Excel.
   - **Clean Raw Data** — open Import Studio to clean a messy file first, then send it directly into TGI Studio.

That's it. All processing happens locally in your browser.

---

## Data format

The analysis expects measurements in repeating six-column blocks:

| Group | ID | Day | Length | Width | Weight |
|-------|----|----|--------|-------|--------|

- **Group** — treatment arm name (e.g. "Vehicle", "Drug A 10 mpk").
- **ID** — animal identifier within the group.
- **Day** — study day for the measurement.
- **Length / Width** — caliper measurements in mm. Tumor volume is computed as `V = L × W² ÷ 2`.
- **Weight** — body weight in grams (optional, used for the BWC views).

A pre-computed **Volume** column can be supplied instead of Length/Width. Column order is flexible on paste/import, and tab, comma, and semicolon delimiters are auto-detected. If your raw file does not match this layout, use Import Studio to restructure it.

---

## Import Studio (built-in)

Import Studio is an embedded module for preparing unstructured lab files. Open it from the **Clean Raw Data** button. It runs in an isolated frame, so its cleanup workflow never interferes with the analysis app.

Workflow:

1. **Upload** your raw Excel file.
2. **Review structure** — assign treatment groups (with click-and-drag), auto-detect treatment arms, resolve duplicate animal IDs, and confirm the day schedule.
3. **Export** — click **Send to TGI Studio** to pipe the cleaned data straight into the analysis views, or download a clean V3 Excel/CSV to import later.

---

## Analysis views

| View | What it shows |
|------|---------------|
| **Tumor Volume** | Group mean tumor volume over time, with optional SD/SEM error bars. |
| **TVC** | Tumor Volume Change (%) relative to baseline, per group. |
| **TVC Clones** | Individual animal TVC% trajectories within a selected group. |
| **EOS TVC** | End-of-study TVC% bar chart, with optional p-value annotation. |
| **Waterfall** | Per-animal change from baseline, ranked. |
| **EOS TGI** | End-of-study Tumor Growth Inhibition (%) per group versus control. |
| **BWC** | Body Weight Change (%) over time — a tolerability readout. |
| **BWC Clones** | Individual body-weight trajectories. |
| **Response Heatmap** | Per-animal, per-timepoint TVC% colored from shrinking to growing. |
| **Data Table** | The full measurement table, filterable by group and day. |
| **Summary** | Key statistics per group at the end of study. |
| **Survival** | Kaplan–Meier curves for progression-free / event-free endpoints. |

### Core formulas

- **Tumor volume:** `V = L × W² ÷ 2`
- **TVC% (per animal):** `(TVₜ − TV₀) ÷ TV₀ × 100`, where TV₀ is the baseline-day volume
- **TGI% (per group):** `(1 − mean TVC%_treatment ÷ mean TVC%_control) × 100`, computed from group means

---

## Controls

**Analysis toolbar (above the plots)**

- **Control group** — pick the reference arm for TGI calculations.
- **Baseline day** — set the day used as TV₀ for TVC/TGI.
- **Error bars** — toggle between None, ±SD, and ±SEM across all relevant plots.
- **Hide gridlines** — clean up plots for export.

**Sidebar styling**

- **Plot Size** — set an exact width and height in pixels for consistent figures across projects (leave at 0 for auto-fit).
- **Chart Font** — choose font family and set tick, axis-title, and legend sizes.
- **Line & Symbol** — set axis spine thickness, data line thickness, error-bar thickness, and data point (symbol) size.
- **Default** — apply the standard styling preset to every plot in one click.

**Treatment groups**

- Show/hide groups, recolor and restyle each group (fill opacity, line style, line width), rename a group inline by clicking its name, and run outlier detection (Grubbs, ESD/Rosner, or Z-score) per group.

---

## Exporting

- **Export All Data** — a single ZIP containing every plot as scalable SVG (dark and light themes), full-view JPGs, per-group TVC clone plots, and the edited Excel data file.
- **Export Excel Only** — the modified data as an Excel workbook with a Summary sheet. When the source was a real imported file, edited cells are highlighted in red bold for traceability.

---

## Privacy

TGI Studio is fully client-side. Your data never leaves your computer — there is no backend, no analytics, and no network transmission of measurements. The file can be used completely offline after the initial load.

---

## Browser support

Works in current versions of Chrome, Edge, Firefox, and Safari. Charting uses Chart.js; Excel handling uses SheetJS and ExcelJS; ZIP packaging uses JSZip. These libraries load from a CDN on first open, so an internet connection is required the first time you open the file (after that it can run offline).

---

## License

© 2026 Alexey Sorokin. All rights reserved.
