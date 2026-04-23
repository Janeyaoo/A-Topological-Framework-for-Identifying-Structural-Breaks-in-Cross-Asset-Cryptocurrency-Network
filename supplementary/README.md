# Supplementary Materials: Reviewer-Response Analyses

**Manuscript ID:** CSEM-D-25-03144  
**Journal:** Computational Economics

---

## Overview

This folder contains the additional analyses conducted in response to the seven reviewers' comments during the revision process. These materials complement the main replication package and document all newly added robustness checks, extended benchmarks, and additional figures reported in the revised manuscript.

---

## Repository Structure

```
supplementary/
├── README.md
├── data/
│   ├── Close_price_15.csv              # Daily closing prices, top-15 cryptos (robustness extension)
│   ├── Parkinson_volatility_15.csv     # Parkinson volatility, 15 assets
│   ├── Rogers_volatility_15.csv        # Rogers volatility, 15 assets
│   ├── SP500_VIX_BVOL_CVI.csv          # Benchmark series (same as replication)
│   ├── auc_tda_results_15.csv          # AUC results for 15-asset TDA analysis
│   ├── benchmark_auc_results_15.csv    # AUC results for 15-asset benchmarks
│   ├── benchmark_changepoint_f1_results_15.csv  # F1 scores for 15-asset change-point analysis
│   └── empirical_before_after_summary_15.csv    # Event study summary, 15 assets
├── code/
│   # 15-asset robustness extension (addresses Reviewer 1 Comments 6–9)
│   ├── 1.new_data_process_15coins_log_return_embeddings.ipynb
│   ├── 2.new_data_process_15coins_similarity_matrices.ipynb
│   ├── 3.new_data_process_15coins_tda_from_similarity.ipynb
│   ├── 4.new_benchmark_multiwindow_15coin.ipynb
│   # Euclidean distance comparison (addresses Reviewer 1 Comment 3)
│   ├── 5.new_euclidean_similarity_tda_auc_15coin.ipynb
│   # Network metrics vs TDA (addresses Reviewer 1 Comment 11)
│   ├── 8.network_metrics_vs_tda.ipynb
│   ├── 9.plot_tda_vs_network_baseline.ipynb
│   # 15-asset full pipeline (addresses Reviewer 1 Comments 6–9)
│   ├── 1.data_process_15test.ipynb
│   ├── 2.empirical_analysis_15test.ipynb
│   └── 3.benchmark_analysis_15test.ipynb
└── figures/
    ├── fig_distance_method_choice.pdf/png      # Euclidean vs. Wasserstein vs. DTW comparison
    ├── fig_tda_vs_network_baseline.pdf/png     # TDA vs. network centrality baseline
    ├── fig_reviewer5_evaluation_window_comparison.pdf/png  # Evaluation window sensitivity
    ├── fig_m21_tau3_dtw_wasserstein_vs_benchmarks.pdf/png  # m=21,τ=3 robustness
    ├── tbl_distance_method_choice_summary.csv
    ├── tbl_tda_vs_network_by_method.csv
    └── tbl_m21_tau3_dtw_vs_benchmarks.csv
```

---

## Mapping to Reviewer Comments

| Analysis | Reviewer(s) | Comment(s) | Notebook(s) |
|----------|-------------|------------|-------------|
| 15-asset robustness extension | R1 | C6 (survivorship bias), C7 (liquidity), C9 (asset class) | `1–4` (15coins), `1–3` (15test) |
| Euclidean distance comparison | R1 | C3 (distance metric selection) | `5.new_euclidean_similarity_tda_auc_15coin.ipynb` |
| Network metrics vs. TDA | R1 | C11 (network-based indicators) | `8.network_metrics_vs_tda.ipynb`, `9.plot_tda_vs_network_baseline.ipynb` |
| Evaluation window sensitivity | R1, R3 | C5 (evaluation window), Q4 | `fig_reviewer5_evaluation_window_comparison` |
| m=21,τ=3 robustness | R1 | C1–C2 (window/lag sensitivity) | `fig_m21_tau3_dtw_wasserstein_vs_benchmarks` |

---

## Additional Analyses in Main Replication Notebooks

The following analyses were added to the main pipeline notebooks (see `replication/code/`) in response to reviewer comments:

| Analysis | Location | Reviewer | Comment |
|----------|----------|----------|---------|
| Parameter sensitivity (12 (m,τ) settings) | `step4_benchmark_analysis.ipynb` (Appendix A.1) | R1, R3, R6, R7 | C1–C2, Q1, Q2, Q9 |
| BVOL benchmark comparison | `step4_benchmark_analysis.ipynb` (Section 5.1) | R1, R3 | C10, Q8 |
| LSTM/Transformer/DCC-GARCH/GLASSO benchmarks | `step4_benchmark_analysis.ipynb` (Section 5.2) | R1, R3, R7 | C12, Q8, Q5 |
| DeLong tests + bootstrap CIs | `step4_benchmark_analysis.ipynb` (Section 5.1) | R3 | Q6 |
| ROC curves for top performers | `step4_benchmark_analysis.ipynb` (Section 5.1) | R3 | Q6 |
| Early-warning window analysis | `step4_benchmark_analysis.ipynb` (Section 4.5) | R1, R3, R7 | C5, Q4–Q5, Q9 |
| Trading/backtesting strategy | `step4_benchmark_analysis.ipynb` (Section 5.3) | R1 | C17 |
| False alarm / threshold analysis | `step4_benchmark_analysis.ipynb` (Section 5.3) | R1 | C18 |
| Runtime benchmarks (N=10 to 50) | `step4_benchmark_analysis.ipynb` (Section 3) | R1 | C19 |
| Category-stratified AUC heatmap | `step5_event_category_analysis.ipynb` (Section 4.4) | R1 | C14 |
| Case studies (4 events) | `step6_case_studies.ipynb` (Section 4.4) | R1 | C15 |
| Granger causality (7 lags) | `step7_granger_causality.ipynb` (Section 6.3) | R1 | C16 |

---

## Notes

- All supplementary analyses use the same software environment as the main replication package.
- The 15-asset extension covers the same 2020–2025 sample period and adds Stellar (XLM), Bitcoin Cash (BCH), Litecoin (LTC), Tezos (XTZ), and IOTA to the baseline 10-asset panel.
- The Euclidean baseline was added to demonstrate the superior discriminative ability of Wasserstein and DTW distances (Log-DTW AUC=0.650 vs. Log-Euclidean AUC=0.516).
