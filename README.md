# A Topological Framework for Identifying Structural Breaks in Cross-Asset Cryptocurrency Networks

## Overview

The cryptocurrency market exhibits pronounced volatility and structural fragility.  
Traditional risk indicators — such as price volatility, market indices, and sentiment measures — primarily capture price-level fluctuations and often fail to reveal deeper structural breaks.

This project introduces a **Topological Data Analysis (TDA)** framework designed to detect **structural state transitions** by tracking the evolving topology of **cross-asset similarity networks** and identifying latent sources of systemic instability.

---

## Methodology

- Compute **Wasserstein distance** and **Dynamic Time Warping (DTW)** within a sliding-window scheme with time-delay embedding.
- Construct **Vietoris–Rips complexes** on cross-asset similarity networks.
- Extract topological features:
  - **Betti-0** (connected components — measures market fragmentation)
  - **Betti-1** (1-cycles — captures higher-order dependence loops)
  - **Persistent Entropy**
- Align topological indicators with 37 realized market events (2020–2025) using data from the ten largest cryptocurrencies by market capitalization.
- Benchmark performance using:
  - **ROC–AUC analysis** (with bootstrapped confidence intervals and DeLong tests)
  - Structural break detection algorithms: **PELT**, **Binary Segmentation**, and **CUSUM**

---

## Key Findings

- **TDA-based indicators consistently outperform traditional market measures.**  
  Topological connectivity features (**Betti-0**) achieve **AUC values of 0.632–0.650**, exceeding the performance of:
  - BVOL (Bitcoin Volatility Index) — AUC 0.625
  - BTC Rogers volatility — AUC 0.618
  - VIX Index — AUC 0.564
  - Fear & Greed Index — AUC 0.541
  - S&P 500 — AUC 0.498

- **TDA also outperforms modern machine learning and dynamic dependence models:**
  - LSTM — AUC 0.620, F1 0.302
  - Transformer — AUC 0.619, F1 0.263
  - DCC-GARCH — AUC 0.545, F1 0.053
  - Graphical LASSO — AUC 0.507, F1 0.000
  - **Best TDA (Log-DTW Betti-0) — AUC 0.650, F1 0.662**

- Across all three structural break detection procedures, **TDA indicators achieve the highest F1 scores**, outperforming conventional market and sentiment indicators, deep-learning methods, and model-based dynamic-dependence approaches.

- **Early-warning content is confirmed**: Betti-0 retains meaningful discriminatory power in strictly pre-event windows (AUC 0.616–0.640 in [−21, 0] to [−7, 0]).

- **Granger causality**: Log-DTW Betti-0 unidirectionally Granger-causes VIX (*p* = 0.017), consistent with it serving as a partial leading indicator of implied volatility.

---

## Conclusion

Topology-based measures provide a powerful and complementary perspective for structural break detection in cryptocurrency markets.  
They capture system-level connectivity shifts — particularly **market fragmentation** measured by Betti-0 — that conventional price-based indicators often overlook, offering meaningful applications in:

- **Portfolio risk management**
- **Crisis monitoring and early warning**
- **Systemic stability assessment**

---

## Example Figure

[View Figure 1 (PDF)](https://github.com/Janeyaoo/A-Topological-Framework-for-Identifying-Structural-Breaks-in-Cross-Asset-Cryptocurrency-Network/blob/main/replication/figures/main_paper/Fig1.pdf)

---

## Code and Data Availability

All analysis code and data are openly available in this repository.  
See [`replication/README.md`](replication/README.md) for the full replication pipeline and data description.
