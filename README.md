
<!-- README.md is generated from README.Rmd. Please edit that file -->

# nbbTD

<!-- badges: start -->
<!-- badges: end -->

Multiprocessing temporal disaggregation of time series

## Description

Perform multi-processing temporal disaggregation of an annual to
quarterly or monthly set of time series. Amongst the possible methods,
the model-based proportional first difference (PFD) Denton is defined in
State Space form. It offers the possibility to add outliers (level
shift) in the Benchmark-to-Indicator (BI) ratio and import infra-annual
BI ratio manually for some periods. The function also includes an
automatic procedure to forecast the annual BI ratio and takes this
forecast into account in the estimation of the out-of-sample periods.
Instead of the automatic procedure, forecasts of the annual BI ratio can
also be defined by the user.

## Installation

``` r
remotes::install_github("clemasso/nbbTD")
```

## Usage (2 possibilities)

``` r
library("nbbTD")
```

### 1) Input in R

``` r
## Input: retail data, 2 benchmarks series to disaggregate, 3 indicators in total
Y <- cbind(
  rjd3toolkit::aggregate(rjd3toolkit::retail$RetailSalesTotal, 1),
  rjd3toolkit::aggregate(rjd3toolkit::retail$ClothingStores, 1)
)
colnames(Y) <- c("retail", "clothing")
x <- cbind(
  rjd3toolkit::aggregate(rjd3toolkit::retail$FoodAndBeverageStore, 4),
  rjd3toolkit::aggregate(rjd3toolkit::retail$AutomobileDealers, 4),
  rjd3toolkit::aggregate(rjd3toolkit::retail$WomensClothingStores, 4)
)
colnames(x) <- c("retail_food", "retail_cars", "clothing_womens")

## Specific input
outliers <- list(clothing = c("20081001" = 10))
disagBIfixed <- list(clothing = c(
  "20080101" = 4.109367, "20080401" = 4.129413,
  "20080701" = 4.139436, "20081001" = 4.109367
))

## TD
rslt <- multiTD(
    benchmarks = Y,
    indicators = x,
    model = list(retail = "Chow-Lin", clothing = "mbDenton"),
    outliers = outliers,
    disagBIfixed = disagBIfixed
)
runShiny(rslt)
print(rslt)
```

### 2) Input in Excel

``` r
## Fill structured .xlsx file with your input. See template or vignette (in the vignette map) for the structure of the Excel file
rslt <- multiTD_fromXLSX(path_data = "my_input.xlsx")
runShiny(rslt)
```

## Package Maintenance and contributing

Any contribution is welcome and should be done through pull requests
and/or issues. pull requests should include **updated tests** and
**updated documentation**. If functionality is changed, docstrings
should be added or updated.

## Licensing

The code of this project is licensed under the [European Union Public
Licence (EUPL)](https://joinup.ec.europa.eu/page/eupl-text-11-12).
