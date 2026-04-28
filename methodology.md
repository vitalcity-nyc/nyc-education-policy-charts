# Methodology

Two infographics restyled in a clean, embeddable card format. Every number, source, and assumption is documented below — no black boxes.

## Chart 1 — Teacher collective bargaining, 1960–1977

**File:** `bargaining-heatmap.html`
**Data:** `data/bargaining.json`

### Source
Saltzman, Gregory M. (1981). *The Growth of Teacher Bargaining and the Enactment of Teacher Bargaining Laws.* PhD dissertation, University of Wisconsin–Madison. The dissertation is the canonical primary source and is held in ProQuest Dissertations & Theses; it is not freely available online.

### What's plotted
For each of 16 states, the percentage of public-school teachers covered by a collective bargaining agreement, sampled in the years **1960, 1962, 1964, 1966, 1968, 1970, 1972, 1975, 1976, and 1977**. The cell with a heavy black border in each row marks the survey year at or after the state's bargaining law took effect (the "law year" shown in parentheses next to the state name).

### Transcription
Because the original dissertation could not be obtained for this build, values were transcribed cell-by-cell from a published reproduction of the table (the source image provided by the editor). Transcription is human and may contain small reading errors of ±1–2 percentage points in faint cells. Readers who need exact figures should consult the dissertation directly via ProQuest or the University of Wisconsin Library.

### Color ramp
8-step sequential ramp tied to value bins (0–5, 5–15, 15–30, 30–50, 50–70, 70–85, 85–95, 95–100), running from a pale chartreuse-cream at the low end to deep red at the high end. The ramp is built from Vital City brand tokens (`--vc-ramp-0` … `--vc-ramp-7`). Empty / 0% cells render with no numeric label to keep the grid quiet.

### Law-year highlight
For each state we identify the survey-year column closest to and not before the state's bargaining-law year. That cell gets a 2px black inset border. This matches the source figure's "highlighted column indicates year collective bargaining law was enacted" convention.

## Chart 2 — NAEP grade-4 reading vs per-pupil spending, 2024

**File:** `naep-spending-scatter.html`
**Data:** `data/naep-spending.json`

### Sources
- **NAEP scores:** 2024 NAEP Reading Assessment, Grade 4, average scale score, public schools, by jurisdiction. Pulled from the NAEP Data Service (`nationsreportcard.gov/Dataservice/GetAdhocData.aspx`), parameters `subject=reading, grade=4, subscale=RRPCM, variable=TOTAL, stattype=MN:MN, Year=2024`. The full state list (50 states + DC) is included.
- **Per-pupil spending:** Per Pupil Amounts for Current Spending of Public Elementary-Secondary School Systems by State, **Fiscal Year 2023** (Table 8 of the U.S. Census Bureau's 2023 Annual Survey of School System Finances summary tables, file `elsec23_sumtables.xlsx`, released May 2025). "Total" current spending column is used.

### What's plotted
Each dot is one of 51 jurisdictions (50 states + DC). X axis is per-pupil current spending in FY2023 dollars. Y axis is the 2024 NAEP grade-4 reading average scale score. Each point carries the USPS state abbreviation as a direct label.

### Statistics
- **N:** 51
- **Pearson r:** 0.082 (computed)
- **R²:** 0.007 (computed)
- **Linear fit:** y = 8.00e-5·x + 212.77

These are very close to the values shown on the reference image (r = 0.085, R² = 0.007). Small differences are within rounding and may also reflect that the reference used FY2024 spending where available; this build uses FY2023 throughout because FY2024 state data has not yet been published in the Census summary tables as of April 2026.

### Calculation
Pearson r and the OLS slope/intercept are computed in Python at build time over the 51-row dataset. Both are stored alongside the per-row data in `data/naep-spending.json` so the chart renders the same line every time.

### Coloring
All points and the trend line are rendered in Vital City Charcoal (`#707175`) per the editor's choice of a neutral framing — no focal state highlighted. (The Vital City convention to put the focal subject in Magenta is intentionally not applied here.)

## Limitations
- The bargaining figures are transcribed from a printed reproduction; consult Saltzman (1981) directly for primary values.
- Grade-4 NAEP reading is one outcome of many; per-pupil current spending is one input of many. The near-zero correlation says only that, *bivariately*, state-mean spending and state-mean reading scores have essentially no linear relationship in the cross-section. It is not a causal claim about whether spending matters for any individual student or district.
- DoDEA (Department of Defense Education Activity) and U.S. territories shown on some NAEP releases are excluded because per-pupil spending in the Census table covers states + DC only.

## Reproducing this build
1. Per-pupil spending: download `elsec23_sumtables.xlsx` from `https://www2.census.gov/programs-surveys/school-finances/tables/2023/secondary-education-finance/`. Read sheet `8`, column "Total" (column index 2).
2. NAEP reading: GET the URL above; parse JSON; take `value` for each two-letter `jurisdiction` code.
3. Join on state, compute Pearson r and OLS line, write `data/naep-spending.json`.
