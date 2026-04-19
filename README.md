# Economic Attractiveness of French Departments

Study on economic development disparities across French metropolitan departments, with a focus on identifying the structural determinants of entrepreneurial activity using OLS estimation.

**> Report is in French**
---

## Context

Business creation is a key indicator of territorial dynamism. Recent data from INSEE shows a slowdown in new firm registrations in France, raising questions about the local factors that drive or hinder entrepreneurship. This project builds on prior work by Levratto et al. (2013) on territorial dynamics and business creation in French departments.

The analysis covers **94 metropolitan departments** for the year 2022. Overseas territories and Corsica were excluded due to missing data and comparability concerns.

---

## Objective

The goal is to explain variations in the proportion of new business creations across French departments using a set of socioeconomic explanatory variables, and to provide a basis for policy recommendations aimed at reducing territorial inequalities.

**Dependent variable:**
- `pcENT`: proportion of new businesses created in 2022 relative to existing businesses in 2021 (%)

**Explanatory variables:**

| Variable   | Description                                      | Source      |
|------------|--------------------------------------------------|-------------|
| `nbENT`    | Number of existing firms in 2021 (thousands)     | INSEE       |
| `POP`      | Municipal population in 2021 (thousands)         | INSEE       |
| `DIPL`     | Share of graduates with Bac+3 or above (%)       | INSEE       |
| `REV`      | Median annual income in 2021 (thousands EUR)     | INSEE       |
| `gndENT`   | Number of large firms (250+ employees) in 2021   | data.gouv   |
| `txCHOM`   | Unemployment rate in 2021 (%)                    | INSEE       |
| `METRO`    | Binary indicator: presence of a metropolis       | collectivites-locales.gouv |

---

## Methodology

### 1. Descriptive Analysis
- Univariate statistics (mean, median, standard deviation, min, max) for each variable
- Distribution plots to identify skewness and outliers
- Bivariate analysis: simple linear regressions between `pcENT` and each explanatory variable
- Boxplot comparison by metropolis status

### 2. Correlation Analysis
- Pearson correlation matrix to detect multicollinearity
- Variance Inflation Factor (VIF) analysis
  - `nbENT`: VIF = 7.6 (high collinearity)
  - `gndENT`: VIF = 5.5 (moderate collinearity)

### 3. Model Selection
Three specifications were estimated and compared using the Akaike Information Criterion (AIC):

| Model | Specification | Adjusted R² |
|-------|--------------|-------------|
| Model 1 | Level-level | 0.590 |
| Model 2 | Level-log | 0.614 |
| Model 3 | Log-log | 0.686 |

The **log-log model** was retained. Variables `nbENT`, `POP`, and `gndENT` were log-transformed to linearize relationships and reduce the influence of extreme values.

### 4. Robustness Checks

| Test | Purpose | Result |
|------|---------|--------|
| White test | Heteroskedasticity | Detected — corrected using White and Newey-West standard errors |
| Shapiro-Wilk + QQ-plot | Normality of residuals | Rejected — presence of influential observations |
| Cook's distance | Influential points | Departments 91, 93, 95 identified |
| Chow test | Structural break (with/without metropolis) | No break detected (p = 0.97) |
| RESET test | Model misspecification | Correctly specified (p = 0.47) |

### 5. Final Model

The model reached:

- **Adjusted R² = 0.758**
- All variables significant at 5%, except `METRO`
- Homoskedasticity and normality of residuals confirmed

## Limitations

- Omitted variables: local cultural factors, regional public policies, and infrastructure are not included.
- Potential endogeneity between some explanatory variables and the dependent variable.
- Cross-sectional data only: no temporal dimension to capture trends over time.

---

## Data Sources

- [INSEE - Statistiques locales](https://statistiques-locales.insee.fr)
- [data.gouv - Annuaire des entreprises](https://annuaire-entreprises.data.gouv.fr)
- [Collectivites-locales.gouv](https://www.collectivites-locales.gouv.fr)

