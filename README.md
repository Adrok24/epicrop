# *epicrop*: Simulation Modelling of Crop Diseases Using a Susceptible-Exposed-Infectious-Removed (SEIR) Model

<img align="right" src="man/figures/logo.png">

<!-- badges: start -->

[![tic](https://github.com/adamhsparks/epicrop/workflows/tic/badge.svg?branch=main)](https://github.com/adamhsparks/epicrop/actions)
[![codecov](https://codecov.io/gh/adamhsparks/epicrop/branch/main/graph/badge.svg?token=NWrKsX9MaP)](https://codecov.io/gh/adamhsparks/epicrop)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](https://www.repostatus.org/badges/latest/active.svg)](https://www.repostatus.org/#active)
<!-- badges: end -->

A fork of the R [cropsim package](https://r-forge.r-project.org/R/?group_id=335) designed to make using the EPIRICE model (Savary et al. 2012) for rice diseases easier to use.
This version provides easy to use functions to fetch weather data from NASA POWER, via the [*nasapower*](https://cran.r-project.org/package=nasapower) package (Sparks 2018, Sparks 2020) and predict disease severity of five rice diseases using a generic SEIR model (Zadoks 1971) function, `SEIR()`.

The original EPIRICE manuscript, Savary et al. (2012), which details the model and results of its use to model global epidemics of rice diseases, was published in *Crop Protection* detailing global unmanaged disease risk of bacterial blight, brown spot, leaf blast, sheath blight and tungro, which are included in this package.

# Quick start

You can easily simulate any of the five diseases for rice grown anywhere in the world for years from 1983 to near current using `get_wth()` to fetch data from the [NASA POWER web API](https://power.larc.nasa.gov). 
Alternatively, you can supply your own weather data for any time period as long as it fits the model’s requirements.

*epicrop* is not yet on CRAN.
You can install it this way.

``` r
if (!require("remotes"))
  install.packages("remotes")
remotes::install_github("adamhsparks/epicrop"
)
```

## Get weather data

First you need to provide weather data for the model. *epicrop* provides the `get_wth()` function to do this. Using it you can fetch weather data for any place in the world from 1983 to near present by providing the
longitude and latitude and dates.

``` r
library(epicrop)

# Fetch weather for year 2000 wet season at the IRRI Zeigler Experiment Station
 wth <- get_wth(
   lonlat = c(121.25562, 14.6774),
   dates = c("2000-06-30", "2000-12-31")
 )

wth
```
    ##        YYYYMMDD DOY  TEMP  TMIN  TMAX  TDEW  RHUM  RAIN   LAT   LON
    ##   1: 2000-06-30 182 25.94 22.97 29.19 23.63 87.22 11.36 14.68 121.3
    ##   2: 2000-07-01 183 25.34 23.65 28.31 23.76 91.07 24.87 14.68 121.3
    ##   3: 2000-07-02 184 25.99 23.28 29.91 23.40 85.71 17.63 14.68 121.3
    ##   4: 2000-07-03 185 25.35 24.00 27.23 24.32 94.01 33.52 14.68 121.3
    ##   5: 2000-07-04 186 25.58 24.20 27.35 24.42 93.28 16.21 14.68 121.3
    ##  ---                                                               
    ## 181: 2000-12-27 362 24.27 22.65 26.20 23.06 92.93 26.32 14.68 121.3
    ## 182: 2000-12-28 363 24.39 23.16 26.87 23.08 92.33  6.67 14.68 121.3
    ## 183: 2000-12-29 364 24.39 22.58 27.46 22.94 91.57  7.19 14.68 121.3
    ## 184: 2000-12-30 365 24.96 22.60 28.50 22.67 87.13  2.97 14.68 121.3
    ## 185: 2000-12-31 366 24.22 21.26 28.49 21.95 87.21  3.04 14.68 121.3

## Modelling bacterial blight disease severity

Once you have the weather data, run the model for any of the five rice
diseases by providing the emergence or crop establishment date for
transplanted rice.

``` r
bb <- predict_bacterial_blight(wth, emergence = "2000-07-01")

bb
```
Once you have the results you can visualise them.

``` r
library(ggplot2)

ggplot(data = bb,
       aes(x = dates,
           y = severity)) +
  labs(y = "Severity",
       x = "Date") +
  geom_line() +
  geom_point() +
  labs(title = "Bacterial blight disease progress over time",
       subtitle = "Results for wet season year 2000 at IRRI Zeigler Experiment
       Station shown",
       caption = "Weather data acknowledgement:\nThese data were obtained from
       the NASA Langley Research Center POWER Project\nfunded through the NASA
       Earth Science Directorate Applied Science Program.") +
  theme_light()
```

<img src="man/figures/plot_severity-1.png">

# Meta

Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

# References

Serge Savary, Andrew Nelson, Laetitia Willocquet, Ireneo Pangga and Jorrel Aunario. Modeling and mapping potential epidemics of rice diseases globally. *Crop Protection*, Volume 34, 2012, Pages 6-17, ISSN
0261-2194 DOI: [10.1016/j.cropro.2011.11.009](https://doi.org/10.1016/j.cropro.2011.11.009).

Serge Savary, Stacia Stetkiewicz, François Brun, and Laetitia Willocquet. Modelling and Mapping Potential Epidemics of Wheat Diseases-Examples on Leaf Rust and Septoria Tritici Blotch Using EPIWHEAT. *European Journal of Plant Pathology* 142, no. 4 (August 1, 2015): 771–90. DOI: [10.1007/s10658-015-0650-7](https://doi.org/10.1007/s10658-015-0650-7).

Jan C. Zadoks. Systems Analysis and the Dynamics of Epidemics. Laboratory of Phytopathology, Agricultural University, Wageningen, The Netherlands; *Phytopathology* 61:600. DOI: [10.1094/Phyto-61-600](https://doi.org/10.1094/Phyto-61-600).

Adam Sparks (2018). nasapower: A NASA POWER Global Meteorology, Surface Solar Energy and Climatology Data
Client for R. Journal of Open Source Software, 3(30), 1035, [10.21105/joss.01035](https://doi.org/10.21105/joss.01035).

Adam Sparks (2020). _nasapower: NASA-POWER Data from R_. R package version 3.0.1, URL: <https://CRAN.R-project.org/package=nasapower>.
