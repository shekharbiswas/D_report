# 2. Implementation of `nonparTrendR_test` Function

This section presents the full implementation of the `nonparTrendR_test` function in R based on the methodology proposed by Bathke (2009). The function handles both independent and dependent sample designs and returns a standardized test statistic, $\hat{\nu}$, and the corresponding p-value.

```r

#' Nonparametric Trend Test (Bathke, 2009)
#'
#' Performs a nonparametric trend test for independent or dependent samples
#' based on Bathke (2009).
#'
#' @param data For type = "I" (independent), a list of vectors;
#'             for type = "D" (dependent), a matrix.
#' @param type "I" or "D"
#' @param alternative "two.sided", "increasing", or "decreasing"
#'
#' @return A list with test statistic, p-value, method, data description
#' @export

nonparTrendR_test <- function(data, type = c("I", "D"), alternative = NULL) {
  type <- match.arg(type)
  data_name <- deparse(substitute(data))

  if (type == "I") {
    # Independent Samples
    if (!is.list(data) || !all(sapply(data, is.numeric))) {
      stop("'data' must be a list of numeric vectors for type = 'I'.")
    }
    if (length(data) < 2) {
      stop("At least two groups are required.")
    }

    if (is.null(alternative)) {
      alternative <- "increasing"
    }
    alternative <- match.arg(alternative, c("two.sided", "increasing", "decreasing"))

    group_sizes <- sapply(data, length)
    all_obs <- unlist(data)
    N <- length(all_obs)
    a <- length(data)
    group_ids <- rep(1:a, times = group_sizes)
    ranks <- rank(all_obs)
    group_f <- factor(group_ids)
    Rik_list <- split(ranks, group_f)
    Ri_dot <- sapply(Rik_list, sum)

    weights <- numeric(a)
    for (i in 1:a) {
      left <- if (i > 1) sum(group_sizes[1:(i - 1)]) else 0
      right <- if (i < a) sum(group_sizes[(i + 1):a]) else 0
      weights[i] <- left - right
    }

    numerator <- sum(weights * Ri_dot)
    denominator_sum <- 0
    for (i in 1:a) {
      ni <- group_sizes[i]
      Rik <- Rik_list[[i]]
      deviations <- Rik - (N + 1) / 2
      term <- (ni / (ni - 1)) * (weights[i]^2) * sum(deviations^2)
      denominator_sum <- denominator_sum + term
    }

    nu_hat <- ifelse(denominator_sum <= 1e-10, 0, numerator / sqrt(denominator_sum))
    method_str <- "Bathke Nonparametric Trend Test (Independent Samples)"
  }

  else if (type == "D") {
    # Dependent Samples
    if (!is.matrix(data)) {
      stop("'data' must be a numeric matrix for type = 'D'.")
    }

    n <- nrow(data)
    b <- ncol(data)
    if (is.null(alternative)) {
      alternative <- "increasing"
    }
    alternative <- match.arg(alternative, c("two.sided", "increasing", "decreasing"))

    ranks <- matrix(rank(data), nrow = n, ncol = b)
    Rj_dot <- colSums(ranks)
    wj <- (1:b) - (b + 1) / 2
    if (alternative == "decreasing") {
      wj <- rev(wj)
    }

    numerator <- sum(wj * Rj_dot)
    mean_rank <- (n * b + 1) / 2
    s_v_hat_squared <- 0
    for (j in 1:b) {
      s_v_hat_squared <- s_v_hat_squared + wj[j]^2 * sum((ranks[, j] - mean_rank)^2)
    }
    for (j in 1:b) {
      for (l in 1:b) {
        if (j == l) next
        s_v_hat_squared <- s_v_hat_squared + wj[j] * wj[l] * sum((ranks[, j] - mean_rank) * (ranks[, l] - mean_rank))
      }
    }

    nu_hat <- ifelse(s_v_hat_squared <= 1e-10, 0, numerator / sqrt(s_v_hat_squared))
    method_str <- "Bathke Nonparametric Trend Test (Dependent Samples)"
  }

  p_value <- switch(
    alternative,
    "two.sided" = 2 * (1 - pnorm(abs(nu_hat))),
    "increasing" = 1 - pnorm(nu_hat),
    "decreasing" = pnorm(nu_hat)
  )

  names(nu_hat) <- "nu_hat"
  return(structure(list(statistic = nu_hat, p.value = p_value, alternative = alternative,
                        method = method_str, data.name = data_name), class = "htest"))
}
```