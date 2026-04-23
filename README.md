# A Topological Framework for Identifying Structural Breaks in Cross-Asset Cryptocurrency Networks

**Junda Wu<sup>1,2</sup>, Jian Yao<sup>1,3,\*</sup>, Haoyuan Feng<sup>4,5</sup>**

<sup>1</sup> School of Economics and Management, University of Chinese Academy of Sciences, Beijing 100190, China  
<sup>2</sup> Digital Economy Laboratory, Beijing Institute of Mathematical Sciences and Applications, Beijing 101408, China  
<sup>3</sup> Beijing Key Laboratory of Topological Statistics and Applications for Complex Systems, Beijing Institute of Mathematical Sciences and Applications, Beijing 101408, China  
<sup>4</sup> PBC School of Finance, Tsinghua University, Beijing 100084, China  
<sup>5</sup> Research Institute of The People's Bank of China, Beijing 100033, China  

\* Corresponding author: yaojian23@mails.ucas.ac.cn

**Journal:** *Computational Economics* | **Manuscript ID:** CSEM-D-25-03144

---

## Abstract

The cryptocurrency market is characterized by pronounced volatility and structural fragility, yet traditional risk indicators primarily capture price-level fluctuations and often fail to detect deeper structural breaks. This study proposes a Topological Data Analysis (TDA) framework for detecting structural state transitions by tracking the topological evolution of cross-asset similarity networks. We compute Wasserstein and DTW distances under a sliding-window scheme with time-delay embedding, construct Vietoris–Rips complexes on the resulting similarity networks, and extract topological features including Betti-0, Betti-1, and persistent entropy. These indicators are aligned with 37 realized market events (2020–2025) using data from the ten largest cryptocurrencies by market capitalization, and benchmarked via ROC–AUC analysis and three structural break detection algorithms (PELT, BinSeg, CUSUM). Betti-0 consistently outperforms BVOL, BTC Rogers volatility, VIX, Fear & Greed, S&P 500, LSTM, Transformer, DCC-GARCH, and Graphical LASSO in both event identification and structural break detection. The findings indicate that topology-based measures provide an important complementary perspective for structural break detection in cryptocurrency markets, with practical implications for portfolio risk management and systemic stability assessment.

**Keywords:** Topological Data Analysis · Structural Break Detection · Cryptocurrency Market · Persistent Homology

---

## Motivation

Traditional approaches to structural break detection face three interrelated limitations in cryptocurrency markets:

- **Volatility-based models** (ARCH/GARCH) rely on parametric assumptions and capture only second-moment dynamics, struggling with heavy tails and 24/7 microstructure frictions.
- **Classical change-point methods** (CUSUM, Bai–Perron) operate on univariate or aggregated statistics, missing rich cross-sectional information in high-dimensional networks.
- **Correlation-based approaches** are insensitive to nonlinear interdependencies and leverage cascades, and yield unstable covariance estimates under asynchronous trading.

These limitations call for a framework that is distribution-free, robust to high dimensionality, and designed to capture evolving structural connectivity — precisely what TDA offers.

---

## Methodology

### Pipeline Overview

```
Raw price & volatility series (top-10 cryptos, Aug 2020 – Sep 2025)
        │
        ▼
Step 1: Sliding-window delay embedding  [m=21, τ=1]
        │
        ▼
Step 2: Pairwise distance matrices
        ├── Wasserstein distance (scipy, p=1)       [Log-return & Volatility]
        └── DTW distance (fastdtw, no warping band) [Log-return & Volatility]
        │
        ▼
Step 3: Vietoris–Rips filtration → Persistent homology  [ripser, maxdim=1]
        ├── Betti-0  (connected components → market fragmentation)
        ├── Betti-1  (1-cycles → higher-order dependence loops)
        └── Persistent Entropy
        │
        ▼
Step 4: Benchmarking  (ROC-AUC + DeLong tests + Bootstrap CI)
        ├── vs. BVOL, BTC Rogers Vol, VIX, Fear & Greed, S&P 500
        └── vs. LSTM, Transformer, DCC-GARCH, Graphical LASSO
        │
        ▼
Step 5–8: Extended analyses
        ├── Category-stratified AUC (8 event types)
        ├── Case studies (4 representative events)
        ├── Granger causality (lags 1–21 days)
        └── Betti-0 timing strategy (backtesting 2021–2025)
```

### Key Implementation Parameters

| Parameter | Value | Rationale |
|-----------|-------|-----------|
| Sliding window *m* | 21 (baseline) | ≈ one trading month |
| Time lag *τ* | 1 | Rapid ACF decay in crypto returns |
| Distance metrics | Wasserstein (*p*=1) + DTW | Outperform Euclidean by >0.13 AUC |
| VR max dimension | 1 (H₀ and H₁) | Cross-asset network has *n*=10 nodes |
| Filtration grid | 50 equally spaced *ε* values | From 0 to global max pairwise distance |
| Max DTW distance | 5.321886 | Global max across all rolling windows |
| Max Wasserstein distance | 0.427174 | Global max across all rolling windows |
| TDA library | `ripser` (Ripser.py) | ~18.5 ms per date, *n*=10 |
| Change-point library | `ruptures` | PELT (RBF, penalty=log *n*), BinSeg & KernelCPD (*n*_bkps=10) |

### Economic Interpretation of Betti-0

Each node represents a cryptocurrency; edges arise when assets are sufficiently similar in the cross-asset distance network. A connected component is a group of assets moving within a shared dependence structure. Under normal conditions, strong cross-asset dependence keeps most assets in one dominant component. As systemic stress accumulates, liquidity is withdrawn unevenly and assets begin to decouple, causing the network to fragment into weakly connected substructures — reflected as rising Betti-0. This fragmentation signal precedes observable price dislocations, explaining why Betti-0 is the strongest early-warning indicator in our framework.

---

## Key Results

### ROC-AUC Comparison (±21-day event window, 37 events)

| Method | AUC | 95% CI | F1 Score |
|--------|-----|--------|----------|
| **Log-DTW Betti-0** | **0.650** | [0.623, 0.674] | **0.662** |
| Log-Wasserstein Betti-0 | 0.646 | [0.621, 0.671] | — |
| Vol-DTW Betti-0 | 0.642 | [0.619, 0.665] | — |
| Vol-Wasserstein Betti-0 | 0.632 | [0.610, 0.655] | — |
| BVOL | 0.625 | [0.599, 0.650] | 0.497 |
| BTC Rogers Volatility | 0.618 | — | — |
| VIX | 0.564 | — | — |
| Fear & Greed Index | 0.541 | — | — |
| S&P 500 | 0.498 | — | — |
| LSTM | 0.620 | — | 0.302 |
| Transformer | 0.619 | — | 0.263 |
| DCC-GARCH (best) | 0.545 | — | 0.053 |
| Graphical LASSO (best) | 0.507 | — | 0.000 |

Log-DTW Betti-0 significantly outperforms BVOL (DeLong test: ΔAUC = 0.025, *p* = 0.026).

### Early-Warning Performance Across Evaluation Windows

| Window | Log-DTW | Log-Wasserstein | Vol-DTW | Vol-Wasserstein |
|--------|---------|-----------------|---------|-----------------|
| [−21, 0] | 0.640 | 0.629 | 0.629 | 0.619 |
| [−14, 0] | 0.629 | 0.617 | 0.615 | 0.606 |
| [−7, 0] | 0.616 | 0.612 | 0.607 | 0.591 |
| [0, +7] | 0.640 | 0.630 | 0.644 | 0.623 |
| [0, +21] | 0.665 | 0.652 | 0.658 | 0.642 |

Betti-0 retains meaningful discriminatory power in strictly pre-event windows, confirming genuine early-warning content.

### Category-Stratified Analysis (8 Event Types)

TDA advantage is heterogeneous across event categories:

- **Market Milestone** (n=6): Log-DTW AUC = **0.974** — TDA outperforms all baselines
- **Systemic Failure** (n=7): Log-DTW AUC = 0.602 vs VIX AUC = 0.802 — conventional indicators dominate
- **Regulation** (n=10): All methods weak; S&P 500 marginally better than TDA
- **Institutional Adoption / Corporate Action**: Baselines generally stronger

The topological signal is strongest for market-cycle and connectivity-driven events; volatility- and sentiment-based indicators retain a comparative advantage for crisis-type shocks.

### Granger Causality

- Log-DTW Betti-0 **unidirectionally Granger-causes VIX** (*p*_min = 0.017, lags 1–21 days), confirming genuine forward-looking content.
- Bidirectional feedback with BVOL and BTC Rogers volatility (*p* < 0.001 in both directions), consistent with a feedback loop between topological fragmentation and realized volatility.

### Betti-0 Timing Strategy (Backtesting 2021–2025)

| Strategy | Total Return | Ann. Volatility | Sharpe Ratio | Max Drawdown |
|----------|-------------|-----------------|--------------|--------------|
| Buy & Hold | 179.07% | 54.60% | 0.7039 | −67.01% |
| **Log-DTW Betti-0** | **199.81%** | **50.16%** | **0.7524** | −67.18% |

Higher returns and lower volatility achieved simultaneously; zero false alarms during the 2023–2024 bull market at the 90th-percentile threshold.

### Robustness

- **Parameter sensitivity:** Betti-0 dominance is preserved across all 12 (m, τ) combinations tested (*m* ∈ {7,14,21,28}, *τ* ∈ {1,2,3}); AUC variation < 0.058 across all settings.
- **Distance metric:** Euclidean distance yields AUC = 0.516 for Betti-0 (barely above random), confirming the necessity of Wasserstein and DTW.
- **15-asset extension:** Main findings qualitatively unchanged when the asset universe is expanded from top-10 to top-15 cryptocurrencies (93.98% of total market cap).

---

## Repository Structure

```
TDA-Crypto-StructuralBreaks/
├── README.md
├── replication/                        # Full replication package for main paper results
│   ├── README.md                       # Detailed replication instructions
│   ├── main_v2.tex                     # LaTeX manuscript source
│   ├── references.bib
│   ├── data/
│   │   ├── raw/                        # Source data from CoinMarketCap, BitMEX, Yahoo Finance
│   │   │   ├── Close_price_10_raw.xlsx
│   │   │   ├── Volatility_10_raw.xlsx
│   │   │   ├── Parkinson_Rogers_Volatility.xlsx
│   │   │   ├── SP500_VIX_benchmarks_raw.xlsx
│   │   │   ├── Fear_Greed_Index_raw.xlsx
│   │   │   ├── fear_and_greed_index.csv
│   │   │   └── Event_Catalog.xlsx      # 37 catalogued events, 8 categories
│   │   └── processed/                  # Main analysis dataset and result tables
│   │       ├── TDA_SP500_VIX_BTC_BVOL_CVI_merged0329_labeled_with_network.csv
│   │       ├── benchmark_auc_with_bootstrap_ci.csv
│   │       ├── benchmark_auc_delong_tests.csv
│   │       ├── TDA_change_point_f1_results_21.csv
│   │       ├── tbl_auc_by_event_category.csv
│   │       ├── tbl_granger_causality_full.csv
│   │       ├── tbl_false_alarm_analysis.csv
│   │       └── similarity_matrices_10coin/   # Pairwise distance matrices (DTW & Wasserstein)
│   ├── code/
│   │   ├── step1_log_return_embeddings.ipynb
│   │   ├── step2_similarity_matrices.ipynb
│   │   ├── step3_tda_from_similarity.ipynb
│   │   ├── step4_benchmark_analysis.ipynb
│   │   ├── step5_event_category_analysis.ipynb
│   │   ├── step6_case_studies.ipynb
│   │   ├── step7_granger_causality.ipynb
│   │   └── step8_topological_signatures.ipynb
│   └── figures/
│       ├── main_paper/                 # Figures 1–10
│       └── appendix/                   # Figures A.1–A.3
└── supplementary/                      # Reviewer-response additional analyses
    ├── README.md
    ├── data/                           # Top-15 asset data for robustness extension
    ├── code/                           # 15-asset pipeline, Euclidean comparison, network metrics
    └── figures/                        # Additional figures for revision responses
```

---

## Quickstart

### Requirements

```bash
pip install ripser fastdtw scipy ruptures scikit-learn pandas numpy matplotlib
```

| Package | Version | Purpose |
|---------|---------|---------|
| `ripser` | ≥ 0.6.0 | Persistent homology (Ripser.py) |
| `fastdtw` | ≥ 0.3.4 | DTW distance |
| `scipy` | ≥ 1.10 | Wasserstein distance (`p=1`) |
| `ruptures` | ≥ 1.1.7 | PELT, BinSeg, KernelCPD |
| `scikit-learn` | ≥ 1.2 | ML benchmarks |

### Reproducing Main Results

To reproduce all paper results from the processed dataset (no need to re-run steps 1–3):

```python
# Load the main analysis dataset directly
import pandas as pd
df = pd.read_csv("replication/data/processed/TDA_SP500_VIX_BTC_BVOL_CVI_merged0329_labeled_with_network.csv")
```

Then run notebooks `step4` through `step8` in sequence.

To reproduce from raw data (full pipeline):

```
step1 → step2 → step3 → step4 → step5 → step6 → step7 → step8
```

See [`replication/README.md`](replication/README.md) for detailed instructions.

---

## Data

| Dataset | Period | Assets | Source |
|---------|--------|--------|--------|
| Cryptocurrency prices & volatility | Aug 2020 – Sep 2025 | Top-10 by market cap | CoinMarketCap |
| Bitcoin Volatility Index (BVOL) | Aug 2020 – Sep 2025 | BTC | BitMEX |
| VIX, S&P 500 | Aug 2020 – Sep 2025 | — | Yahoo Finance |
| Crypto Fear & Greed Index | Aug 2020 – Sep 2025 | — | alternative.me |
| Event catalog | Aug 2020 – Sep 2025 | 37 events, 8 categories | Authors' compilation |

The top-10 assets (BTC, ETH, XRP, USDT, BNB, SOL, USDC, DOGE, TRX, ADA) collectively account for **93.11%** of total cryptocurrency market capitalization as of September 2025.

---

## Citation

If you use this code or data, please cite:

```bibtex
@article{wu2026topological,
  title   = {A Topological Framework for Identifying Structural Breaks in
             Cross-Asset Cryptocurrency Networks},
  author  = {Wu, Junda and Yao, Jian and Feng, Haoyuan},
  journal = {Computational Economics},
  year    = {2026},
  note    = {Manuscript ID: CSEM-D-25-03144}
}
```

---

## License

This repository is released for academic use. Please contact the corresponding author for any commercial applications.
