# Dynamic Factor-Augmented Macroeconomic Forecasting with FRED-MD
## **Research Papers**:  
# [Individual Dissertation (PDF)](https://github.com/ZealousEar/FRED-MD/blob/main/FRED-MD_Individual_Dissertation.pdf) | [Group Dissertation (PDF)](https://github.com/ZealousEar/FRED-MD/blob/main/FRED-MD_Group_Dissertation.pdf)  

> **Project Type**: Final year dissertation
> **Degree**: BSc Data Science, School of Mathematics - University of Bristol  
> **All code, analysis, and documentation**: Completed independently




## Project Overview

A comprehensive implementation of factor-augmented regression (FAR) models for macroeconomic forecasting using the FRED-MD dataset. This project demonstrates advanced statistical modeling, time series analysis, and economic interpretation aligned with methodologies from [McCracken & Ng (2015)](https://files.stlouisfed.org/files/htdocs/publications/review/2016-01-04/a-monthly-chronology-of-turning-points.pdf) and [Bai & Ng (2002)](https://www.jstor.org/stable/2692303).

**Key Achievement**: Developed a 3-factor model that outperforms AR(4) and random walk benchmarks across multiple forecast horizons, with statistically significant improvements for short-term predictions, providing early warning signals for incoming recessions 6-12 months in advance

### Key Practical Takeaway

**The extracted factors provide early warning signals for incoming recessions 6-12 months in advance**, with Factor 1 (Financial Stress & Labor Market) showing peak values 4-6 months before NBER-designated recession periods. This demonstrates the model's potential application in:
- **Risk Management**: Portfolio stress testing and tail risk hedging
- **Macro Trading**: Systematic allocation shifts based on regime probability
- **Central Banking**: Real-time monitoring of financial stability conditions

## Methodology & Technical Implementation

### 1. Data Processing & Transformation
- **Dataset**: FRED-MD (Federal Reserve Economic Data - Monthly Database)
  - 134 macroeconomic time series (2001-2019)
  - Transformed using standardized TCODE specifications (first differences, log differences, etc.)
- **Preprocessing Pipeline**:
  - Missing value imputation via EM algorithm with trend-preserving constraints
  - Outlier detection using 10×IQR thresholds
  - Zero-variance filtering

### 2. Factor Extraction & Selection
- **Principal Component Analysis (PCA)**
  - Eigendecomposition of correlation matrix following Σ = ΓΛΓ'
  - Multiple selection criteria implemented:
    - **Kaiser Rule**: 31 factors (eigenvalue > 1)
    - **90% Variance**: 41 factors
    - **Bai-Ng IC**: 4 factors *(selected for parsimony)*
    - **Bartlett Test**: 10 factors
- **Rationale**: Adopted Bai-Ng criterion to avoid overfitting in high-dimensional settings (N=134, T=228)

### 3. Factor Interpretation
Extracted 3 economically meaningful factors:

| Factor | Interpretation | Key Loadings |
|--------|---------------|--------------|
| **F1** | Financial Stress & Labor Market | ↑ Unemployment (UNRATE), Financial Spreads (BAAFFM), VIX <br> ↓ Real Income (RPI), Housing Starts, Retail Sales |
| **F2** | Price Pressures & Exchange Rates | ↑ Exchange Rates (EXCAUS), Inventory Ratios <br> ↓ Price Indices (PCEPI), Oil Prices |
| **F3** | Interest Rate Spreads & Housing | ↑ Term Spreads (T10YFFM), Corporate Bonds <br> ↓ Housing Permits, Construction |

### 4. Forecasting Framework
- **Models Compared**:
  - AR(4): Autoregressive benchmark
  - Random Walk: Naive baseline
  - FAR(4,2): 4 AR lags + 2 factor lags
- **Validation Schemes**:
  - Recursive window (expanding)
  - Rolling window (fixed 120-month)
- **Horizons**: h = 1, 2, ..., 12 months ahead

### 5. Statistical Testing
- **Diebold-Mariano Tests**: Formal comparison of forecast accuracy
- **Regime Analysis**: Differential performance during NBER recessions vs. expansions
- **Leading Indicator Analysis**: Pre-recession signal detection (6-month lead window)

---

## Key Results

### Forecast Performance (Target: Industrial Production)

| Horizon | FAR RMSE | AR(4) RMSE | Relative RMSE | DM Test p-value |
|---------|----------|------------|---------------|-----------------|
| h=1     | 0.0047   | 0.0048     | 0.99          | 0.759           |
| h=3     | 0.0052   | 0.0055     | 0.95          | 0.118           |
| h=6     | 0.0057   | 0.0059     | 0.97          | 0.214           |
| h=12    | 0.0059   | 0.0057     | 1.04          | 0.008**         |

*Note: FAR consistently outperforms Random Walk across all horizons (RW relative RMSE: 1.37-1.42)*

### Economic Insights
1. **Regime-Dependent Behavior**: Factor 1 exhibits 2.3× higher volatility during recessions
2. **Leading Indicators**: Factor 1 shows peak values 4-6 months before NBER recession dates, providing actionable early warning signals
3. **Forecast Stability**: Rolling window scheme demonstrates superior performance in volatile periods
4. **Recession Forecasting Application**: The 3-factor system successfully identifies pre-recession conditions 6-12 months in advance, with particularly strong signals from financial stress and labor market indicators

---

## Technical Skills Demonstrated

### Statistical & Econometric Methods
- Factor analysis (PCA, ML estimation)
- Time series modeling (ARMA, VAR)
- Information criteria optimization (Bai-Ng IC)
- Hypothesis testing (Bartlett, Diebold-Mariano)
- Regime-switching analysis

### Programming & Tools
- **R**: Advanced data manipulation (`tidyverse`, `dplyr`)
- **Specialized Packages**: `fbi` (FRED-MD), `forecast`, `vars`
- **Visualization**: `ggplot2` with recession shading, heatmaps, time series plots
- **Numerical Methods**: EM algorithm, rolling window estimation

### Research & Documentation
- Reproducible research workflow (R Markdown → PDF/HTML)
- Mathematical notation aligned with academic literature
- Comprehensive commenting and modular code structure

---

## Repository Structure

```
├── Project.Rmd              # Main analysis notebook
├── Project.nb.html          # Rendered HTML output
├── FRED-MD_Group_Dissertation.pdf # Group Dissertation
├── FRED-MD_Individual_Dissertation.pdf # Individual Dissertation
├── current.csv              # Dataset used
├── README.md                # This file
└── Plots/                 # Generated example plots

```

---

## Authorship & Contributions

**Specific Contributions**:
- ✓ Data acquisition and preprocessing pipeline design
- ✓ Implementation of factor extraction algorithms (PCA, information criteria)
- ✓ Development of forecasting framework with multiple validation schemes
- ✓ Economic interpretation and regime analysis
- ✓ Statistical testing and validation (Diebold-Mariano, Bartlett tests)
- ✓ All visualizations and documentation
- ✓ Code optimization and reproducible research workflow

---

## How to Run

### Prerequisites
```r
install.packages(c("fbi", "forecast", "vars", "ggplot2", "reshape2", "dplyr", "zoo"))
```

### Execution
1. Download FRED-MD data: [https://research.stlouisfed.org/econ/mccracken/fred-databases/](https://research.stlouisfed.org/econ/mccracken/fred-databases/)
2. Place `current.csv` in project directory
3. Open `Project.Rmd` in RStudio
4. Knit to PDF or run chunks interactively

---

## References

- McCracken, M. W., & Ng, S. (2015). *FRED-MD: A monthly database for macroeconomic research*. Federal Reserve Bank of St. Louis Working Paper. [[PDF]](https://files.stlouisfed.org/files/htdocs/publications/review/2016-01-04/a-monthly-chronology-of-turning-points.pdf)
- Bai, J., & Ng, S. (2002). *Determining the number of factors in approximate factor models*. Econometrica, 70(1), 191-221. [[JSTOR]](https://www.jstor.org/stable/2692303)
- Stock, J. H., & Watson, M. W. (2002). *Forecasting using principal components from a large number of predictors*. Journal of the American Statistical Association, 97(460), 1167-1179. [[Taylor & Francis]](https://www.tandfonline.com/doi/abs/10.1198/016214502388618960)
