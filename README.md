# discrim

<!-- badges: start -->
[![Lifecycle: experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![Azure pipelines build status](https://img.shields.io/azure-devops/build/topepo/discrim/2)](https://dev.azure.com/topepo/discrim/_build/latest?definitionId=1&branchName=master)
[![Azure pipelines test status](https://img.shields.io/azure-devops/tests/topepo/discrim/2?color=brightgreen&compact_message)](https://dev.azure.com/topepo/discrim/_build/latest?definitionId=1&branchName=master)
[![Travis build status](https://travis-ci.org/tidymodels/discrim.svg?branch=master)](https://travis-ci.org/tidymodels/discrim)
[![Codecov test coverage](https://codecov.io/gh/tidymodels/discrim/branch/master/graph/badge.svg)](https://codecov.io/gh/tidymodels/discrim?branch=master)
[![Azure pipelines coverage status](https://img.shields.io/azure-devops/coverage/topepo/discrim/2)](https://dev.azure.com/topepo/discrim/_build/latest?definitionId=1&branchName=master)
<!-- badges: end -->

`discrim` contains simple bindings to enable the `parsnip` package to fit various discriminant analysis models, such as 

 * Linear discriminant analysis (LDA, simple and L2 regularized)
 * Regularized discriminant analysis (RDA, via [Friedman (1989)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C7&q=%22Regularized+Discriminant+Analysis%22&btnG=))
 * Flexible discriminant analysis (FDA) using MARS features
 * Naive Bayes models
 
## Installation


``` r
devtools::install_github("tidymodels/discrim")
```

## Example

Here is a simple model using a simulated two-class data set contained in the package:


```r
library(discrim)
#> Loading required package: parsnip

parabolic_grid <-
  expand.grid(X1 = seq(-5, 5, length = 100),
              X2 = seq(-5, 5, length = 100))

fda_mod <-
  discrim_flexible(num_terms = 3) %>%
  # increase `num_terms` to find smoother boundaries
  set_engine("earth") %>%
  fit(class ~ ., data = parabolic)

parabolic_grid$fda <-
  predict(fda_mod, parabolic_grid, type = "prob")$.pred_Class1

library(ggplot2)
ggplot(parabolic, aes(x = X1, y = X2)) +
  geom_point(aes(col = class), alpha = .5) +
  geom_contour(data = parabolic_grid, aes(z = fda), col = "black", breaks = .5) +
  theme_bw() +
  theme(legend.position = "top") +
  coord_equal()
```

<img src="man/figures/README-example-1.png" title="plot of chunk example" alt="plot of chunk example" width="100%" />

