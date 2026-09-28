# afl-player-performance-analytics

A data cleaning, exploratory analysis, and reporting pipeline built on real AFL (Australian Football League) player and match data — from raw, messy source files to a merged, validated dataset with documented data-quality findings.

## Overview

This project takes three raw AFL data sources (player biographical info, seasonal stats, and round-by-round stats) and turns them into a single analysis-ready dataset, then explores it for performance trends and insights. Every merge decision, data-quality issue, and assumption is documented rather than silently patched.

**Dataset scale:** ~25,000 player-season records / ~274,000 player-match records after merging.

## Project Structure

```
afl-player-performance-analytics/
├── 01_data_cleaning_merging.ipynb      # Raw CSV cleaning + composite-key merge
├── 02_exploratory_analysis.ipynb       # EDA and visualizations
├── 03_insights.ipynb                   # Team/player-level performance insights
├── 04_round_by_round_analysis.ipynb    # Season and round-level trend analysis
├── 05_match_context_integration.ipynb  # Home/away + match context merge
├── data/
│   ├── afl_players_info_raw.csv
│   ├── afl_players_seasonal_stats_raw.csv
│   ├── afl_players_round_by_round_stats_raw.csv
│   ├── team_matches_home_away_raw.csv
│   └── merged_players.csv              # Final cleaned/merged dataset
└── reports/
    └── data_quality_report.md          # Merge methodology, issues, and validation
```

## Key Findings from the Data Quality Report

- **Merge key:** no single column uniquely identifies a match, so records were joined on the composite key `Season + Round + Team + Opponent`.
- **Naming inconsistencies caused ~20% unmatched records** (55,000 of ~275,000) before normalization — e.g. "W. Bulldogs" vs. "Western Bulldogs", and inconsistent lowercasing in ~15% of the opponent column. Resolved via case-normalization and manual name mapping, bringing unmatched records to 0.
- **10 pre-existing duplicate rows** were identified (unrelated to the merge) and removed.
- **Missing data handled deliberately:** 398 records with genuinely missing crowd attendance were kept as `NaN` rather than imputed as 0, to avoid introducing false signal.
- **Merge validated for correctness:** player record count was unchanged immediately after the join (274,089 → 274,089), confirming no duplication was introduced by the merge itself.

## Tech Stack

- Python, Pandas, NumPy
- Matplotlib / Seaborn (visualization)
- Jupyter Notebook

## Getting Started

```bash
git clone https://github.com/<your-username>/afl-player-performance-analytics.git
cd afl-player-performance-analytics
pip install -r requirements.txt
jupyter notebook
```

Run the notebooks in order (`01` → `05`); each one reads the output of the previous step.

## License

MIT