
<!-- README.md is generated from README.Rmd. Please edit that file -->

# sweater

<!-- badges: start -->

<!-- badges: end -->

The goal of sweater (Simple Word Embedding Association TEst using R) is
to test for biases in word embeddings.

## Installation

You can install the Github version of sweater with:

``` r
devtools::install_github("chainsawriot/sweater")
```

## Example

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
sw <- sweater(glove_math, S, T, A, B)
sweater_es(sw)
#> [1] 1.055015
```

The exact test described in Caliskan et al. (2017) is also available.
But it takes a long time to calculate.

``` r
## Don't do it. It takes a long time and is almost always significant.
exact_test(sw)
```

## References

1.  Caliskan, Aylin, Joanna J. Bryson, and Arvind Narayanan. “Semantics
    derived automatically from language corpora contain human-like
    biases.” Science 356.6334 (2017): 183-186.
