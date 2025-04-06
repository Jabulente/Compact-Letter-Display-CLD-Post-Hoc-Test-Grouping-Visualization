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

## 📂 Project Structure
```
📦 Compact Letters Display/
│── 📂 Datasets/                      # Folder for raw and processed datasets  
│   ├── Dataset.csv         # Cleaned and preprocessed data  
│
│── 📂 src/                       # Source code and core scripts  
│   ├── __init__.py               # Makes this directory a Python package  
│   ├── perform_tests.py          # Statistical test functions (e.g., ANOVA)  
│   ├── cld_assignment.py         # Functions to assign compact letter displays  
│   ├── visualization.py          # Plotting and visualization scripts  
│
│── 📂 Notebooks/                 # Jupyter Notebooks for exploratory analysis  
│   ├── exploratory_analysis.ipynb # EDA and statistical exploration  
│   ├── final_results.ipynb       # Notebook summarizing final results  
│
│── 📂 Figures/                   # Generated plots and charts  
│   ├── cld_plot.png              # Example CLD visualization  
│   ├── boxplot.png               # Boxplot with statistical comparisons  
│   ├── barplot.png               # Barplot with compact letters  
│
│── 📂 Results/                   # Processed results, tables, and summary files  
│   ├── anova_results.csv         # Results of ANOVA/statistical tests  
│   ├── cld_results.csv           # Compact letter display assignments  
│   ├── summary_table.csv         # Final structured results table  
│
│── 📂 docs/                      # Documentation and reports  
│   ├── report.pdf                # Detailed project report (if applicable)  
│
│── 📂 tests/                     # Unit tests for functions  
│   ├── test_perform_tests.py     # Tests for statistical functions  
│   ├── test_visualization.py     # Tests for visualization functions  
│
│── .gitignore                    # Ignore unnecessary files  
│── requirements.txt               # Required Python libraries  
│── setup.py                       # Script for packaging (if needed)  
│── main.py                        # Main script to execute the pipeline  
├── README.md                 # Project overview, installation, and usage  
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
