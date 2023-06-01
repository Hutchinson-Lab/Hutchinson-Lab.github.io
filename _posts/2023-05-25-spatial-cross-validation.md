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
K-fold Cross-validation (KFCV) randomly divides a training set into K non-overlapping folds and iteratively holds out one fold at a time, training a model on the remainder (i.e., training folds) and measuring error on the held-out fold (i.e., validation fold). The average of these model errors across folds is the estimate of generalization performance on a test set.
KFCV provides unbiased estimates with independent, identically distributed (iid) data. But does it also work well on geospatial data?

Geospatial learning problems are frequently characterized by spatial autocorrelation in the input features and the potential for covariate shift at test time. These realities violate the iid assumption. Now let us explain the two concepts a little bit.

- **Spatial autocorrelation**

It is key concept in geostatistics which describes the spatial variation of a feature. In most cases, spatial autocorrelation is positive as nearby areas or sites tend to have similar feature values. For example, the precipitation of your city is more similar to that of the neighboring city than that of another city far away. You may check the [Chapter 13 of Intro to GIS and Spatial Analysis](https://mgimond.github.io/Spatial/spatial-autocorrelation.html) for more detailed intro.

- **Covariate shift**

Geospatial problems may exhibit different kinds of data shift, which happens when the joint distribution of features and responses differs between the training and test sets.
Covariate shift arises frequently when feature distribution of training set is different from that of test set while the functional relationship keeps the same. For example, when considering whether to translocate a threatened species to a currently unoccupied region, natural resource managers may need to construct an Species Distribution Model with data from a currently occupied region and predict habitat suitability in the new area.

To be continued...
