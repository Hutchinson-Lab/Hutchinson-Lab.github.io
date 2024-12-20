---
title: 'How to Evaluate ML Models in Geospatial Settings?'
date: 2023-09-18
permalink: /posts/2023/09/spatial_cross_validation/
tags:
  - spatial autocorrelation
  - covariate shift
  - geospatial
  - cross-validation
---

Standard K-fold Cross-validation (KFCV) randomly divides a training set into K non-overlapping folds and iteratively holds out one fold at a time, training a model on the remainder (i.e., training folds) and measuring error on the held-out fold (i.e., validation fold). The average of these model errors across folds is the estimate of generalization performance for an unseen test set.
KFCV provides unbiased performance estimates when applied to independent, identically distributed (iid) data. But does it also work well on geospatial data?

## Background
Geospatial learning problems are frequently characterized by spatial autocorrelation in the input features and the potential for covariate shift at test time. These realities violate the iid assumption. **Spatial autocorrelation** is a key concept in geostatistics which describes the spatial variation of a feature. In most cases, spatial autocorrelation is positive as nearby areas or sites tend to have similar feature values. For example, the precipitation of your city is more similar to that of the neighboring city than that of another city far away. **Covariate shift** is one kind of data shift that may occur in geospatial analyses. Data shift refers to any circumstance in which the joint distribution of features and responses differs between the training and test sets.
Covariate shift arises when feature distribution of the training set is different from that of the test set, while the functional relationship of the features to the response variable remains the same. 

A few cross-validation procedures have been developed to address the two above properties (Figure 1). Blocking cross-validation (BLCV) groups geographically proximal points into the same block. Buffered cross-validation (BFCV) places a buffer between training and validation folds and removes points within the buffer so that they are used in neither set. These spatial strategies have been implemented in some software packages, e.g., [sperrorest](https://cran.r-project.org/web/packages/sperrorest/), [ENMeval](https://cran.r-project.org/web/packages/ENMeval/), and [blockCV](https://cran.r-project.org/web/packages/blockCV/). 
Cross-validation methods to account for covariate shift have been developed in non-spatial settings, most notably including importance-weighted cross-validation ([IWCV](https://www.statistik.tu-dortmund.de/~wornowiz/RuLSIF.txt)), which rectifies the loss function with the ratio between the test and training probability densities. 
<p align="center">
<img width="720" alt="image" src="https://github.com/Hutchinson-Lab/Hutchinson-Lab.github.io/assets/17716760/7d7a9d7e-8200-45bf-b832-aa8396831d5b">
</p>
<p align="center">
Figure 1: Visualization of how different CV methods split 1800 training data points on a $60 \times 60$ landscape into 9 folds. (a) Each color represents a fold. (b) An example of BLCV with block size of 12 grid cells, each assigned to one of the 9 folds. (c) BFCV was based on a grid with 20x20 cells. This example shows fold 5 as the validation fold, where training samples in its surrounding buffer region (buffer size = 12) have been removed.
</p>

## Motivating Example
Do these exsiting CV methods provide accurate predictions on model errors? As a motivating example, when considering whether to translocate a threatened species to a currently unoccupied region, natural resource managers may need to construct a species distribution model (SDM) with data from a currently occupied region and predict habitat suitability in the new area.
Here we use data from Amundson, et. al. (2018) to demonstrate the performance of various CV methods in model evaluation. 

This dataset records observations of 15 bird species across three national parks in Alaska, USA. These surveys were conducted during May-June, 2004-2006 in Katmai National Park and Preserve (KATM) and Lake Clark National Park and Preserve (LACL), and during May-June, 2008 in Aniakchak National Monument and Preserve (ANIA). The features are the proportions of eight land cover classes around each survey site. We set up a balanced binary classification task for the most prevalent bird species, Golden-crowned Sparrow (GCSP). The training set consists of 710 data points (355 non-detections and 355 detections) collected from KATM and LACL in 2004-2006. The test set contains 134 data points (67 non-detections and 67 detections) collected from ANIA in 2008 (Figure 2). 

<p align="center">
  <img src="https://github.com/Hutchinson-Lab/Hutchinson-Lab.github.io/assets/17716760/422cc66d-d90e-4eec-a9f7-8826049ce8eb" width="50%" height="50%" />
</p>
<p align = "center">
Figure 2: Alaskan Birds Dataset. Training samples are from Katmai and Lake Clark, and test samples are from Aniakchak. Each red point represents a data point.
</p>

We explored five classification models: Ridge classifier (Ridge), Linear SVM (LSVM), K-Nearest Neighbors (KNN), Random Forest (RF), and Naive Bayes (NB),
and compared their test errors with the CV error estimates (Table 1).

<p align="center">Table 1: Test errors (targets) and 10-fold CV estimates (best estimates in each column in bold).</p>

| Model        | Test error    | KFCV   | IWCV       | BLCV       | BFCV       |
|--------------|-----------|------------|------------|------------|------------|
| Ridge         | 0.1866   | 0.3225       |**0.2526**    |0.2777      |0.2881      |
| LSVM         | 0.1866    | 0.3211       |**0.2488**     |0.2723    |0.2791        |
| KNN            | 0.3284  | 0.3113       |0.2780      |0.3030    |**0.3251**    |
| RF             | 0.3657  | 0.3169       |0.2911      |0.3176    |**0.3343**   |
| NB             | 0.4030  | 0.3521       |0.2938    |0.3230      |**0.3597**     |

In this scenario where the features are spatially autocorrelated and the training and test distributions present covariate shift, we found that KFCV never produced the closest model error estimates. The non-standard CV methods generated the best estimates but still show room for improvement. We wondered: How can we get generalization performances estimates from cross-validation that match the true test errors more closely? When is it appropriate to use these different types of cross-validation?

## Our work
To address these questions, our paper _"[Cross-validation for Geospatial Data: Estimating Generalization Performance in Geostatistical Problems](https://openreview.net/forum?id=VgJhYu7FmQ&referrer=%5BTMLR%5D(%2Fgroup%3Fid%3DTMLR))"_ created a framework to sort geospatial datasets into four scenarios that would help practitioners choose an appropriate CV approach. We demonstrated them with experiments on simulated and real datasets. We also developed a new CV algorithm called Importance Weighted Buffered Cross-Validation (IBCV) which makes improvements sometimes on the existing CV methods when training and test sets present spatial independence and covariate shift at the same time (Table 2). Theoretically，we proved a criterion for unbiased cross-validation and the unbiasedness of IBCV.

<p align="center">Table 2: Test errors (targets) and 10-fold CV estimates (best estimates in each column in bold). </p>

| Model        | Test error    | KFCV   | IWCV       | BLCV       | BFCV       | IBCV |
|--------------|-----------|------------|------------|------------|------------| ------------|
| Ridge         | 0.1866   | 0.3225       |0.2526   |0.2777      |0.2881      | **0.2374**|
| LSVM         | 0.1866    | 0.3211       |0.2488    |0.2723    |0.2791        |**0.2244**|
| KNN            | 0.3284  | 0.3113       |0.2780      |0.3030    |**0.3251**    |0.2542|
| RF             | 0.3657  | 0.3169       |0.2911      |0.3176    |**0.3343**   |0.2998|
| NB             | 0.4030  | 0.3521       |0.2938    |0.3230      |**0.3597**     |0.2748|


### References
1. Roberts, David R., et al. "Cross‐validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure." Ecography 40.8 (2017): 913-929.
2. Pohjankukka, Jonne, et al. "Estimating the prediction performance of spatial models via spatial k-fold cross validation." International Journal of Geographical Information Science 31.10 (2017): 2001-2019.
3. Sugiyama, Masashi, Matthias Krauledat, and Klaus-Robert Müller. "Covariate shift adaptation by importance weighted cross validation." Journal of Machine Learning Research 8.5 (2007).
4. Amundson, Courtney L., et al. "Montane-breeding bird distribution and abundance across national parks of southwestern Alaska." Journal of Fish and Wildlife Management 9.1 (2018): 180-207.

