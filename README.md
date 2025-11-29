# A Topological Framework for Detecting Structural Breaks in Cross-Asset Cryptocurrency Networks

## Overview

The cryptocurrency market exhibits pronounced volatility and structural fragility.  
Traditional risk indicators — such as price volatility, market indices, and sentiment measures — primarily capture price-level fluctuations and often fail to reveal deeper structural breaks.

This project introduces a **Topological Data Analysis (TDA)** framework designed to detect **structural state transitions** by tracking the evolving topology of **cross-asset similarity networks** and identifying latent sources of systemic instability.

---

## Methodology

- Compute **Wasserstein distance** and **Dynamic Time Warping (DTW)** within a sliding-window scheme.  
- Construct **Vietoris–Rips complexes** on cross-asset similarity networks.  
- Extract topological features:
  - **Betti-0**
  - **Betti-1**
  - **Persistent Entropy**
- Align topological indicators with realized market events (2020–2025) using data from the ten largest cryptocurrencies by market capitalization.
- Benchmark performance using:
  - **ROC–AUC analysis**
  - Structural break detection algorithms: **PELT**, **Binary Segmentation**, and **CUSUM**

---

## Key Findings

- **TDA-based indicators consistently outperform traditional market measures.**  
  Topological connectivity features (**Betti-0**) achieve **AUC values of 0.632–0.650**, exceeding the performance of:
  - Fear & Greed Index  
  - BTC volatility  
  - S&P 500  
  - VIX Index  

- Across all three structural break detection procedures, **TDA indicators achieve the highest F1-scores**, outperforming both traditional market indicators and BTC volatility in identifying structural breaks.

---

## Conclusion

Topology-based measures provide a powerful and complementary perspective for structural break detection in cryptocurrency markets.  
They capture system-level connectivity shifts that conventional price-based indicators often overlook, offering meaningful applications in:

- **Portfolio risk management**
- **Crisis monitoring**
- **Systemic stability assessment**

---

## Example Figure

![Figure 1](https://raw.githubusercontent.com/Janeyaoo/A-Topological-Framework-for-Identifying-Structural-Breaks-in-Cross-Asset-Cryptocurrency-N/main/Fig1.png)

---

## Code Availability

All analysis code is openly available in this repository.
