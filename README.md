
<!-- README.md is generated from README.Rmd. Please edit that file -->

# forestploter

<!-- badges: start -->
<!-- badges: end -->

The goal of forestploter is to create a publication-ready forest plot
with little effort. This package provide some extra displays compared to
other packages.

## Installation

You can install the development version of forestploter from
[GitHub](https://github.com/adayim/forestploter) with:

``` r
# install.packages("devtools")
devtools::install_github("adayim/forestploter")
```

## Basic usage

This is a basic example which shows you how to create a forestplot:

``` r
library(forestploter)

dt <- read.csv(system.file("extdata", "example_data.csv", package = "forestploter"))

# indent the subgroup if there is a number in the placebo column
dt$Subgroup <- ifelse(is.na(dt$Placebo), 
                      dt$Subgroup,
                      paste0("   ", dt$Subgroup))

# NA to blank
dt$Treatment <- ifelse(is.na(dt$Treatment), "", dt$Treatment)
dt$Placebo <- ifelse(is.na(dt$Placebo), "", dt$Placebo)
dt$se <- (log(dt$hi) - log(dt$est))/1.96

# Add blank column for the forest plot to display CI
dt$` ` <- paste(rep(" ", 5), collapse = " ")

# Create confidence interval column to display
dt$`Beta (95% CI)` <- ifelse(is.na(dt$se), "",
                             sprintf("%.2f (%.2f to %.2f)",
                                     dt$est, dt$low, dt$hi))

p <- forest(dt[,c(1:3, 8:9)],
            est = dt$est,
            lower = dt$low, 
            upper = dt$hi,
            sizes = dt$se,
            ci.col = 4,
            null.at = 1,
            ci.width = 2,
            arrow.lab = c("Left", "Right"),
            tick.breaks = c(0.5, 1, 2, 4))

# Draw plot
grid::grid.newpage()
grid::grid.draw(p)
```

<img src="man/figures/README-example-1.png" width="100%" />
