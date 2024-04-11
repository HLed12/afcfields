##Summary

This project aims to leverage RStudio to comprehensively analyze field size data across Africa, incorporating uncertainty visualization and temporal trends.

##Objectives

Map Uncertainty in Field Sizes: We will create our own uncertainty score and create maps that visually represent this level of uncertainty that is associated with field size measurements at various locations throughout Africa.

Analyze Average Field Size by Region: Calculate and compare the average field sizes across different regions in Africa.

Track Average Field Size Trends: Use data to investigate how average field sizes have changed throughout Africa. We will analyze time series of locations to find the trends.

##Approach and Method

We will possibly use the packages below to gather the data, clean it and prepare it for spatial mapping/calculations. Then each of us will work on one of the three sections of the project.

Sfarrow package will be used to open geoparquet polygons as sf objects

Terra package to read, write, analyze and model our spatial data. As well as for Intersect, buffer, and creating vectors.

Ggplot2 for plotting

Sf for supporting spatial vector data, used to read write and convert projections.

Dplyr for filtering and manipulations

Readr to read csv

We can use tidyr in case we want to do any pivot_widers or csv manipulations

##Data/code

Our code will be split into three sections. We will need to map uncertainty throughout sites, find average field sizes by region, and find trends in the field sizes over several years.
