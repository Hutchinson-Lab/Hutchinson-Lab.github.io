---
title: 'How to Evaluate ML Models in Geospatial Settings?'
date: 2023-05-25
permalink: /posts/2023/05/spatial_cross_validation/
tags:
  - spatial autocorrelation
  - covariate shift
  - geospatial
  - cross-validation
---
Standard K-fold Cross-validation (KFCV) randomly divides a training set into K non-overlapping folds and iteratively holds out one fold at a time, training a model on the remainder (i.e., training folds) and measuring error on the held-out fold (i.e., validation fold). The average of these model errors across folds is the estimate of generalization performance for an unseen test set.
KFCV provides unbiased performance estimates when applied to independent, identically distributed (iid) data. But does it also work well on geospatial data?

Geospatial learning problems are frequently characterized by spatial autocorrelation in the input features and the potential for covariate shift at test time. These realities violate the iid assumption. **Spatial autocorrelation** is a key concept in geostatistics which describes the spatial variation of a feature. In most cases, spatial autocorrelation is positive as nearby areas or sites tend to have similar feature values. For example, the precipitation of your city is more similar to that of the neighboring city than that of another city far away. You may check the [Chapter 13 of Intro to GIS and Spatial Analysis](https://mgimond.github.io/Spatial/spatial-autocorrelation.html) for more detailed intro. **Covariate shift** is one kind of data shift that may occur in geospatial analyses. Data shift refers to any circumstance in which the joint distribution of features and responses differs between the training and test sets.
Covariate shift arises when feature distribution of the training set is different from that of the test set, while the functional relationship remains the same. 

Previously, a few cross-validation procedures have been developed to address the two above properties. Blocking cross-validation (BLCV) groups geographically proximal points into the same block. Buffered cross-validation (BFCV) places a buffer between training and validation folds and removes points within the buffer so that they are used in neither set. These spatial strategies have been implemented in some software packages, e.g., sperrorest, ENMeval, and BLOCKCV. CV methods to account for covariate shift have been developed in non-spatial settings, most notably including importance-weighted cross-validation (IWCV), which rectifies the loss function with the ratio between the test and training probability densities. 

As a motivating example, when considering whether to translocate a threatened species to a currently unoccupied region, natural resource managers may need to construct a species distribution model (SDM) with data from a currently occupied region and predict habitat suitability in the new area.

To be continued...

### References
1. Roberts, David R., et al. "Cross‐validation strategies for data with temporal, spatial, hierarchical, or phylogenetic structure." Ecography 40.8 (2017): 913-929.
2. Pohjankukka, Jonne, et al. "Estimating the prediction performance of spatial models via spatial k-fold cross validation." International Journal of Geographical Information Science 31.10 (2017): 2001-2019.
3. Sugiyama, Masashi, Matthias Krauledat, and Klaus-Robert Müller. "Covariate shift adaptation by importance weighted cross validation." Journal of Machine Learning Research 8.5 (2007).
