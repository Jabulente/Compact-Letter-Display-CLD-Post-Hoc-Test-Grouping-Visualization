# Compact Letter Displays and Publication-Ready Tables in Python

This repository provides tools and examples for creating **Compact Letter Displays (CLDs)** in Python following **ANOVA** and **Tukey HSD** tests. It also includes utilities for generating **publication-ready tables** from datasets, summarizing statistics, visualizing distributions, and drawing statistical inference.

---

## Features

- Perform one-way ANOVA tests on grouped data
- Conduct **Tukey’s HSD** post-hoc analysis
- Automatically generate **Compact Letter Displays (CLDs)**
- Create **publication-ready summary tables** with means, standard errors, and group labels
- Provide descriptive and inferential statistics for reporting

---

## Technologies Used

- **Python 3.x**
- **pandas** – Data manipulation
- **numpy** – Numerical operations
- **statsmodels** – ANOVA and statistical modeling
- **scikit-posthocs** – Post-hoc tests and multiple comparisons
- **matplotlib** & **seaborn** – Data visualization
- **scipy** – Statistical functions

---

## Installation

Install the required Python libraries using pip:

```bash
pip install pandas numpy statsmodels scikit-posthocs seaborn matplotlib scipy
```

---

## Folder Structure

```
.
├── data/                     # Sample datasets
├── scripts/                  # Main analysis scripts
│   ├── anova_tukey_cld.py    # ANOVA + Tukey HSD + CLD generation
│   ├── summary_tables.py     # Publication-ready table creator
├── output/                   # Generated tables and CLDs
├── README.md
```

---

## Example Illustration

Let’s assume you’re working with a dataset comparing crop yields (`Yield`) across different treatments (`Treatment`).

### Dataset Structure

| Treatment | Yield |
|-----------|-------|
| A         | 2.3   |
| A         | 2.4   |
| B         | 2.0   |
| B         | 2.1   |
| C         | 1.8   |
| C         | 1.9   |

### 1. Descriptive Statistics

| Treatment | Mean | Std. Dev | Std. Error |
|-----------|------|----------|------------|
| A         | 2.35 | 0.07     | 0.05       |
| B         | 2.05 | 0.07     | 0.05       |
| C         | 1.85 | 0.07     | 0.05       |

### 2. Distribution Visualization

```python
import seaborn as sns
sns.boxplot(data=data, x='Treatment', y='Yield')
```

Displays the spread and central tendency of yield by treatment.

### 3. ANOVA Results

```python
import statsmodels.api as sm
from statsmodels.formula.api import ols

model = ols('Yield ~ C(Treatment)', data=data).fit()
anova_table = sm.stats.anova_lm(model, typ=2)
print(anova_table)
```

**Output:**

| Source        | Sum Sq | df | F       | PR(>F)  |
|---------------|--------|----|---------|---------|
| C(Treatment)  | 0.45   | 2  | 15.00   | 0.0032  |
| Residual      | 0.09   | 6  |         |         |

Interpretation: There is a statistically significant difference between treatments.

### 4. Tukey HSD Post-Hoc Test

```python
from statsmodels.stats.multicomp import pairwise_tukeyhsd

tukey = pairwise_tukeyhsd(endog=data['Yield'], groups=data['Treatment'], alpha=0.05)
print(tukey)
```

**Output:**

```
Group1 Group2  Meandiff  p-adj   Lower   Upper  Reject
-------------------------------------------------------
A      B       -0.30     0.04    -0.58   -0.02   True
A      C       -0.50     0.01    -0.78   -0.22   True
B      C       -0.20     0.07    -0.48   0.08    False
```

### 5. Compact Letter Display (CLD)

| Treatment | Mean Yield | Group |
|-----------|------------|-------|
| A         | 2.35       | a     |
| B         | 2.05       | ab    |
| C         | 1.85       | b     |

Interpretation: Treatments not sharing a letter are significantly different.

---

## Usage Example

```python
from scripts.anova_tukey_cld import run_anova_and_cld
from scripts.summary_tables import generate_summary_table

cld_df = run_anova_and_cld(data, group_col='Treatment', value_col='Yield')
summary = generate_summary_table(data, group_col='Treatment', value_col='Yield', cld_df=cld_df)
print(summary)
```

---

## Contributing

Contributions are welcome! You can:

- Add support for more post-hoc tests (e.g., Games-Howell, Dunn's)
- Improve visualization formatting
- Extend to two-way ANOVA or repeated measures

---

## License

This project is licensed under the MIT License.
