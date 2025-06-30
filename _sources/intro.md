# Nonparametric Trend Test Statistic

**Author**: Daria Suraeva  
**Date**: 2025-06-27

---

## 📑 Table of Contents

1. [Introduction](#1-introduction)  
2. [Implementation of `nonparTrendR_test` Function](function.md)  
3. [Methodology](methodology.md)  
    - 3.1 [Independent Samples](methodology.md#independent-samples)  
    - 3.2 [Dependent Samples](methodology.md#dependent-samples)  
4. [Simulation Studies](simulation.md)  
    - 4.1 [Table 1 and Table 3 Replication](simulation.md#table-1-and-table-3-replication)  
    - 4.2 [Balanced Sample Visualization](visuals.md#balanced-sample-visualization)  
    - 4.3 [Unbalanced Sample Visualization](visuals.md#unbalanced-sample-visualization)  
5. [Discussion and Interpretation](discussion.md)  
6. [Conclusion](conclusion.md)  
7. [References](references.md)

---

## 1. Introduction

Monotonic trends are frequently investigated in fields such as medicine, psychology, economics, and environmental science, where the progression of an outcome is expected to follow a consistent direction across ordered groups or time points. Traditional parametric approaches like linear regression or ANOVA rely heavily on assumptions such as normality and homoscedasticity, which may not hold in real-world data, especially when sample sizes are small or distributions are skewed.

To address these challenges, **nonparametric trend tests** offer a robust alternative by using rank-based statistics that are less sensitive to distributional assumptions. In particular, **Bathke (2009)** proposed a unified framework to assess monotonic trends in both **independent sample settings** and **dependent (repeated measures)** designs. This method uses a test statistic $\hat{\nu}$ that compares weighted rank sums across ordered groups or time levels.

The current work implements Bathke's methodology in R, offering a practical and modular solution named `nonparTrendR_test`. This implementation allows researchers and analysts to rigorously test for increasing or decreasing trends in their data, regardless of sample structure, while remaining distribution-free.

We demonstrate the test using both synthetic and real-world-inspired data, replicating key results from Bathke's original paper, and evaluating its performance under **balanced** and **unbalanced** designs. Visualizations and simulation studies accompany the implementation to deepen understanding and support practical application.
