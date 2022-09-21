
<!-- README.md is generated from README.Rmd. Please edit that file -->

# halfmoon

<!-- badges: start -->
<!-- badges: end -->

The goal of halfmoon is to cultivate balance in propensity score models.

## Installation

You can install the development version of halfmoon from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("malcolmbarrett/halfmoon")
```

## Example

halfmoon includes several techniques for assessing the balance created
by propensity score weights.

``` r
library(halfmoon)
library(ggplot2)

# weighted mirrored histograms
ggplot(nhefs_weights, aes(.fitted)) +
  geom_mirror_histogram(
    aes(group = qsmk),
    bins = 50
  ) +
  geom_mirror_histogram(
    aes(fill = qsmk, weight = w_ate),
    bins = 50,
    alpha = 0.5
  ) + scale_y_continuous(labels = abs)
```

<img src="man/figures/README-example-1.png" width="100%" />

``` r

# weighted ecdf
ggplot(
  nhefs_weights,
  aes(x = smokeyrs, color = qsmk)
) +
  geom_ecdf(aes(weights = w_ato)) +
  xlab("Smoking Years") +
  ylab("Proportion <= x")
```

<img src="man/figures/README-example-2.png" width="100%" />

``` r

# weighted SMDs
plot_df <- tidy_smd(
  nhefs_weights,
  race:active,
  .group = qsmk,
  .wts = starts_with("w_")
)

ggplot(
  plot_df,
  aes(
    x = abs(smd),
    y = variable,
    group = weights,
    color = weights,
    fill = weights
  )
) +
  geom_love()
```

<img src="man/figures/README-example-3.png" width="100%" />
