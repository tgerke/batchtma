
<!-- README.md is generated from README.Rmd. Please edit that file -->

# `batchtma` R package: Methods to address batch effects

<!-- badges: start -->

<!-- badges: end -->

The goal of the `batchtma` is to provide functions for batch
effect-adjusting biomarker data. It implements different methods that
address batch effects while retaining differences between batches that
may be due to “true” underlying differences in factors that drive
biomarker values.

## Installation

Currently, the development version of `batchtma` can be installed from
[GitHub](https://github.com/) using:

``` r
# install.packages("remotes")  # The "remotes" package needs to be installed
library(remotes)
remotes::install_github("stopsack/batchtma")
```

Once released on [CRAN](https://CRAN.R-project.org), installation will
be possible with:

``` r
install.packages("batchtma")
```

## Usage

Load the package:

``` r
library(batchtma)
```

Define example data with batch effects:

``` r
df <- data.frame(batch = rep(1:2, times = 25),
                 biomarker = rep(c(2, 2.5, 3, 5, 6), each = 10) + runif(max = 5, n = 50))
```

Run the `adjust_batch` function to adjust for batch effects:

``` r
adjust_batch(data = df, markers = biomarker, batch = batch, method = simple)
```

  - The [“Get Started”](articles/batchtma.html) vignette has details on
    how to use the package.

## Methodology

The package implements five different approaches to obtaining batch
effect-adjusted biomarker values. The methods differ depending on what
distributional property of batch effects they address and how they
handle “true” between-batch differences. Such differences can result
from confounding when batches include samples with different
characteristics that are expected to lead to differences in biomarker
levels. They should ideally be retained when performing batch effect
adjustments.

| \# | `method =`    | Approach                      | Addressed                       | Retains “true” between-batch differences |
| -- | ------------- | ----------------------------- | ------------------------------- | ---------------------------------------- |
| 1  | —             | Unadjusted                    | —                               | Yes                                      |
| 2  | `simple`      | Simple means                  | Marginal means                  | No                                       |
| 3  | `standardize` | Standardized batch means      | Conditional means               | Yes                                      |
| 4  | `ipw`         | Inverse-probability weighting | Conditional means               | Yes                                      |
| 5  | `quantreg`    | Quantile regression           | Low/high quantiles, conditional | Yes                                      |
| 6  | `quantnorm`   | Quantile normalization        | All quantiles                   | No                                       |

  - [“Next steps (1): Different adjustment
    methods”](articles/methods.html) shows general examples on the
    different methods in absence of confounding.

  - [“Next steps (2): Batch effects and
    confounding”](articles/confounding.html) shows examples of how
    addressing confounding can retain some of the “true” between-batch
    differences.

  - [“Next steps (3): Model diagnostics”](articles/diagnostics.html)
    shows examples of how to inspect fit of the models used for batch
    effect adjustment.

## References

  - Batch means after model-based standardization for confounders
    (`method = "standardize"`) are partially based on a method briefly
    described in: *Rosner B, Cook N, Portman R, Daniels S, Falkner B.
    Determination of blood pressure percentiles in normal-weight
    children: some methodological issues. [Am J
    Epidemiol 2008;167(6):653-66](https://pubmed.ncbi.nlm.nih.gov/18230679).*
  - Quantile normalization (`method = "quantnorm"`) is described in:
    *Bolstad BM, Irizarry RA, Åstrand M, Speed TP. A comparison of
    normalization methods for high density oligonucleotide array data
    based on variance and bias.
    [Bioinformatics 2003;19:185–193](https://pubmed.ncbi.nlm.nih.gov/12538238).*
  - Further details, all methods, and suggestions for implementation and
    interpretation are in: *Stopsack KH et al., Batch effects in tumor
    biomarker studies using tissue microarrays: Extent, impact, and
    remediation. In preparation.*
