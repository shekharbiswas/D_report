# 5. Discussion and Interpretation

The simulation results provide strong evidence for the effectiveness of nonparametric trend tests under both balanced and unbalanced designs.

## Key Takeaways

- **High Power Across Conditions**: All three tests — ν̂ (Bathke's unified test), T_HN (Hettmansperger-Norton), and T_JT (Jonckheere-Terpstra) — show consistently high power under both Normal and Binomial distributions.
- **Robustness to Unbalancing**: The simulated power remains near 100% even with unbalanced sample sizes. This suggests the methods are well-suited for real-world scenarios where perfect group balance is uncommon.
- **Balanced vs. Unbalanced**: While power increases with larger sample sizes in balanced designs, the advantage diminishes in unbalanced setups due to already high baseline performance.
- **Distribution Type**: No significant drop in performance was noted between continuous (Normal) and discrete (Binomial) outcomes, reaffirming the nonparametric robustness of these methods.
- **Test Comparisons**:
  - ν̂ provides a unified and flexible structure, adaptable to both independent and dependent samples.
  - T_HN is simple and powerful for detecting linear trends.
  - T_JT remains a classical standard for ordered alternatives and shows excellent consistency.

## Limitations

- The simulations assume idealized conditions (independent errors, known distribution types).
- Real-world applications may involve missing data, ties, or more complex correlation structures not addressed here.

## Practical Implications

Researchers working with ordinal, ranked, or non-normally distributed data can confidently use these nonparametric trend tests to assess ordered effects. Their flexibility and robustness make them strong alternatives to parametric ANOVA-based approaches, especially in small-sample or distributionally-challenged studies.
