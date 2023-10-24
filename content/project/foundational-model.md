---
title: "Foundational model"
date: 2023-10-23
slug: "model-1"
description: "Initial model"
keywords: ["project", "data", "TensorFlow", "machine learning"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

This is the second task outlined in the Gantt chart. Some preliminary work has already been completed regarding the model within the data collection portion, but now the entire focus is getting it working as the data (to my knowledge) is fully preprocessed.

## 20/10/23

### Action points

* Indexing of PVLive zero rows was wrong
  * This was due to sorting by ascending order
  * But it needs to be sorted so the data sets match temporally
  * ignore_index = true seems to fix this
* Not working
  * Could be useful: https://www.tensorflow.org/tutorials/keras/regression

### TODO

* Replace my data with Auto MPG

## 23/10/23

### Action points

* Worked with MPG data which didn't cause issues
* By changing which inputs I use on NSRDB for small sample (100) it either gives NaNs or doesn't
  * Surface albedo causes Nan in losses, likely because it's always nearly the same value
  * Temp, solar zenith, ghi, dni, dhi works on 100, 1000, 5000, 8000, 8700, 8705
    * Increasing sample reduced loss but also reduced accuracy
    * PVLive has "" at row 8707
* With 80% data, loss, mae, mse decreases (already small), accuracy constant (0.4428)
* Adding more NSRDB data has negligible effect
* Can make predictions, validation data accuracy 44%

### TODO

* Accuracy far too low, looking for at least 70% for specification goal
* For a day/few days plot actual pv generation at the gsp to predictions
  * Although it's not good enough yet, sorting the code for plotting will be useful

## 24/10/23

### Action points

* Accuracy is not a correct measure for regression problems
  * "In regression you are estimating the parameters. So, you can’t say an estimated value completely wrong or completely right, but you can only say how far or near it is to the original value"  
  * mean squared, root mean squared, mean absolute

### TODO

* Report errors
  * Should do it in plot form