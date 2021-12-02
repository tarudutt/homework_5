# ERHS 535 R programming for research course 2021 setting up github and homework 5

## This repository is for homework 5, which is about Washington post homicides

### We need to pick one city in the data and create a map showing the locations of the homicides in that city, using the sf framework. We need to use tigris package to download boundaries for some sub-city geography (e.g., tracts, block groups, county subdivisions) to show as a layer underneath the points showing homicides. We will use different facets for solved versus unsolved homicides and different colors to show the three race groups with the highest number of homicides for that city. 

## Loading packages

```{r, message=FALSE, warning=FALSE, error=FALSE}
library(rmarkdown)
library(tidyverse)
library(forcats)
library(lubridate)
library(tigris)
library(sf)
```

## Loading data

```{r, message=FALSE, warning=FALSE, error=FALSE}
homicides_data <- read_csv("../data/homicide-data.csv")
```

## Cleaning data

For cleaning data:
1. I am uniting city and state column as "city_state"
2. Setting date class for reported date
3. Changing names of victim first and victim last columns to sentence as all letters are in uppercase.
4. 
```{r}
homicide_data_atlanta <- homicide_data %>% 
  unite(col = "city_state", city, state, sep = ",") %>% 
  mutate(victim_first = str_to_sentence(victim_first),
         victim_last = str_to_sentence(victim_last),
         disposition = str_to_lower(disposition),
         disposition = str_replace(disposition, "closed without arrest", "unsolved"),
         disposition = str_replace(disposition, "open/no arrest", "unsolved"),
         disposition = str_replace(disposition, "closed by arrest", "solved")) %>% 
  filter(city_state == "Atlanta,GA") %>% 
 
 homicide_data_atlanta
```

## Creating map

```{r, message=FALSE, warning=FALSE, error=FALSE}

# Adding race parameter

homicide_data_atlanta$victim_race <- as.factor(homicide_data_atlanta$victim_race) 


# Creating sf 


sf_homicide_data_atlanta <- st_as_sf(x = homicide_data_atlanta, 
                                     coords = c("lon", "lat")) %>% 
  st_set_crs(4269) 

# Plot graph

atlanta <- tracts("GA", county = "fulton", cb = TRUE)

ggplot() +
  geom_sf(data = atlanta) +
  geom_sf(data = sf_homicide_data_atlanta, alpha = 0.5, 
          aes(color = fct_lump(victim_race, n = 3))) +
  facet_wrap( ~ disposition) +
  scale_color_viridis_d() +
  ggtitle("Atlanta homicide incidents", 
          subtitle = "Separated by solved or unsolved homicides") +
  scale_color_discrete(name = 'Divided by Race') +
  theme(axis.text.x = element_blank(), 
        axis.text.y = element_blank(),
        plot.title = element_text(color="black", size=20, face="bold"),
        strip.text = element_text(size=14, face = "bold"))
  
  
```
