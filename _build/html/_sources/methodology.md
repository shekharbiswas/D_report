# 3. Methodology

This section outlines the theoretical basis for the `nonparTrendR_test` function. We follow Bathke (2009), who proposed a unified nonparametric trend test applicable to both independent and dependent (repeated measures) sample settings. The test statistic is based on rank-based methods and yields a standardized value denoted by $\hat{\nu}$, which is used to assess the presence of monotonic trends.

## 3.1 Independent Samples

For independent samples, we assume we have $a$ ordered groups, each containing a numeric sample. The total number of observations is $N$. The core steps are:

1. **Ranking**: All observations are combined and ranked.
2. **Weight Vector**: A contrast vector $w = (w_1, w_2, ..., w_a)$ is computed to encode the trend structure. A common choice is:
   \[
   w_i = \sum_{j < i} n_j - \sum_{j > i} n_j
   \]
   where $n_j$ is the size of group $j$.

3. **Rank Sums**: Let $R_i$ be the sum of ranks for group $i$.

4. **Test Statistic**:
   \[
   \hat{\nu} = \frac{\sum_{i=1}^a w_i R_i}{\sqrt{\sum_{i=1}^a \frac{n_i}{n_i - 1} w_i^2 \sum_{k \in i} (r_k - \frac{N+1}{2})^2}}
   \]
   where $r_k$ is the rank of the $k$-th observation in group $i$.

This statistic approximates a standard normal distribution under the null hypothesis (no trend).

<br>

## 3.2 Dependent Samples

For dependent (repeated measures) samples, each subject has $b$ repeated observations across ordered time points. We denote the data as an $n \times b$ matrix.

1. **Ranking Within Subjects**: Rank the data across columns (time points) for each subject.

2. **Column Rank Sums**: Compute $R_{\cdot j}$, the sum of ranks at time point $j$.

3. **Weight Vector**:
   \[
   w_j = j - \frac{b+1}{2}
   \]
   This vector centers the time axis to detect increasing or decreasing trends.

4. **Test Statistic**:
   \[
   \hat{\nu} = \frac{\sum_{j=1}^b w_j R_{\cdot j}}{\sqrt{\sum_{j=1}^b w_j^2 \sum_{i=1}^n (r_{ij} - \bar{r})^2 + \sum_{j \neq l} w_j w_l \sum_{i=1}^n (r_{ij} - \bar{r})(r_{il} - \bar{r})}}
   \]
   where $r_{ij}$ is the rank of subject $i$ at time point $j$, and $\bar{r} = \frac{n(b+1)}{2}$ is the average rank.

This version of $\hat{\nu}$ also approximates a standard normal distribution under the null hypothesis of no trend across time.

<br>

Both versions of the test are robust, distribution-free, and suitable for small samples or non-normal data. The same implementation logic is followed in the `nonparTrendR_test` function provided earlier.
