# QQ-EVCS+: A Data-Grounded Hybrid Framework for Electric-Vehicle Adoption Forecasting, Quantum-Enhanced Optimization, and Dynamic Charging Management

A single, self-contained Google Colab notebook that reproduces **every data figure and table** in the *Q-EVCS+ v2* study — from the raw Washington State EV registration dataset through forecasting, a variational quantum classifier, metaheuristic optimisation, dynamic pricing, and charger allocation.

Everything runs from one file (`Q_EVCS_full_colab.ipynb`). You only need the dataset CSV; there are no separate `.py` scripts to manage.

**Author:** Sunawar Khan

---

## What this notebook does

The pipeline is organised into five analytical modules plus figure/table generation. Each module was rewritten from an earlier version (v1) to fix specific methodological and reporting issues, and each writes its metrics to a `results_*.json` file so tables and figures can never drift from the code that produced them.

| Module | Topic | Produces |
|--------|-------|----------|
| **1 — Core** | Dataset audit, city-year panel, EV-growth forecasting benchmark, ablations, city clustering, range statistics | Tables 2, 4, 5, 6 |
| **2 — Quantum** | Variational quantum classifier (VQC) under grouped cross-validation, barren-plateau diagnostics | Figure 10, Tables 7 & 8 |
| **3 — Optimisation** | QPSO vs PSO vs GA vs budget-matched random search (multi-seed, wall-clock reported) | Table 9 |
| **4 — Pricing** | Dynamic pricing via REINFORCE with a learned baseline, plus an elasticity sensitivity sweep | Figures 13 & 13b, Table 10 |
| **5 — Allocation** | Charger allocation framed as a queue-tolerance sensitivity analysis on an M/G/c station | Figures 14 & 15, Table 11 |
| **Figures** | Regenerates the remaining data figures (6, 7, 8, 9, 11, 12) from the consolidated metrics | Figures 6–12 |
| **Tables** | Renders all paper tables (2, 4, 5, 6, 7, 8, 9, 10, 11) as DataFrames | All tables |

Because every figure is generated from the same `results_*.json` files the modules emit, the reproducibility claim holds for the entire manuscript, not just part of it.

---

## Requirements

- **Google Colab** (recommended — the notebook uses Colab's upload/download helpers), or a local Jupyter environment.
- **Dataset:** `Electric_Vehicle_Population_Data.csv` (~43 MB), the Washington State EV population data.
- **Python packages:** installed automatically by the first cell:

  ```bash
  pip install -q xgboost pennylane
  ```

  The notebook also uses `numpy`, `pandas`, `scipy`, `scikit-learn`, and `matplotlib`, which are pre-installed on Colab.

---

## How to run

1. Open `Q_EVCS_full_colab.ipynb` in Google Colab.
2. Select **Runtime → Run all**.
3. When prompted (Cell 2), **upload `Electric_Vehicle_Population_Data.csv`**.
   - Alternatively, uncomment the Google Drive block in that cell to load the CSV from your Drive.
4. Let the notebook run. Figures render inline; tables print as DataFrames near the end.
5. (Optional) The final cell downloads the `results_*.json` metric files.

> ⚠️ **Module 3 (the optimiser) is slow** — roughly 10–15 minutes. If you just want a quick pass, you can skip that cell; the remaining tables and figures will still be produced (Table 9 will be the only one missing).

---

## Getting the dataset

The notebook expects the Washington State **Electric Vehicle Population Data** CSV. It is published on the Washington State open-data portal (data.wa.gov) as "Electric Vehicle Population Data." Download it, keep the filename as `Electric_Vehicle_Population_Data.csv`, and upload it when the notebook asks.

---

## Output files

Running the notebook produces these consolidated metric files, each backing a set of tables/figures:

- `results_core.json` — dataset audit, forecasting benchmark, clustering, range stats
- `results_quantum.json` — VQC results and barren-plateau diagnostics
- `results_opt.json` — optimiser comparison (QPSO/PSO/GA/random search)
- `results_rl_pricing.json` — pricing agent and elasticity sweep
- `results_allocation.json` — charger allocation sensitivity analysis
- `results_all_v2.json` — combined results

Generated figures are saved under a `figs/` directory.

---

## Notes on methodology

This v2 pipeline was written to be honest about what it measures. Among the corrections baked in:

- County counts are reported separately for in-state (Washington) vs. all county-of-record names in the file.
- The forecasting benchmark uses city-grouped 5-fold cross-validation, with the growth-label threshold computed **inside each training fold** to avoid leakage; model selection is backed by a paired t-test across folds rather than point estimates alone.
- The VQC is evaluated under the **same** grouped cross-validation as the classical baselines, for a like-for-like comparison.
- The optimiser comparison runs random search at a **matched evaluation budget**, across multiple seeds, with wall-clock time reported per method.
- The pricing agent is **REINFORCE** (not PPO); it is named accurately, and a diagnostic shows how much of the revenue lift is structural vs. learned.
- The allocation "policies" are named by their queue-tolerance parameter and framed as a sensitivity analysis rather than a comparison against trained RL baselines.

---

## Citation

If you use this work, please cite it as:

```bibtex
@misc{khan_qevcs_2026,
  author       = {Sunawar Khan, Habib Hamam},
  title        = {Q-EVCS+ v2: A Reproducible Pipeline for Quantum-Enhanced
                  EV Charging Station Analysis},
  year         = {2026},
  howpublished = {\url{https://github.com/SunawarKhan/Q-EVCS-A-Data-Grounded-Hybrid-Framework-for-Electric-Vehicle-Adoption-Forecasting}},
  note         = {Accessed: 2026}
}
```

Plain text:

> Sunawar Khan. *Q-EVCS+ v2: A Reproducible Pipeline for Quantum-Enhanced EV Charging Station Analysis.* 2026. GitHub repository.

---

## Author

**Sunawar Khan**

---

## License

Add a license of your choice (e.g. MIT) before publishing if you intend others to reuse the code.
