[portfolio-hub-README.md](https://github.com/user-attachments/files/32883329/portfolio-hub-README.md)
# 👨‍💻 Chauncey's Portfolio

Welcome to my portfolio. Here you'll find my projects, code repositories, and technical notes
spanning data engineering, analytics, and energy markets.

**Live site:** [chaunceyjones.github.io](https://chaunceyjones.github.io)

## 📌 Table of Contents

- [Data Engineering](#%EF%B8%8F-data-engineering)
- [SQL](#%EF%B8%8F-sql)
- [Python](#-python)
- [Visualization](#-visualization)
- [Chauncey's Guides](#-chaunceys-guides)

---

## 🏗️ Data Engineering

End-to-end pipelines against real public data sources — including the parts that aren't clean.
Both projects below source data that required handling genuine messiness: a discontinued API,
prices buried in narrative prose, merged spreadsheet headers, and Excel formula errors sitting
in the source file itself.

### [Permian–Waha Basis Tracker](https://github.com/ChaunceyJones/waha-basis-tracker)
Weekly pipeline tracking the price spread between Permian Basin (Waha) and Henry Hub natural
gas. Pulls from the FRED and EIA APIs, parses Waha prices out of unstructured EIA report text
with a regex parser, joins three sources on mismatched reporting dates, and flags statistical
dislocations. Orchestrated as a scheduled Airflow DAG.
`Python` · `Airflow` · `Pandas` · `SQL/DuckDB` · `scipy` · `BeautifulSoup`

### [Solar PPA Pricing & Discounting](https://github.com/ChaunceyJones/solar-ppa-pricing)
Deal-pricing analysis across 1,775 utility-scale solar projects from LBNL's research database.
Reads a 59-tab, ~57 MB Excel workbook, cleans a benchmark table with merged headers and literal
`#N/A` cells, and segments realized prices against a third-party market index. Single-command
pipeline runner with an automated validation step.
`Python` · `Pandas` · `openpyxl` · `SQL/DuckDB` · `Jupyter`

---

## 🗄️ SQL

Analytical SQL written against real project data, not toy schemas. In both cases the SQL exists
alongside an independent pandas implementation, and a runner script diffs the two — two
implementations agreeing is a stronger correctness signal than one looking plausible.

### [Rolling anomaly detection](https://github.com/ChaunceyJones/waha-basis-tracker/blob/main/sql/basis_anomalies.sql)
Window functions (`AVG() OVER`, `STDDEV_SAMP() OVER`) computing a trailing 12-observation
z-score to flag price dislocations, with a minimum-periods guard so early rows don't get a
score computed from too little history. Matches the pandas implementation to within rounding.

### [Vintage-matched price segmentation](https://github.com/ChaunceyJones/solar-ppa-pricing/blob/main/sql/pricing_segments.sql)
CTEs, `UNION ALL` across two segment dimensions, and `CASE` logic that distinguishes "too few
projects to be reliable" from "no benchmark exists for this region" — two genuinely different
reasons for a missing result that are easy to silently collapse into one blank cell.

---

## 🐍 Python

Data extraction, cleaning, statistical analysis, and pipeline code. Highlights:

- **Web scraping with real-world failure handling** — retries with backoff, browser headers,
  URL-pattern fallbacks, and honest reporting of which weeks couldn't be parsed and why
  ([`parse_waha_weekly.py`](https://github.com/ChaunceyJones/waha-basis-tracker/blob/main/src/parse_waha_weekly.py))
- **Hypothesis testing with multiple-comparisons correction** — correlation tests against
  candidate leading indicators, where the honest conclusion was that a nominally significant
  result doesn't survive a Bonferroni correction
- **Excel parsing beyond `read_excel()` defaults** — merged headers located by inspecting raw
  rows, and Excel formula-error strings explicitly converted rather than left as un-computable
  text ([`load_benchmark_index.py`](https://github.com/ChaunceyJones/solar-ppa-pricing/blob/main/src/load_benchmark_index.py))
- **Reproducible pipelines** — each project has a single `run_all.py` entry point that runs
  every step in order and stops at the first failure

---

## 📊 Visualization

Charts are generated in-pipeline with Matplotlib and embedded in each project's README. Each
project also has a 12-slide presentation deck walking through context, method, findings,
limitations, and recommendation.

*Tableau dashboards: in progress — I'll add them here as they're built.*

---

## 📚 Chauncey's Guides

*In progress.* Planned write-ups, drawn from problems I actually hit building the projects above:

- Parsing data out of government reports when no clean API exists
- Why your SVG renders in a browser but breaks on GitHub (strict XML vs. HTML entities)
- Vintage-matching price data before benchmarking it — and the $194/MWh mistake that happens
  when you don't

---

## 📫 Contact

- **Portfolio:** [chaunceyjones.github.io](https://chaunceyjones.github.io)
- **GitHub:** [@ChaunceyJones](https://github.com/ChaunceyJones)
