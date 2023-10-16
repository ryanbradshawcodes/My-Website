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

# Motivation

This is the first task outlined on the Gantt chart following the project specification submission.

# 16/10/23

## Action points

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

## TODO

* Get tensorflow locally
* See how to provide test data to tensorflow model, and how to add labels
  * Do I want the NSRDB and PVLive in separate or combined CSV?