# Project 3 — Statistical Storytelling

**Question:** Do public holidays have fewer bike rentals per day than non-holidays, by enough to justify a reduced bike-supply schedule on holidays?

## Data
- **Seoul Bike Sharing Demand**, UCI Machine Learning Repository (dataset id 560)
- Source: https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand
- DOI: https://doi.org/10.24432/C5F62R
- **Licence:** Creative Commons Attribution 4.0 (CC BY 4.0)
- 8,760 rows, one row = one hour; outcome = rented bike count; grouping = `Holiday`.

## Files
- `Projekt3_NaveenKumar.ipynb` — full analysis (audit, naive gap, confounding, artefact, sensitivity, stretch)
- `REPORT.md` — eight-section write-up

## How to run
```bash
pip install pandas numpy scipy matplotlib jupyter
jupyter notebook project3_analysis.ipynb
```
The notebook downloads the data directly from UCI's static zip endpoint:

```python
url = "https://archive.ics.uci.edu/static/public/560/seoul+bike+sharing+demand.zip"
```

It fetches the zip with `urllib.request`, opens it with `zipfile`, and reads the CSV inside with `pandas.read_csv(..., encoding="latin1")` (the file is not UTF-8). No local file or API key is needed; the notebook downloads the data itself when it runs. If UCI is unreachable, download the zip manually from the source link above, extract `SeoulBikeData.csv` next to the notebook, and read it the same way.

Random seed is fixed (`SEED = 42`), so bootstrap results are reproducible.
