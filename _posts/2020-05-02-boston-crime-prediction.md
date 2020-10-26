---
layout: post
title: "Boston crime prediction"
date: 2020-05-02 22:00:00
categories: jekyll update
---

I have done an analysis on the boston crime data and it can be found here. My current goal is to predict the number of crimes in Boston. I use the same crime data in the previous analysis. 

## Analysis 

I used the offense group from data as the main parameter in prediction. I used the spatial file of Boston neighborhoods and zipcodes in Boston city as the other data. I can find the number of crimes in Boston based on offense group. After this I need to convert that into a matrix format. I used 100 X 132 matrix of latitude X longitude. Here I used equal bins of latitude and logitude. This means that if the bins of latitude is 1 deg and I use 1 deg for longitude. I used geopandas to check whether the Boston neighborhoods spatial file has these coordinates and creates a mask of Boston. Many of the elements from generated array are empty and I use an Inverse Distance Weighting algorithm to fill the empty elements. I will be working on the area of zipcode area for this prediction. It is also possible to work for an address area in the same way as zipcode area. Fortunately, the number of crime is so small in different addresses. After that I look for the array points in the zipcode region based on spatial file and reinserted the interpolated values.

I use five years of crime data in a zipcode area and I find the sum of crimes per year. I use a linear regression of the sum of crimes within the zipcode region to check dependence of the year. I use this regression to predict crimes in the zipcode region. If I have no trend in the sum of crimes per year then I use the average of sum for the prediction. There are a few instances where there is no data and I ignore that. If there is a trend for the rest of the sum I use that for the prediction and if this not then I use the average of the sum of crimes per year.

    




