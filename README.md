# post_codes_london

# Setup
setwd('~/R/gis-london/maps-in-R')
x = c("ggmap", "rgdal", "rgeos", "maptools", "dplyr", "tidyr", "tmap", "stringr")
# install.packages(x)
lapply(x, library, character.only = TRUE)   

# Load data
london = readOGR(dsn = "postcode_data/Districts.shp")          # http://www.opendoorlogistics.com/downloads/
head(london@data)                                              # summary of first two data points 
sapply(london@data, class)                                     # class of data/column

london_dat = read.csv("postcode_data/london-postcodes.csv", 
                      stringsAsFactors = FALSE)                # https://www.doogal.co.uk/london_postcodes.php
names(london_dat) = tolower(names(london_dat))

# Wrangle
london_dat$postcode_3d = str_split_fixed(london_dat$postcode, " ", 2)[,1]

pc_pop = london_dat %>% 
  group_by(postcode_3d) %>%
  summarise(pop = n_distinct(population)) 

type_of(london_dat$population) 
sum(pc_pop$pop)
sum(pc_pop$pop)
