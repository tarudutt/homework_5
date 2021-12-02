# ERHS 535 R programming for research course 2021 setting up github and homework 5

## This repository is for homework 5, which is about Washington post homicides

### We need to pick one city in the data and create a map showing the locations of the homicides in that city, using the sf framework. We need to use tigris package to download boundaries for some sub-city geography (e.g., tracts, block groups, county subdivisions) to show as a layer underneath the points showing homicides. We will use different facets for solved versus unsolved homicides and different colors to show the three race groups with the highest number of homicides for that city. 

Loading packages

```{r, message=FALSE, warning=FALSE, error=FALSE}
library(rmarkdown)
library(tidyverse)
library(forcats)
library(lubridate)
library(tigris)
library(sf)
