# Simpson's Paradox in Kidney Stone Treatment: A Statistical Investigation

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Complete-success.svg)]()

## 📋 Table of Contents
- [Overview](#overview)
- [The Paradox](#the-paradox)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [Installation & Usage](#installation--usage)
- [Results & Visualizations](#results--visualizations)
- [Statistical Methods](#statistical-methods)
- [Clinical Implications](#clinical-implications)
- [Limitations](#limitations)
- [Contributors](#contributors)
- [References](#references)
- [License](#license)

---

## 🔬 Overview

This project investigates a fascinating statistical phenomenon known as **Simpson's Paradox** using real medical data from a 1986 study published in *The British Medical Journal*. The study compared two kidney stone removal procedures across 700 patients and revealed a counterintuitive finding: a treatment that appeared superior overall was actually inferior when examining patient subgroups.

### What is Simpson's Paradox?

Simpson's Paradox occurs when a trend that appears in different groups of data disappears or reverses when the groups are combined. It is a cautionary tale about the dangers of aggregating data without accounting for confounding variables.

### Research Question

**"After accounting for kidney stone severity, which treatment method (open surgery vs. percutaneous nephrolithotomy) is more effective for kidney stone removal?"**

---

## 🎭 The Paradox

### The Puzzle

When urologists analyzed their data, they discovered something perplexing:

**Overall (Aggregated) Results:**
```
Treatment A (Open Surgery):        78.0% success rate
Treatment B (Percutaneous):        82.6% success rate
→ Treatment B appears BETTER by 4.6 percentage points
```

**But when examining small and large stones separately:**
```
Small Stones:
Treatment A: 93.1% success  |  Treatment B: 86.7% success
→ Treatment A is BETTER by 6.4 percentage points

Large Stones:
Treatment A: 73.0% success  |  Treatment B: 68.8% success
→ Treatment A is BETTER by 4.2 percentage points
```

**The Paradox:** Treatment B appears better overall, but Treatment A is better in BOTH subgroups!

### The Resolution

The paradox arises from **selection bias** and **confounding**:
- Treatment A was used predominantly for difficult cases (large stones: 76.7%)
- Treatment B was used predominantly for easier cases (small stones: 75.6%)
- Stone size is the true driver of outcomes, not treatment choice
- Non-random treatment allocation created the misleading aggregate statistic

---

## 📊 Dataset

### Source
**kidney_stone_data.csv** - Based on the 1986 British Medical Journal study by Charig et al.

### Structure
- **Sample Size:** 700 patients
- **Variables:** 3
- **Study Period:** 1986
- **Institution:** London urological center

### Variables

| Variable | Type | Description | Values |
|----------|------|-------------|--------|
| `treatment` | Categorical | Treatment method | A = Open surgery (invasive)<br>B = Percutaneous nephrolithotomy (less invasive) |
| `stone_size` | Categorical | Kidney stone size | small, large |
| `success` | Binary | Treatment outcome | 1 = successful removal<br>0 = unsuccessful |

### Data Distribution
```
Treatment Groups:
├── Treatment A: 350 patients (50.0%)
│   ├── Small stones: 87 patients (24.9%)
│   └── Large stones: 263 patients (75.1%)
│
└── Treatment B: 350 patients (50.0%)
    ├── Small stones: 270 patients (77.1%)
    └── Large stones: 80 patients (22.9%)

Overall Success Rate: 80.3% (562/700)
```

### Data Quality
- ✅ No missing values
- ✅ No invalid entries
- ✅ Balanced treatment allocation
- ✅ Appropriate sample size for analysis

---

## 🔍 Methodology

### Analytical Framework: CRISP-DM

This project follows the Cross-Industry Standard Process for Data Mining:
```
1. Business Understanding → Define research question and Simpson's Paradox hypothesis
2. Data Understanding → Exploratory data analysis and distribution assessment
3. Data Preparation → Variable encoding and data transformation
4. Modeling → Multiple logistic regression models
5. Evaluation → Model comparison and paradox verification
6. Deployment → Clinical recommendations and guidelines
```

### Statistical Approach

#### 1. Descriptive Analysis
- Cross-tabulation of treatment by stone size
- Success rate calculation overall and by subgroup
- Identification of selection bias patterns

#### 2. Stratified Analysis
- Separate analysis for small stones
- Separate analysis for large stones
- Comparison with aggregated results

#### 3. Logistic Regression Modeling

**Three models with increasing complexity:**
```python
Model 1 (Unadjusted): SUCCESS ~ TREATMENT
Model 2 (Stone Size):  SUCCESS ~ STONE_SIZE
Model 3 (Full Model):  SUCCESS ~ TREATMENT + STONE_SIZE
```

#### 4. Model Evaluation
- Akaike Information Criterion (AIC)
- Pseudo R-squared
- Likelihood ratio tests
- Odds ratio estimation with 95% confidence intervals

#### 5. Visualization
- Bar charts comparing success rates
- Forest plots showing odds ratios
- Heatmaps of predicted probabilities
- Comprehensive model comparison graphics

---

## 🎯 Key Findings

### 1. Simpson's Paradox Confirmed ✓

The analysis definitively demonstrates Simpson's Paradox:
- Aggregate data misleadingly favors Treatment B (+4.6%)
- Stratified data consistently favors Treatment A (+6.4% and +4.2%)
- Direction of treatment effect completely reverses upon disaggregation

### 2. Statistical Results

#### Model Comparison

| Model | Predictors | AIC | Pseudo R² | Treatment B OR | P-value |
|-------|-----------|-----|-----------|----------------|---------|
| Model 1 | Treatment only | 696.67 | 0.0033 | 1.336 | 0.129 |
| Model 2 | Stone size only | 669.31 | 0.0427 | N/A | N/A |
| **Model 3** | **Both** | **668.87** | **0.0462** | **0.700** | **0.119** |

**Best Model:** Model 3 (lowest AIC, highest explanatory power)

#### Treatment Effect

**Unadjusted (Model 1):**
```
OR = 1.336 (95% CI: 0.919-1.943), p = 0.129
→ Treatment B appears 34% better (not significant)
```

**Adjusted for Stone Size (Model 3):**
```
OR = 0.700 (95% CI: 0.447-1.096), p = 0.119
→ Treatment A appears 43% better (not significant)
→ EFFECT DIRECTION REVERSED!
```

#### Stone Size Effect
```
Large stones OR = 0.283 (95% CI: 0.178-0.453), p < 0.001
→ 72% reduction in success odds for large stones
→ Highly significant predictor
```

### 3. Predicted Success Probabilities

| Scenario | Predicted Success Rate |
|----------|----------------------|
| Treatment A + Small stones | **90.8%** |
| Treatment B + Small stones | 87.4% |
| Treatment A + Large stones | **73.8%** |
| Treatment B + Large stones | 66.3% |

### 4. Root Cause Analysis

The paradox stems from **confounding by indication**:
```
Selection Bias Pattern:
┌─────────────────────────────────────────────────────┐
│ Doctors preferentially assigned:                   │
│ • Treatment A → Difficult cases (large stones)     │
│ • Treatment B → Easier cases (small stones)        │
│                                                     │
│ Result: Treatment B's higher overall success rate  │
│ reflects easier case mix, not superior efficacy    │
└─────────────────────────────────────────────────────┘
```

### 5. Clinical Conclusions

**Primary Finding:**
> No statistically significant difference between treatments when properly controlling for stone size (p = 0.119). Treatment choice should be driven by patient-specific factors, not efficacy concerns.

**Key Insight:**
> Stone size, not treatment type, is the dominant predictor of success. Large stones have 72% lower success odds regardless of treatment method.

---

## 📁 Project Structure
```
kidney-stone-analysis/
│
├── data/
│   ├── kidney_stone_data.csv           # Original dataset
│   ├── data_summary.json                # Data description and metadata
│   └── regression_analysis_results.json # Model outputs
│
├── notebooks/
│   └── kidney_stone_analysis.ipynb      # Main analysis notebook
│
├── output/
│   ├── kidney_stone_data.json           # Processed data
│   ├── simpsons_paradox_focused.png     # Key visualization
│   └── model_comparison_plots.png       # Statistical charts
│
├──                      
│
├── README.md                            # This file
├── requirements.txt                     # Python dependencies
├── CRISP-DM_Framework.md                # Detailed methodology
├── LICENSE                              # MIT License
└── .gitignore                           # Git ignore rules
```

---

## 🚀 Installation & Usage

### Prerequisites

- Python 3.8 or higher
- pip package manager
- Jupyter Notebook (for interactive analysis)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/kidney-stone-simpsons-paradox.git
cd kidney-stone-simpsons-paradox
```

2. **Create virtual environment** (recommended)
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

### Required Packages
```
pandas>=1.3.0
numpy>=1.21.0
matplotlib>=3.4.0
seaborn>=0.11.0
scipy>=1.7.0
scikit-learn>=0.24.0
statsmodels>=0.13.0
jupyter>=1.0.0
```

### Running the Analysis

#### Option 1: Jupyter Notebook (Interactive)
```bash
jupyter notebook notebooks/kidney_stone_analysis.ipynb
```

#### Option 2: Python Script
```bash
python src/main_analysis.py
```

#### Option 3: Step-by-Step Modules
```python
from src.data_preparation import load_and_prepare_data
from src.modeling import fit_logistic_models
from src.visualization import create_paradox_plots

# Load data
df = load_and_prepare_data('data/kidney_stone_data.csv')

# Fit models
models = fit_logistic_models(df)

# Create visualizations
create_paradox_plots(df, models)
```

### Quick Start Example
```python
import pandas as pd
from src.modeling import demonstrate_simpsons_paradox

# Load data
df = pd.read_csv('data/kidney_stone_data.csv')

# Demonstrate the paradox
results = demonstrate_simpsons_paradox(df)

print(f"Overall: Treatment B success rate = {results['overall_b']:.1%}")
print(f"Overall: Treatment A success rate = {results['overall_a']:.1%}")
print(f"\nSmall stones: Treatment A is better by {results['small_diff']:+.1%}")
print(f"Large stones: Treatment A is better by {results['large_diff']:+.1%}")
```

---

## 📈 Results & Visualizations

### 1. Simpson's Paradox Demonstration

<img width="1484" height="1178" alt="image" src="https://github.com/user-attachments/assets/6411a5c7-61cd-448a-99d1-8959bbeb2036" />


This visualization clearly shows:
- Treatment B appears better in aggregate (82.6% vs 78.0%)
- Treatment A is better in both subgroups (small: 93.1% vs 86.7%; large: 73.0% vs 68.8%)
- The paradoxical reversal of the treatment effect

### 2. Model Comparison Forest Plot

<img width="1184" height="784" alt="image" src="https://github.com/user-attachments/assets/110d3e2a-717d-4a56-8e6a-24cff672f9a9" />


Odds ratio comparison:
- Unadjusted model: OR = 1.34 (favors Treatment B)
- Adjusted model: OR = 0.70 (favors Treatment A)
- Demonstrates effect reversal upon controlling for stone size

### 3. Predicted Probability Heatmap

<img width="1581" height="1178" alt="image" src="https://github.com/user-attachments/assets/8897becd-6056-4ada-b69c-e5ba4e6759b4" />


Shows predicted success rates for all treatment-stone size combinations:
- Highest: Treatment A + Small stones (90.8%)
- Lowest: Treatment B + Large stones (66.3%)

---

## 📐 Statistical Methods

### Logistic Regression

The logit model relates the probability of success to predictor variables:
```
log(p/(1-p)) = β₀ + β₁(Treatment) + β₂(Stone_Size)

where:
- p = probability of successful treatment
- β₀ = intercept (baseline log-odds)
- β₁ = treatment effect coefficient
- β₂ = stone size effect coefficient
```

### Odds Ratios

Odds ratios quantify the effect of predictors:
```
OR = exp(β)

Interpretation:
- OR > 1: Predictor increases odds of success
- OR < 1: Predictor decreases odds of success
- OR = 1: No effect
```

### Model Selection Criteria

**Akaike Information Criterion (AIC):**
```
AIC = -2 × log-likelihood + 2k

where k = number of parameters
→ Lower AIC indicates better model fit
```

**Pseudo R-squared (McFadden's):**
```
R² = 1 - (log-likelihood_model / log-likelihood_null)

→ Proportion of uncertainty explained by model
```

### Hypothesis Testing

**Likelihood Ratio Test:**
```
LR = -2 × (log-likelihood_reduced - log-likelihood_full)
→ Tests whether additional predictors improve fit
→ Follows χ² distribution
```

### Confidence Intervals

95% Confidence intervals for odds ratios:
```
CI = exp(β ± 1.96 × SE)

→ Range likely to contain true population parameter
```

---

## 🏥 Clinical Implications

### Evidence-Based Recommendations

#### 1. Treatment Selection Guidelines

**Primary Recommendation:**
```
✓ Either Treatment A or Treatment B can be used effectively
✗ No significant efficacy difference (p = 0.119)
→ Base treatment choice on patient-specific factors
```

**Patient-Specific Decision Factors:**
- Comorbidities and surgical risk profile
- Stone characteristics beyond size
- Cost and insurance coverage
- Recovery time considerations
- Patient preference and quality of life

#### 2. Prognostic Counseling

**Set Realistic Expectations Based on Stone Size:**

| Stone Size | Expected Success Rate | Counseling Message |
|------------|----------------------|-------------------|
| Small | ~87-93% | Excellent prognosis with either treatment |
| Large | ~67-74% | Good prognosis, but higher failure risk |

**Key Message to Patients:**
> "Stone size is the most important factor determining treatment success. Both procedures are effective, and we'll choose based on what's best for your specific situation."

#### 3. Quality Improvement Implications

**For Healthcare Systems:**
- Monitor treatment allocation patterns for selection bias
- Track outcomes stratified by stone size
- Avoid relying solely on aggregate success rates for quality metrics
- Implement risk-adjusted outcome reporting

**For Researchers:**
- Design randomized controlled trials to eliminate selection bias
- Always control for confounding variables in analysis
- Report stratified results, not just aggregate statistics
- Be aware of Simpson's Paradox in observational data

#### 4. Medical Education

**Teaching Points:**
- Classic example of Simpson's Paradox for statistics education
- Demonstrates importance of multivariable analysis
- Illustrates confounding by indication
- Shows dangers of aggregate metrics in quality assessment

---

## ⚠️ Limitations

### Study Design Limitations

1. **Observational Data**
   - Not a randomized controlled trial
   - Cannot definitively establish causality
   - Selection bias evident in treatment allocation
   - Unmeasured confounding may exist

2. **Limited Variables**
   - Only stone size and treatment captured
   - Missing potential confounders:
     - Patient age and comorbidities
     - Stone location and composition
     - Surgeon experience
     - Hospital characteristics

3. **Sample Size Constraints**
   - Small subgroups limit precision
     - Treatment A + small stones: n=87
     - Treatment B + large stones: n=80
   - May be underpowered to detect smaller differences
   - Wide confidence intervals in some analyses

4. **Historical Data**
   - Study conducted in 1986
   - Treatment techniques have evolved
   - Modern success rates may differ
   - Generalizability to current practice uncertain

### Statistical Limitations

1. **Non-Significance Does Not Prove Equivalence**
   - p = 0.119 means "insufficient evidence of difference"
   - Not the same as "treatments are equivalent"
   - Confidence intervals include clinically meaningful effects
   - Formal equivalence testing not performed

2. **Multiple Comparisons**
   - Subgroup analyses not adjusted for multiplicity
   - Increased Type I error risk
   - Results should be considered exploratory

3. **Model Assumptions**
   - Logistic regression assumes:
     - Linear relationship between log-odds and predictors
     - Independence of observations
     - No omitted variable bias
   - Violations may affect estimates

### Generalizability Limitations

1. **Population Specificity**
   - Results from 1986 London hospital
   - May not apply to:
     - Different geographic regions
     - Modern patient populations
     - Different healthcare systems

2. **Operational Definitions**
   - "Small" vs "large" stone criteria not specified
   - Definitions may vary between institutions
   - Results depend on specific size threshold used

3. **Treatment Evolution**
   - Surgical techniques improved since 1986
   - Newer technologies available (e.g., laser lithotripsy)
   - Success rates likely higher with modern equipment

---

## 👥 Contributors

**Principal Investigator:** [Phinidy George]
- Data analysis and statistical modeling
- Visualization development
- Clinical interpretation

**Original Study Authors:**
- Charig CR, Webb DR, Payne SR, Wickham JE (1986)
- *Comparison of treatment of renal calculi by open surgery, percutaneous nephrolithotomy, and extracorporeal shockwave lithotripsy.*
- British Medical Journal (Clinical Research Ed.), 292(6524), 879-882

**Acknowledgments:**
- DataCamp for project framework
- Statistical consulting support
- Medical advisory board for clinical interpretation

---

## 📚 References

### Primary Literature

1. **Charig, C. R., Webb, D. R., Payne, S. R., & Wickham, J. E. (1986).** 
   *Comparison of treatment of renal calculi by open surgery, percutaneous nephrolithotomy, and extracorporeal shockwave lithotripsy.* 
   British Medical Journal (Clinical Research Ed.), 292(6524), 879-882.
   DOI: 10.1136/bmj.292.6524.879

2. **Julious, S. A., & Mullee, M. A. (1994).**
   *Confounding and Simpson's paradox.*
   BMJ, 309(6967), 1480-1481.
   DOI: 10.1136/bmj.309.6967.1480

### Statistical Methods

3. **Simpson, E. H. (1951).**
   *The interpretation of interaction in contingency tables.*
   Journal of the Royal Statistical Society: Series B, 13(2), 238-241.

4. **Pearl, J. (2014).**
   *Comment: Understanding Simpson's paradox.*
   The American Statistician, 68(1), 8-13.
   DOI: 10.1080/00031305.2014.876829

5. **Hosmer, D. W., Lemeshow, S., & Sturdivant, R. X. (2013).**
   *Applied Logistic Regression* (3rd ed.).
   Wiley Series in Probability and Statistics.

### Methodological References

6. **Hernán, M. A., Clayton, D., & Keiding, N. (2011).**
   *The Simpson's paradox unraveled.*
   International Journal of Epidemiology, 40(3), 780-785.
   DOI: 10.1093/ije/dyr041

7. **Tu, Y. K., Gunnell, D., & Gilthorpe, M. S. (2008).**
   *Simpson's paradox, Lord's paradox, and suppression effects are the same phenomenon–the reversal paradox.*
   Emerging Themes in Epidemiology, 5(1), 2.
   DOI: 10.1186/1742-7622-5-2

### Data Science Methodology

8. **Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C., & Wirth, R. (2000).**
   *CRISP-DM 1.0: Step-by-step data mining guide.*
   SPSS Inc.

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### MIT License Summary
```
MIT License

Copyright (c) 2024 [Your Name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

[Full license text in LICENSE file]
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

### How to Contribute

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/AmazingFeature`)
3. **Commit your changes** (`git commit -m 'Add some AmazingFeature'`)
4. **Push to the branch** (`git push origin feature/AmazingFeature`)
5. **Open a Pull Request**

### Contribution Areas

- 🐛 Bug fixes and error corrections
- 📊 Additional visualizations
- 📈 Extended statistical analyses
- 📚 Documentation improvements
- 🧪 Unit tests and validation
- 🔬 Replication with modern data

### Code Standards

- Follow PEP 8 style guidelines for Python code
- Include docstrings for all functions
- Add unit tests for new features
- Update documentation as needed

---

## 📞 Contact

**Project Maintainer:** [Phinidy George]
- Email: phinidygeorge01@gmail.com

---

## 🌟 Star History

If you find this project useful, please consider giving it a star! ⭐

[![Star History Chart](https://api.star-history.com/svg?repos=[https://github.com/BigTime5/Kidney-Stone-Treatment-Analysis&type=Date)](https://star-history.com/#https://github.com/BigTime5/Kidney-Stone-Treatment-Analysis&Date)

---

## 📝 Citation

If you use this analysis in your research or teaching, please cite:
```bibtex
@misc{kidney_stone_simpsons_paradox,
  author = {Your Name},
  title = {Simpson's Paradox in Kidney Stone Treatment: A Statistical Investigation},
  year = {2024},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/yourusername/kidney-stone-simpsons-paradox}}
}
```

---

## 🎓 Educational Use

This project is ideal for:
- **Statistics courses:** Teaching Simpson's Paradox and confounding
- **Data science bootcamps:** Demonstrating CRISP-DM methodology
- **Medical education:** Illustrating evidence-based medicine principles
- **Research methods classes:** Showing importance of study design

**Suggested Discussion Questions:**
1. What would happen if stone size was not recorded in this study?
2. How would randomization have changed the results?
3. What other medical scenarios might exhibit Simpson's Paradox?
4. How should aggregate quality metrics be interpreted in healthcare?

---

## 🔮 Future Work

Potential extensions of this analysis:

- [ ] Replication with modern kidney stone treatment data
- [ ] Incorporation of additional confounders (age, BMI, stone composition)
- [ ] Cost-effectiveness analysis comparing treatments
- [ ] Meta-analysis of multiple kidney stone studies
- [ ] Machine learning models for treatment recommendation
- [ ] Interactive web dashboard for exploring the paradox
- [ ] Simulation study of Simpson's Paradox under different scenarios
- [ ] Bayesian analysis incorporating prior clinical knowledge

---

## 📊 Project Metrics

![GitHub repo size](https://img.shields.io/github/repo-size/yourusername/kidney-stone-simpsons-paradox)
![GitHub code size](https://img.shields.io/github/languages/code-size/yourusername/kidney-stone-simpsons-paradox)
![GitHub last commit](https://img.shields.io/github/last-commit/yourusername/kidney-stone-simpsons-paradox)
![GitHub issues](https://img.shields.io/github/issues/yourusername/kidney-stone-simpsons-paradox)
![GitHub pull requests](https://img.shields.io/github/issues-pr/yourusername/kidney-stone-simpsons-paradox)

---

<div align="center">

**Made with ❤️ for the advancement of statistical literacy and evidence-based medicine**

[⬆ Back to Top](#simpsons-paradox-in-kidney-stone-treatment-a-statistical-investigation)

</div>
