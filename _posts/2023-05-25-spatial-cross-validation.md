---
title: 'How to Evaluate ML Models with Geospatial Data?'
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

Geospatial learning problems are frequently characterized by spatial autocorrelation in the input features and the potential for covariate shift at test time. These realities violate the iid assumption.

To be continued...
