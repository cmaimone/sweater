
<!-- README.md is generated from README.Rmd. Please edit that file -->

# sweater <img src="man/figures/sweater_logo.svg" align="right" height="200" />

<!-- badges: start -->

<!-- badges: end -->

The goal of sweater (Speedy Word Embedding Association TEst using R) is
to test for biases in word embeddings.

The package provides functions that are speedy. They are either
implemented in C++, or are speedy but accurate approximation of the
original implementation proposed by Caliskan et al (2017).

If your goal is to reproduce the analysis in Caliskan et al (2017),
please consider using the [original Java
program](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DX4VWP&version=2.0)
or the R package [cbn](https://github.com/conjugateprior/cbn) by Lowe.

## Installation

You can install the Github version of sweater with:

``` r
devtools::install_github("chainsawriot/sweater")
```

## Example: WEAT

This example reproduces the detection of “Math. vs Arts” gender bias in
Caliskan et al (2017).

``` r
require(sweater)
#> Loading required package: sweater
data(glove_math) # a subset of the original GLoVE word vectors

S <- c("math", "algebra", "geometry", "calculus", "equations", "computation", "numbers", "addition")
T <- c("poetry", "art", "dance", "literature", "novel", "symphony", "drama", "sculpture")
A <- c("male", "man", "boy", "brother", "he", "him", "his", "son")
B <- c("female", "woman", "girl", "sister", "she", "her", "hers", "daughter")
sw <- weat(glove_math, S, T, A, B)

# extraction of effect size
weat_es(sw)
#> [1] 1.055015
```

## A note about the effect size

By default, the effect size from the function `weat_es` is adjusted by
the pooled standard deviaion (see Page 2 of Caliskan et al. 2007). The
standardized effect size can be interpreted the way as Cohen’s d (Cohen,
1988).

One can also get the unstandardized version (aka. test statistic in the
original paper):

``` r
weat_es(sw, standardize = FALSE)
#> [1] 0.02486533
```

The original implementation assumes equal size of S and T. This
assumption can be relaxed by pooling the standard deviaion with sample
size adjustment. The function `weat_es` does it when S and T are of
different length.

Also, the effect size can be converted to point-biserial correlation
(mathematically equivalent to the Pearson’s product moment correlation).

``` r
weat_es(sw, r = TRUE)
#> [1] 0.4912066
```

## Exact test

The exact test described in Caliskan et al. (2017) is also available.
But it takes a long time to calculate.

``` r
## Don't do it. It takes a long time and is almost always significant.
weat_exact(sw)
```

Instead, please use the resampling approximaton of the exact test. The
p-value is very close to the reported 0.018.

``` r
weat_resampling(sw)
#> 
#>  Resampling approximation of the exact test in Caliskan et al. (2017)
#> 
#> data:  sw
#> bias = 0.024865, p-value = 0.0171
#> alternative hypothesis: true bias is greater than 7.245425e-05
#> sample estimates:
#>       bias 
#> 0.02486533
```

## References

1.  Caliskan, Aylin, Joanna J. Bryson, and Arvind Narayanan. “Semantics
    derived automatically from language corpora contain human-like
    biases.” Science 356.6334 (2017): 183-186.
2.  Cohen, J. (1988), Statistical Power Analysis for the Behavioral
    Sciences, 2nd Edition. Hillsdale: Lawrence Erlbaum.
3.  McGrath, R. E., & Meyer, G. J. (2006). When effect sizes disagree:
    the case of r and d. Psychological methods, 11(4), 386.
4.  Rosenthal, R. (1991), Meta-Analytic Procedures for Social Research.
    Newbury Park: Sage
