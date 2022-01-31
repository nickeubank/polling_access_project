---
title: Results Methodologies
date: 2022-01-20
---

## Introduction

Processes pertaining to  sourcing, cleaning, and analyzing our data.

<!--more--> 


## College Demographic Data


To help understand polling place access by university demographics, we require polling place (Election Day and Early Voting) location, college demographics and college campus boundary data. Additionally in an attempt to ensure the integrity of our analysis we needed to make sure that said data was comprehensive and properly sourced. The “Students Learn, Students Vote Coalition” was generous enough to provide us with a comprehensive list of universities with a number of interesting demographics(word choice?) such as College name, city and state, Minority Serving Institution Type, percent of population by race, and institution type (public vs private / 2 yr vs 4yr). The data provided was already in a clean, structured csv format, so no data manipulation was needed. 


## Campus Boundary Data


We also needed the location data of US College campuses in the form of a point, polygon or multipolygon.  A point representation would be a single longitude latitude value present anywhere on the campus and a polygon representation depicts the boundaries of the college campus. When a campus is spread out in many places, multi-polygons are used to connect multiple polygons as one university. The illustration below depicts the differentiation between a point polygon and multipolygon.

![polygon_example](content/post/22-01-20-methodology/polygon_example.png)

We were able to source college and university geospatial data by accessing data provided by the Homeland Infrastructure Foundation-Level Data (HIFLD - ARCGIS). ARCGIS is a geographical information system (GIS) software that allows handling and analyzing geographic information by visualizing geographical statistics through layer building maps like climate data or trade flows. This dataset is composed of all Post Secondary Education facilities as defined by the Integrated PostSecondary Education System (IPEDS, http://nces.ed.gov/ipeds/), National Center for Education Statistics (NCES, https://nces.ed.gov/), US Department of Education for the 2018-2019 school year.1  The HIFLD data was stored in a shape file consisting of the college name and it’s respective coordinates. There were instances in which specific departments were designated as individual colleges. For example, along with Duke University, Fuqua School of Business, Sanford School of Public Policy, University Apartments were also included as separate colleges. To remedy this we removed said entries from the data utilizing string matching techniques.


## Merging Data


We utilized a two step merge to merge the college campus data with the college demographics data. We used the college demographic dataset as our gro und truth data for merging because it included the university demographic data. The first step of our join was to join the data based on college name and state. Because there were several instances of the same college being spelled differently in the two datasets, we manually went through the colleges that were not included in the merge to check whether or not existed in both datasets, ultimately including them in the final merged dataset.


## Polling Place Data


Our polling place data comes from a couple different sources. The first of which was the Center for Public Integrity which provided polling place for for 35 states in 2020. Our second data source was Ballot Ready, which provided the 2020 Early Voting data for 45 states.


## Distance Calculations


We used a point-to-polygon calculation in geopandas to measure the distance between campuses and their nearest polling place. If a polling place was within a campus’ polygon boundary, it was labeled on campus and given a distance of zero. If a polling place was outside of a campus’ polygon boundary, it was considered off-campus and given the distance to the nearest edge.
We used the Google Maps API to calculate the distance (point to point) and duration between the polling place and the college campus centroid through various means of transportation, such as driving, walking, and public transit. This information is used to better understand the travel times, and by proxy access levels, students have by travel modalities from the centroid of the polygon to the polling place. 


### Footnotes: 
[1] - https://hifld-geoplatform.opendata.arcgis.com/datasets/geoplatform::colleges-and-universities/about

