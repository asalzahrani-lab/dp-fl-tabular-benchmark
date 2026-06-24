# Revisiting Differentially Private Federated Learning for Tabular Data: A Matched-Accounting Benchmark of Boosting versus DP-SGD

<!-- After your first Zenodo release, paste the DOI badge here (replace XXXXXXX). -->
<!-- [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX) -->

This repository contains the source code, experimental configurations, privacy-accounting parameters, and result logs accompanying the paper:

> **Revisiting Differentially Private Federated Learning for Tabular Data: A Matched-Accounting Benchmark of Boosting versus DP-SGD.**
> *(Manuscript under review — full citation will be added upon publication.)*

The study provides a like-for-like comparison of differentially private **gradient-boosted trees** against **DP-SGD** for federated learning on tabular data, under a **shared (matched) privacy-accounting protocol** so that both methods are evaluated at identical (ε, δ) budgets.

---

## Repository structure

```
dp-fl-tabular-benchmark/
├── README.md            # This file
├── CITATION.cff         # Machine-readable citation metadata
├── .zenodo.json         # Metadata for the Zenodo archival record
├── LICENSE              # MIT license
├── requirements.txt     # Python dependencies
├── src/                 # Source code (models, DP mechanisms, training loop)
├── configs/             # Experiment + privacy-accounting configuration files
├── data/                # Dataset acquisition instructions (raw data not committed)
├── results/             # Metric logs / CSVs that back the paper's tables
├── figures/             # Scripts that regenerate the paper's figures
└── scripts/             # Top-level run scripts (e.g., reproduce_all)
```

---

## Requirements & setup

Tested with Python 3.10+.

```bash
# (recommended) create an isolated environment
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate

# install dependencies
pip install -r requirements.txt
```

> **Reproducibility note:** the versions in `requirements.txt` are a starting point.
> Before your Zenodo release, replace it with your *exact* environment by running
> `pip freeze > requirements.txt`. Pinned versions are what make the benchmark
> independently reproducible.

---

## Datasets

The benchmark uses publicly available tabular datasets from the **UCI Machine Learning Repository**
(e.g., Adult / Census Income, Bank Marketing). These are **reused public datasets** and are **not
redistributed** in this repository. See [`data/README.md`](data/README.md) for download links and the
expected directory layout.

---

## Reproducing the benchmark

> The commands below are **templates** — edit them to match your actual entry points / file names.

```bash
# 1. Prepare datasets (download + preprocess into the expected format)
python -m src.data.prepare --config configs/datasets.yaml

# 2. Run the full matched-accounting sweep (boosting vs. DP-SGD across ε budgets)
bash scripts/reproduce_all.sh

# 3. Regenerate tables and figures from the logged results
python -m src.evaluate --results results/ --out results/tables/
python figures/make_figures.py --results results/ --out figures/
```

Each run writes metrics to `results/` keyed by `{dataset, method, epsilon, seed}` so that every
number in the paper can be traced back to a specific log.

---

## Matched privacy accounting

The central methodological contribution is **matched accounting**: both the DP boosting models and the
DP-SGD models are calibrated with the **same privacy accountant** (Rényi DP / Gaussian-mechanism
accounting) to the **same (ε, δ)** targets. The accounting parameters (noise multipliers, sampling
rates, clipping norms, composition settings) live in `configs/` and are loaded at run time so that the
reported privacy guarantees are auditable. See [`configs/README.md`](configs/README.md).

---

## Mapping results to the paper

| Paper artifact | Produced by | Output location |
|---|---|---|
| Table 1 (datasets) | static | `data/README.md` |
| Main accuracy–privacy results | `src/evaluate.py` | `results/tables/` |
| Figures (utility vs. ε) | `figures/make_figures.py` | `figures/` |

*(Update this table to match your final table/figure numbering.)*

---

## How to cite

If you use this code, please cite the paper and the archived software record (Zenodo DOI, added after
the first release). A machine-readable citation is provided in [`CITATION.cff`](CITATION.cff).

```bibtex
@software{dp_fl_tabular_benchmark,
  author  = {Alzahrani, Ahmed},
  title   = {Revisiting Differentially Private Federated Learning for Tabular Data:
             A Matched-Accounting Benchmark of Boosting versus DP-SGD},
  year    = {2026},
  version = {1.0.0},
  doi     = {10.5281/zenodo.XXXXXXX},
  url     = {https://doi.org/10.5281/zenodo.XXXXXXX}
}
```

---

## License

Released under the **MIT License** — see [`LICENSE`](LICENSE).

## Contact

Ahmed Alzahrani — Faculty of Computing and Information Technology (FCIT),
King Abdulaziz University, Jeddah, Saudi Arabia.
ORCID: [0000-0003-3261-4413](https://orcid.org/0000-0003-3261-4413)
