---
title: "Further data collection (satellite imagery)"
date: 2024-01-09
slug: "data-2"
description: "Data for developing the model"
keywords: ["project", "data", "TensorFlow", "machine learning"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

The main chunk of work for second term will involve incorporating satellite imagery into the foundational model to allow it to react to cloud coverage.

## 9/1/24

### Action points

* Over Christmas break I worked on accessing the data for satellite imagery
  * This involved numerous calls and e-mails due to the lack of up-to-date documentation

### TODO

* My term 2 gantt chart is still relevant, but I will be ahead of schedule as I did not assume that I would get the satellite imagery over Christmas
* I'll have more time to improve the foundational model, as well as study different ways in collecting the data for the foundational model
* I'll also have more time to develop the model with satellite imagery
* If I remain ahead of schedule I will then look into further improvements to the model
  * Looking at data regarding aerosols
  * Not dealing with live data since this will be a major challenge with little reward for the report
  
## 10/1/24

### Action points

* Deciding which satellite datasets to use from the ones I have available
* Working on finely selecting the data to access around the key GSP I'm considering
* E-mailed for support
* Made a kanban board for coding tasks to keep better track of them

## 11/1/24

### Action points

* Refined selection of data
* Collecting multiple time samples at once
* Started looking into the model to use for just satellite -> pv

### TODO

* Model is giving nan which likely means there is missing/erroneous data
  * More pre-processing required

## 12/1/24

### Action points

* Additional pre-processing
* Model runs
* Actual output vs prediction (Model results 1) depicts:
  * Predicts generation during nightime since the clouds are necessarily different, but the model is unaware of the sun
    * We can deal with this for this model, but if we merge with NWP that will be handled automatically
  * The predictions are massively off, but there is a general correspondance for each day which is promising
* Using less data seems to get the near 0 pv generation (Model results 2)
  * There may just be an issue with some of the data and how it matches up to the pv generation data

* Remember that I may end up overfitting because I haven't split the dataset into training/validation
  * For now just trying to find problematic data

* This is what problematic data does, typically we have 30 min intervals, but if there is missing data then we will skip further ahead
* This shifts the entire timings so that all future data doesn't temporally match

```
2018-03-05T19:00:00.000000000
2018-03-05T19:30:00.000000000
2018-03-05T20:10:00.000000000
2018-03-05T20:40:00.000000000
2018-03-05T21:10:00.000000000
2018-03-05T21:40:00.000000000
```

* Removed times that don't have '00' or '30' in the minutes column

### TODO

* Continue working on pre-processing
* Will have to check in the end that all the data is temporally matching

## 16/1/24

### Action points

* 5000 images - model seems fine except around july where additional small peaks are predicted between two consecutive days
* 7500 images - prediction is shifted up by ~5MW
* 6000 images - still shifted
* 5500 images - prediction is shifted down by ~2MW
* 5250 images - prediction is shifted up by ~1.8MW
* 5100 images - model fine, peaks start from around sample 3900
* 5175 images - prediction is shifted down minimally
* 5150 images - prediction shifted up
* 5125 images - prediction shifted up
* 5110 images - prediction shifted up
* 5105 images - tiny shift
* 5103 images - large shift ~ 4.5MW
* 5012 images - shift below
* 5011 images - large shift ~ 4.5MW
* Nevermind 5100 is shifted

4000 fine, 4500 bad, 4250 fine, 4400 small shift

* There doesn't seem to be an issue with the satellite data in terms of the timing, so it may be the content of the image, or from the gsp data?

## 17/1/24

* Still trying to determine where the prediction shift originates from
  * There aren't NaN values in the satellites data
  * Since some of the times were missing, data is chosen at 30min intervals, but there's a chance that 30 min interval could be missing
    * This appears to be the issue since although asking for x images provides x images, the timing of the last image is further ahead than it's supposed to be