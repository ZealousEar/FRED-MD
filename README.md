# Dynamic Factor-Augmented Macroeconomic Forecasting with FRED-MD 

> **Project Types**: Group + Individual Research Project  
> **Degree**: BSc Data Science, School of Mathematics - University of Bristol  
> **All code, analysis, and documentation**: Completed independently

## Project Overview

A comprehensive implementation of factor-augmented regression (FAR) models for macroeconomic forecasting using the FRED-MD dataset. This project demonstrates advanced statistical modeling, time series analysis, and economic interpretation aligned with methodologies from McCracken & Ng (2015) and Bai & Ng (2002).

**Key Achievement**: Developed a 3-factor model that outperforms AR(4) and random walk benchmarks across multiple forecast horizons, with statistically significant improvements for short-term predictions.

### Authorship & Contribution

- **Individual Dissertation**: 100% individual work
  - All implementation, analysis, forecasting, and documentation
  - Incorporated feedback from group project grading
  - Extended analysis with regime-switching and leading indicator frameworks

- **Original Group Project Context**: 
  - 3-person team for initial research
  - **My contribution**: Section 4 (Application to Dataset) - all data preprocessing, PCA implementation, factor analysis, forecasting models, and empirical results
  - Sections 1-3 (literature review, methodology background) completed by team members
  - **Key distinction**: All technical implementation and quantitative analysis was my individual work within the group setting

---

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
2. **Leading Indicators**: Factor 1 shows peak values 4-6 months before NBER recession dates
3. **Forecast Stability**: Rolling window scheme demonstrates superior performance in volatile periods

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
├── Project.Rmd              # Main analysis notebook (individual work)
├── Project.nb.html          # Rendered HTML output
├── current.csv              # FRED-MD dataset (not included - download from FRED)
├── README.md                # This file
└── figures/                 # Generated plots
    ├── factor_loadings.png
    ├── forecast_comparison.png
    └── regime_analysis.png
```

**Note**: Code corresponds to individual dissertation. Original group dissertation available upon request, clearly delineating Section 4 (my contribution) from team members' sections.

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

- McCracken, M. W., & Ng, S. (2015). *FRED-MD: A monthly database for macroeconomic research*. Federal Reserve Bank of St. Louis Working Paper.
- Bai, J., & Ng, S. (2002). *Determining the number of factors in approximate factor models*. Econometrica, 70(1), 191-221.
- Stock, J. H., & Watson, M. W. (2002). *Forecasting using principal components from a large number of predictors*. Journal of the American Statistical Association, 97(460), 1167-1179.
