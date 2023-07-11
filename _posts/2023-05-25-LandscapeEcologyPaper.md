---
title: 'A comparison of remotely sensed environmental predictors for SDMs'
date: 2023-05-25
permalink: /posts/2023/05/A_comparison_of_remotely_sensed_environmetnal_predictors_for_SDMs/
tags:
  - species distribution models
  - remote sensing
  - landsat
  - Oregon 2020 Project
---

There are many options when establishing environmntal variables from satellite imagery. First, one must select a satellite dataset (e.g., Landsat, MODIS, Sentinel). Second is deciding how to summarize the data (i.e., how to turn satellite imagery into input feature vectors). With the many satellite imagery datasets and methods of summarization, there is an open question of which environmental variables are best suited for SDMs. To help address this question, we compared the predictive power of several sets of environmental variables derived from Landsat satellite imagery in predicting 13 bird species across the state of Oregon, USA. This work was done in collaboration with the Oregon 2020 project and was published in Landscape Ecology. 

Landsat imagery is a common source for environmental variables in species modeling. When working with Landsat (or any satellite) data, it is common to work with the
multispectral bands, transformations of the bands, or indices derived from the bands. Indices are single bands that are computed from different subsets of the multispectral bands and represent physical attributes of the landscape. The raw bands, Tasseled Cap transformation of the bands, and indices have all been shown to predict species well. It is common to establish environmental variables from satellite imagery for SDMs by computing statistics over specified areas centered at species sightings. This can be done for a single scale (e.g., 50m radius) or multiple scales (e.g., 50m, 100m, & 500m radii). Similarly, calculations can be made for a single season (e.g., the breeding season) or over multiple seasons. In addition to quantifying temporal variation, spatial variation in the landscape can be characterized through texture metrics, such as the gray level cooccurrence matrix (GLCM) analysis.

# Experimental Design
Our primary objectives were to:
1. Identify which of the spectral bands, indices, or transformations of spectral bands consistently informed the highest performing SDMs across several bird species
2. Examine whether data from additional seasons improved SDM performance
3. Explore whether standard deviations or textural metrics improved SDM performance
To answer the above questions, we modeled 13 bird species in the state of Oregon with different spectral predictor sets (i.e., sets of input variables).

## Spectral predictor sets
We developed spectral predictor sets based on previous SDM studies. Specifically, we analyzed the multispectral bands, their associated Tasseled Cap transformations, and eight single-valued indices derived from the spectral bands: NDMI, NDVI, NBR, NBR2, EVI, SAVI, MSAVI, NDSI. We computed the mean,
standard deviation, and GLCM texture metrics at three buffered radii (75, 600, and 2400m) centered at the count location for spring, summer, and fall imagery. We selected buffer sizes that have been previously shown to predict songbirds well in the state of Oregon.

## Species data
We used bird surveys obtained through the Oregon 2020 project. To cover a range of habitats, we selected thirteen species which are known to occur in six common habitats in Oregon and selected generalists and specialists within each habitat.

## Model setup
Predicting species occurrences (detections vs. non-detections) can be considered a binary classification problem. Following Johnston et al. (2021), we used
balanced random forests to address the high class imbalance (significantly more nondetections). Balanced random forests sample the same number of detections and nondetections in each bootstrapped sample drawn for each tree.

## Evaluation
We split data into spatial blocks to address potential spatial autocorrelation and then evaluated models with 10-fold cross validation. We used area under the receiver operating characteristic curve (AUC) and computed 95% DeLong confidence intervals to evaluate model performance.

# Results
**Objective 1: Raw bands consistently informed the highest performing SDMs.**

**Objective 2: Additional seasonal information did not improve SDM performance for the raw bands.**

**Objective 3: Including standard deviations or texture metrics did not improve SDM performance for the raw bands.**
