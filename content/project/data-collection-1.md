---
title: "Initial data collection"
date: 2023-10-16
slug: "data-1"
description: "Initial data collection for 1 GSP point"
keywords: ["project", "data", "CSV"]
draft: false
tags: ["project"]
math: false
toc: true
---

## Motivation

This is the first task outlined on the Gantt chart following the project specification submission.

## 16/10/23

### Action points

* Selected GSP ABHA1, Latitude: 50.47108, Longitude: -3.72977, PVLive ID: 1
* Collected NSRDB data with corresponding coordinates, temporal accuracy of 30 minutes
* Excel data actually says 50.49, -3.74
* NSRDB data will be split 80:10:10 for training, validating, and testing
  * 17520 total data entries
  * 14,016 for training
  * 1,752 each for validating and testing
  * Dates are: 2019-01-01 00:00 to 2019-12-31 @ 23:30 
* Getting PV data for corresponding GSP between date and time specified
  * The time ordering of the data is inconsistent, but can be sorted with a simple A to Z Excel sort

### TODO

* Get tensorflow locally
* See how to provide test data to tensorflow model, and how to add labels
  * Do I want the NSRDB and PVLive in separate or combined CSV?

## 17/10/23

### Action points

* Downloaded tensorflow locally
  * Will my laptop be powerful enough? Should I use DCS instead? I never mentioned this in my specification but it was considered. I don't think for the foundational model it will be required but once Satellite imagery is involved it may become more intensive
* The data can stay in separate files then be accessed separately within the code
  * It can then be merged easily in code if necessary
* Data formatted and is accessible from the code for use in the model
* Trying to find how to provide the test data which is leading to looking into ANNs
  * Perceptron models -> receive multiple numerical inputs to produce a single numerical output (output is binary)

## 18/10/23

### Action points

* Since I've got all the data I'm slightly ahead of schedule and am looking into models and trying to build the foundational model
* First attempt with mean squared error loss of nan, accuracy increases slightly once then maintains constant value so errors
  * Since I had to gather sections of the data in the code, the ordering is different again so times aren't matching up
* Sort data within the code so its always consistent
  * Data now looks correct and is temporally matching
* Same issue with the model itself

### TODO

* Potentially normalise the values for input and labels
  * NSRDB currently has large ranges and for each measurement the range is different
  * PVLive has large range between ~50 to e-5
* Which activation function should I be using, since sigmoid is for classification so that doesn't apply here
  * I think it is linear

## 19/10/23

* Attended webinar
  * Not too applicable, interesting to hear about how the system works in California
* Normalised the data
  * Still not improved the model
* I think the abundance of all zero rows is causing issues
  * I need to remove all 0s rows between both NSRDB and PVLive

### TODO

* Correct bug for removing 0 rows