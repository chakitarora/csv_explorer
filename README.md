# CSV Explorer

A fast, fully client-side CSV analysis tool that runs entirely in your browser. No uploads, no servers, no accounts, no data ever leaves your device.

Drop in any CSV or TSV file and instantly get summary statistics, column profiles, interactive charts, a correlation matrix, missing value analysis, duplicate detection, and a filterable data table — all in a single HTML file with no dependencies.

---

## Live demo

👉 `https://chakitarora.github.io/csv-explorer`

---

## Features at a glance

| Tab | What it does |
|---|---|
| **Overview** | Dataset-level stats, auto-generated data quality warnings, copy-as shortcuts |
| **Columns** | Per-column statistical profiles with type detection and visualisation |
| **Data** | Searchable, sortable, filterable table with outlier highlighting and export |
| **Visualise** | Five chart types rendered as SVG, exportable |
| **Correlations** | Pearson correlation heatmap across all numeric columns |
| **Missing** | Ranked bar chart of null/empty values by column |
| **Duplicates** | Exact duplicate row finder and key-column duplicate detector |

---

## Detailed feature breakdown

### Overview tab

- **Summary statistics** — total rows, column count, numeric column count, overall missing value percentage, exact duplicate row count, estimated in-memory size
- **Automatic data quality warnings** — flags columns with >30% missing values, notifies of duplicate rows, lists columns with detected outliers
- **Column type breakdown** — visual summary of how many columns are numeric, text, boolean, or date
- **Copy as** shortcuts:
  - Python list of column names
  - R character vector of column names
  - Markdown table of the first 10 rows
  - JSON schema inferred from column types

### Columns tab

Each column gets a dedicated profile card showing:

- **Type badge** — `numeric`, `text`, `boolean`, or `date`, auto-detected from values
- **Numeric columns**: min, max, mean, median, standard deviation, outlier count (IQR method), unique value count, histogram sparkline
- **Text / boolean columns**: unique value count, present row count, top-6 value frequency bar chart
- **Missing value bar** — proportional bar showing what fraction of rows are null/empty for that column

Null detection covers blank strings, `NA`, `null`, `NULL`, `NaN`, `N/A`, `#N/A`, `none`, `nil` (case-insensitive).

### Data tab

- **Live search** — filters all rows in real time across every column
- **Filter builder** — add multiple simultaneous conditions:
  - `contains` — substring match
  - `equals` / `not equals` — exact match
  - `>` / `<` — numeric comparison
  - `is null` / `not null` — presence check
  - Conditions are combined with AND logic
  - Filters can be added, removed, and combined freely
- **Click-to-sort** — click any column header to sort ascending; click again to sort descending
- **Outlier highlighting** — cells in numeric columns that fall outside 1.5×IQR are highlighted in red
- **Null highlighting** — missing/empty cells are visually dimmed and marked with a centre dot
- **Adjustable page size** — 50, 100, or 250 rows per page
- **Export filtered data** — download currently visible (filtered + sorted) rows as CSV or JSON

### Visualise tab

Five chart types, all rendered as crisp SVG:

| Chart | Best for |
|---|---|
| **Histogram** | Distribution of a numeric column, 20 bins |
| **Bar chart** | Top value frequencies for a text/categorical column |
| **Scatter plot** | Relationship between two numeric columns (auto-sampled to 2000 points for performance) |
| **Box plot** | Spread and outliers of a numeric column; outlier dots plotted individually |
| **Line chart** | Numeric column values as a sequence (first 500 rows) |

All charts export as `.svg` — ready for publication or presentations.

### Correlations tab

- Computes **Pearson correlation coefficients** for all pairs of numeric columns
- Displayed as a colour-coded heatmap:
  - **Blue** = positive correlation (darker = stronger)
  - **Red** = negative correlation (darker = stronger)
  - **Neutral** = diagonal (self-correlation = 1)
- Hover over any cell to see the exact r value to 3 decimal places
- Computed on the currently filtered rows

### Missing values tab

- Lists every column that has at least one missing value
- Sorted by missing count descending
- Colour-coded bar chart:
  - **Red** — most missing (relative to worst column)
  - **Yellow** — moderate
  - **Blue** — low
- Shows both percentage and absolute row count

### Duplicates tab

Two detection modes:

**Exact duplicate rows** — finds rows where every column value is identical. Shows which row numbers are duplicates and groups them.

**Duplicate keys** — select any column as a key field (e.g. an ID column), find all values that appear more than once, and see exactly which row numbers they appear in.

---

## Supported file formats

| Format | Extension | Notes |
|---|---|---|
| Comma-separated | `.csv` | Standard format; quoted fields supported |
| Tab-separated | `.tsv` | Auto-detected if tabs are present |
| Plain text | `.txt` | Treated as CSV or TSV depending on delimiter |

**Quoted fields** are handled correctly — commas inside `"double quotes"` are not treated as delimiters.

**Encoding** — files are read as UTF-8. Most exports from Excel, Google Sheets, R, Python (pandas), and SPSS are compatible.

---

## Performance

All processing is single-threaded in the browser. Practical limits depend on your machine, but typical performance:

| File size | Rows (approx.) | Load time |
|---|---|---|
| < 1 MB | < 20,000 | Instant |
| 1–10 MB | 20,000–200,000 | 1–3 seconds |
| 10–50 MB | 200,000–1,000,000 | 5–15 seconds |

The scatter plot auto-samples to 2,000 points and the line chart caps at 500 rows to keep rendering fast regardless of file size.

---

## Privacy

Everything runs locally using the browser's built-in JavaScript engine and Canvas/SVG APIs. No data is transmitted to any server at any point. The only external request the page makes is loading the Google Fonts stylesheet for typography — which can be removed by deleting the `<link>` tag if full offline use is needed.

---

## How to use

1. Open the tool in any modern browser
2. Drag and drop a CSV/TSV file onto the landing area, or click to browse
3. Navigate between tabs to explore your data
4. Use **Load new file** in the top-right to switch datasets without refreshing

---

## Deploy your own instance (GitHub Pages)

You only need a free GitHub account.

**Step 1 — Create a repository**

Go to [github.com](https://github.com) → **+** → **New repository** → name it `csv-explorer` → set to **Public** → **Create repository**

**Step 2 — Upload the file**

On the repo page → **Add file** → **Upload files** → upload `csv_explorer.html` and rename it to `index.html` (or upload it as `index.html` directly) → also upload `README.md` → **Commit changes**

**Step 3 — Enable GitHub Pages**

**Settings** → **Pages** → under Branch select `main` and folder `/` (root) → **Save**

**Step 4 — Done**

Your instance will be live at `https://yourusername.github.io/csv-explorer` within 1–2 minutes.

---

## Browser compatibility

| Browser | Support |
|---|---|
| Chrome / Edge 90+ | ✅ Full |
| Firefox 90+ | ✅ Full |
| Safari 15+ | ✅ Full |
| Mobile Chrome (Android) | ✅ Full |
| Mobile Safari (iOS) | ✅ Full |

---

## Local use (no internet required)

After removing the Google Fonts `<link>` tag, the file works fully offline:

```bash
# macOS / Linux
open csv_explorer.html

# Windows
start csv_explorer.html

# Or serve locally
python3 -m http.server 8080
# then open http://localhost:8080/csv_explorer.html
```

---

## Tech stack

| Component | Technology |
|---|---|
| Parsing | Vanilla JavaScript (custom quoted-CSV parser) |
| Statistics | Vanilla JavaScript (mean, median, std, IQR, Pearson r) |
| Charts | SVG generated inline — no chart library |
| Styling | Pure CSS with CSS variables |
| Fonts | IBM Plex Mono + IBM Plex Sans via Google Fonts |
| Build | None — single HTML file, zero dependencies |

---

## License

MIT — free to use, modify, and distribute.
