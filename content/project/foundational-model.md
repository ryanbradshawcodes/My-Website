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

## 25/10/23

### Action points

* Plotting a single day of actual generation data
* Making predictions (currently on training data not validation)
  * Reverted normalisation
  * Some values negative
  * Predictions don't appear close: loss = 0.0050, mae = 0.0430, mse = 0.0050 (but this is on normalised data)

### TODO

* Check I understand how the predict method works
* Check I am reverting normalisation properly for the predictions
* Plot a single day of predictions vs the actual plot

## 1/11/23

### Action points

* Plotted normalised predicted vs actual data over 2 days
  * Fair correlation in the plots

## 2/11/23

### Action points

* Looking into GSP region ABHA1
  * I'm using the central location and correlating that to NWP data at the same longitude and latitude currently
  * ABHA1 = Abham
  * Report in notes to show at meeting
  * I've emailed Jamie Taylor from PV_Live for more information

* I think using the central location was fine for prototyping but my strategy will need to change

## 3/11/23

### Action points

* Clean up code
* Re plotting
* Working on using historical data more effectively

## 5/11/23

### Action points

* Clean up code
* Produced another PV actual v predicted plot
  * [Normalised-actual-vs-predicted-2.png](https://postimg.cc/Cz6C6XYD)

### TODO

* Undo normalisation for plot
* Use more rows of data for each PV output

## 6/11/23

### Action points

* New file which is more organised
* Working on using historical data & predicting ahead
* Researching time series prediction

### TODO

* Continue with this work

## 7/11/23

### Action points

* Continuing with research and upgrading model
* Plotted NWP inputs in training set
  * [nwp-data.png](https://postimg.cc/tsc7XSRn)

### TODO

* Fix array sizing mismatch
* Couple of changes to make use of multiple rows of past data

## 8/11/23

### Action points

* Fixed bugs
* Using past data
* Good loss curve, but bad predictions

### TODO

* Clean up function arguments
  * Calling a lot of times unnecessarily
* Debug the bad prediction despite good loss

## 9/11/23

### Action points

* Bugs fixed
* Predictions good, but graph shape not as expected

### TODO

* Get the normal bell curve shape back
  * Then can start evaluating use of multiple past data rows
  * As well as how far we can predict ahead

## 22/11/23

Significant coursework workloads and personal circumstances have delayed progress. 
Research has been completed outside of what has been mentioned in the blog, as well as additional
meetings with companies.

## 24/11/23

### Action points

* Producing epoch loss graphs
* Increasing size of input data

## 27/11/23

### Action points

* Validating plots

## 28/11/23

### Action points

* Formulating progress report

## 29/11/23

### Action points

* Reporting error evolution
* Formulating progress report

## 30/11/23

### Action points

* Script for improved coverage of error evolution

### TODO

* Distinction between NSRDB data and standard NWP data
* Look into Renewables Ninja
* Look into Solar forecast arbiter
* Normalise PV generation values using the capacity of the gsp region (PV yield)
  * This is to deal with a region installing new capacity and having a spike in pv generation
* Discuss ideas of collecting more data points within a gsp, as well as neighbouring gsps
  * Potential research focused direction
* Attempt to collect normal NWP data from ECMWF
  * Validate trained model on ECMWF data

## 4/12/23

### Action points

* Focused on producing the final report from 30/11/23
* Called John Walsh (head of forecasting and ESO) to discuss use of forecasting tools

## 5/12/23

### Action points

* Called Jamie Taylor (Sheffield Solar) to further discuss my project and areas to look at

## 6/12/23

### Action points

* Using PV yield for training to account for GSP capacity

