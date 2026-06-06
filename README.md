# TGI Studio

**A browser-based analysis platform for preclinical oncology efficacy studies.**

TGI Studio turns raw caliper measurements from PDX (patient-derived xenograft) and other tumor-growth studies into publication-ready analyses — tumor growth curves, growth inhibition, body-weight tracking, response classification, and survival analysis — entirely in your browser. No installation, no server, and your data never leaves your machine.

> *Load your data and buckle up — this ride's about to get fun.* 🐭

**© 2026 Alexey Sorokin**

---

## Table of Contents

1. [Highlights](#highlights)
2. [Getting Started](#getting-started)
3. [Importing Data](#importing-data)
4. [Analysis Views](#analysis-views)
5. [Survival Analysis](#survival-analysis-kaplan-meier)
6. [Editing & Cleaning Data](#editing--cleaning-data)
7. [Statistical Methods](#statistical-methods)
8. [Exporting Results](#exporting-results)
9. [Key Formulas](#key-formulas)
10. [Privacy & Support](#privacy--browser-support)

---

## Highlights

- **12 linked analysis views** — from raw tumor volume to Kaplan-Meier survival
- **Runs fully offline** — a single HTML file; your data never leaves the browser
- **Inline data editor** — fix typos, exclude animals, mark deaths, with full undo/revert
- **Statistics built in** — Welch's t-test, RECIST-style response classification, Grubbs/ESD outlier detection, log-rank survival test
- **Export everything** — high-resolution plots (SVG/PNG), formatted Excel, CSV

---

## Getting Started

1. Download `TGI_Studio.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, or Safari).
3. Drag in your data file — or click **Demo Data** to explore immediately.

That's it. There is nothing to install and nothing to configure.

---

## Importing Data

TGI Studio auto-detects three layouts:

| Format | Structure |
|--------|-----------|
| **V4 (wide)** | A `MODEL` header row with measurements in repeating per-timepoint blocks. |
| **V3 (grouped)** | Group / animal rows with day columns. |
| **Flat** | One row per measurement (group, animal, day, length, width, weight). |

Tumor volume is computed as **V = (L × W²) / 2** unless a pre-calculated volume column is present, in which case that value is used. Length, width, and body weight are all optional per row — views simply skip animals with missing values.

You can drag-and-drop the file anywhere on the import screen, or use the file picker.

---

## Analysis Views

| View | What it shows |
|------|---------------|
| **Tumor Volume** | Mean tumor volume per group over time, with SD/SEM error bars. |
| **TVC** | Tumor Volume Change % vs. baseline — group means ± error. |
| **TVC Clones** | Individual-animal TVC% trajectories (spaghetti plot). |
| **EOS TVC** | End-of-study TVC% bar chart with optional pairwise Welch t-tests and significance brackets. |
| **Waterfall** | Per-animal best response, ranked — the classic oncology waterfall. |
| **EOS TGI** | End-of-study Tumor Growth Inhibition % vs. control, color-coded by response zone. |
| **BWC** | Body Weight Change % over time — your tolerability readout. |
| **BWC Clones** | Individual-animal body-weight trajectories. |
| **Response Heatmap** | Animal × timepoint grid of TVC%, for spotting responders at a glance. |
| **Data Table** | The full underlying dataset, filterable. |
| **Summary** | Per-group endpoint statistics and RECIST-style classification. |
| **Survival** | Kaplan-Meier progression-free survival with log-rank test (see below). |

All views respect the active group selection, excluded animals, and active timepoints — change a filter once and every view updates. Plot lines connect measured points with straight segments (no curve smoothing) so trends aren't visually distorted.

---

## Survival Analysis (Kaplan-Meier)

Because true survival is rarely the endpoint in xenograft work, TGI Studio uses **tumor progression-free survival**: an "event" occurs when an animal's tumor crosses a threshold you define. No extra data entry is needed — it reuses your existing measurements.

**Choose your event metric:**

- **TVC% reaches** *(default, 100% = tumor doubling)* — progression defined by relative growth from baseline.
- **Tumor volume reaches** *(default 1000 mm³)* — progression defined by absolute size.

**What you get:**

- **Kaplan-Meier step curves** per group, showing the percentage still progression-free over time.
- **Censoring marks (✛)** for animals that died early or never reached the threshold.
- **Median survival** per group (NR = not reached).
- **Log-rank (Mantel-Cox) test** across all groups, with χ², degrees of freedom, p-value, and significance stars.

The event time is **linearly interpolated** between the two measurements that bracket the threshold crossing, so curves aren't artificially snapped to measurement days. Animals marked dead before crossing are censored at the time of death; animals that never cross are censored at their last observation.

> **A note on small samples:** With typical PDX group sizes (n ≈ 8–10), the log-rank test is naturally underpowered. A visually clear separation can still yield p just above 0.05 — that's a property of the statistics, not the tool.

---

## Editing & Cleaning Data

Open the **inline data editor** (double-click any point on the clone plots, or use the editor toolbar) to correct measurements without leaving the app:

- Click any cell to edit length, width, or weight. The editor is **context-aware** — on TVC Clones it edits length/width; on BWC Clones it edits weight; non-relevant columns are locked.
- The **Day** column is locked to protect timepoint integrity.
- A built-in check flags any row where **width exceeds length** (a likely transposition) with a red highlight.
- Mark animals as **dead** so they're handled correctly in stats and survival.
- **Exclude** individual animals or whole groups from analysis.
- **Outlier detection** — flag suspect measurements with Grubbs' test, the generalized ESD test, or a simple z-score, then review before applying.
- **Undo / Revert** — step back one change at a time (**Revert Last**), or restore everything to the original imported values (**Revert All**); each edited row also has its own revert control.

Edited cells are highlighted and edits flow through to every view in real time.

---

## Statistical Methods

| Method | Used for |
|--------|----------|
| **Welch's t-test** | Pairwise group comparisons at end of study (unequal variances). |
| **Log-rank (Mantel-Cox)** | Comparing survival curves across groups. |
| **Grubbs' / Generalized ESD / z-score** | Outlier detection. |
| **RECIST-style classification** | Categorizing per-animal responses (regression / stable / progression). |

p-value approximations (t-distribution and chi-square tails) are computed via standard numerical methods — the regularized incomplete beta and gamma functions — and have been validated against textbook critical values.

---

## Exporting Results

- **Plots** — export any chart as SVG or PNG at a configurable scale for crisp figures.
- **Excel** — formatted `.xlsx` preserving group structure and styling.
- **CSV** — plain data export for downstream tools.

A bulk export bundles all plots at once.

---

## Key Formulas

- **Tumor volume:** V = (L × W²) / 2
- **TVC% (Tumor Volume Change):** (Vₜ − V₀) / V₀ × 100
- **TGI% (Tumor Growth Inhibition):** (1 − ΔV_treated / ΔV_control) × 100
- **BWC% (Body Weight Change):** (BWₜ − BW₀) / BW₀ × 100

---

## Privacy & Browser Support

TGI Studio runs entirely client-side. Your data is read into the browser tab and never transmitted anywhere — closing the tab clears it.

Works in current versions of Chrome, Edge, Firefox, and Safari. Best experienced on a desktop or laptop given the data-dense visualizations.

---

## Disclaimer

TGI Studio is a research aid for exploratory and presentation purposes. It is not validated software for regulatory submission. Always confirm critical statistics in a validated statistical package before reporting.

---

TGI Studio © 2026 Alexey Sorokin.
