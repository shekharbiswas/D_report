# 3. Methodology

This section outlines the theoretical basis for the `nonparTrendR_test` function. We follow Bathke (2009), who proposed a unified nonparametric trend test applicable to both independent and dependent (repeated measures) sample settings. The test statistic is based on rank-based methods and yields a standardized value denoted by $\hat{\nu}$, which is used to assess the presence of monotonic trends.

## 3.1 Independent Samples

For independent samples, we assume we have $a$ ordered groups, each containing a numeric sample. The total number of observations is $N$. The core steps are:

1. **Ranking**: All observations are combined and ranked.

2. **Weight Vector**: A contrast vector $w = (w_1, w_2, \dots, w_a)$ is computed to encode the trend structure.  
   A common choice is:  
   $$
   w_i = \sum_{j < i} n_j - \sum_{j > i} n_j
   $$
   where $n_j$ is the size of group $j$.

3. **Rank Sums**: Let $R_i$ be the sum of ranks for group $i$.

4. **Test Statistic**:  
   $$
   \hat{\nu} =
   \frac{
     \sum_{i=1}^{a} w_i R_i
   }{
     \sqrt{
       \sum_{i=1}^{a} \frac{n_i(n_i - 1)}{12} w_i^2 \sum_{k \in i} \left( r_k - \frac{N+1}{2} \right)^2
     }
   }
   $$

   where $r_k$ is the rank of the $k$-th observation in group $i$.

This statistic approximates a standard normal distribution under the null hypothesis (no trend).

---

## 3.2 Dependent Samples

For dependent (repeated measures) samples, we assume $n$ units are observed under $a$ ordered conditions (e.g., time points). The steps are:

1. **Ranking within Units**: For each unit, rank the $a$ repeated measurements.

2. **Weight Vector**: Same definition of $w$ as in independent case:
   $$
   w_i = \sum_{j < i} n_j - \sum_{j > i} n_j
   $$

3. **Average Ranks per Condition**: Let $\bar{R}_i$ be the average rank for condition $i$ across all $n$ units.

4. **Test Statistic**:  
   $$
   \hat{\nu} =
   \frac{
     \sum_{i=1}^{a} w_i \bar{R}_i
   }{
     \sqrt{
       \frac{1}{n(a - 1)} \sum_{i=1}^{a} w_i^2
     }
   }
   $$

This statistic also approximates a standard normal distribution under the null hypothesis (no trend).

