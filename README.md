
<!-- README.md is generated from README.Rmd. Please edit that file -->

# batchtma R package: Methods to address batch effects

<!-- badges: start -->
<!-- badges: end -->

The goal of the batchtma package is to provide functions for batch
effect-adjusting biomarker data. It implements different methods that
address batch effects while retaining differences between batches that
may be due to “true” underlying differences in factors that drive
biomarker values.

## Installation

Currently, the development version of batchtma can be installed from
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
df <- data.frame(tma = rep(1:2, times = 10),
                 biomarker = rep(1:2, times = 10) + runif(max = 5, n = 20))
```

Run the `adjust_batch()` function to adjust for batch effects:

``` r
df_adjust <- adjust_batch(data = df, markers = biomarker, batch = tma, method = simple)

plot_batch(data = df, marker = biomarker, batch = tma, title = "Raw data")
plot_batch(data = df_adjust, marker = biomarker_adj2, batch = tma, title = "Adjusted data")
```

<img src="man/figures/README-example-1.png" width="40%" /><img src="man/figures/README-example-2.png" width="40%" />

-   The [“Get Started”](articles/batchtma.html) vignette has details on
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

| \#  | `method =`    | Approach                      | Addressed              | Retains “true” between-batch differences |
|-----|---------------|-------------------------------|------------------------|------------------------------------------|
| 1   | —             | Unadjusted                    | —                      | Yes                                      |
| 2   | `simple`      | Simple means                  | Means                  | No                                       |
| 3   | `standardize` | Standardized batch means      | Means                  | Yes                                      |
| 4   | `ipw`         | Inverse-probability weighting | Means                  | Yes                                      |
| 5   | `quantreg`    | Quantile regression           | Low and high quantiles | Yes                                      |
| 6   | `quantnorm`   | Quantile normalization        | All ranks              | No                                       |

-   [“Get Started”](articles/batchtma.html) shows general examples on
    the different methods in absence and presence of confounding.

## References

-   Batch means after model-based standardization for confounders
    (`method = "standardize"`) are partially based on a method briefly
    described in: *Rosner B, Cook N, Portman R, Daniels S, Falkner B.
    Determination of blood pressure percentiles in normal-weight
    children: some methodological issues. [Am J Epidemiol
    2008;167(6):653-66](https://pubmed.ncbi.nlm.nih.gov/18230679).*
-   Quantile normalization (`method = "quantnorm"`) is described in:
    *Bolstad BM, Irizarry RA, Åstrand M, Speed TP. A comparison of
    normalization methods for high density oligonucleotide array data
    based on variance and bias. [Bioinformatics
    2003;19:185–193](https://pubmed.ncbi.nlm.nih.gov/12538238).*
-   Further details, all methods, and suggestions for implementation and
    interpretation are in: *Stopsack KH et al., Batch effects in tumor
    biomarker studies using tissue microarrays: Extent, impact, and
    remediation. In preparation.*
