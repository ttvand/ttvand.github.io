---
layout: post
title: "Facebook V: Predicting Check Ins"
subtitle:   "I won the Kaggle competition!"
date:       2016-07-09 00:00:00
author:     "Tom Van de Wiele"
header-img: "img/FBCheckinDark.jpg"
comments: true
---



The [Facebook V: Predicting Check Ins data science competition](https://www.kaggle.com/c/facebook-v-predicting-check-ins) where the goal was to predict which place a person would like to check in to has just ended. I participated with the goal of learning as much as possible and maybe aim for a top 10% since this was my first serious Kaggle competition attempt. I managed to exceed all expections and finish 1st out of 1212 participants! In this post, I’ll explain my approach.

## Overview

This blog post will cover all sections to go from the raw data to the winning submission. Here's an overview of the different sections. If you want to skip ahead, just click the section title to go there.

* *[Introduction](#introduction)*
* *[Exploratory analysis](#explorAnalysis)*
* *[Problem definition](#probDef)*
* *[Strategy](#strategy)*
* *[Candidate selection 1](#candidateSel1)*
* *[Feature engineering](#featEng)*
* *[Candidate selection 2](#candidateSel2)*
* *[First level learners](#firstLL)*
* *[Second level learners](#secondLL)*
* *[Conclusion](#conclusion)*

## <a name="introduction"><a> Introduction

{% include image.html url="/img/FB5_banner.png" description="Competition banner" %}

From the competition page: The goal of this competition is to predict which place a person would like to check in to. For the purposes of this competition, Facebook created an artificial world consisting of more than 100,000 places located in a 10 km by 10 km square. For a given set of coordinates, your task is to return a ranked list of the most likely places. Data was fabricated to resemble location signals coming from mobile devices, giving you a flavor of what it takes to work with real data complicated by inaccurate and noisy values. Inconsistent and erroneous location data can disrupt experience for services like Facebook Check In.

The training data consists of approximately 29 million observations where the location (x,y), accuracy, and timestamp is given along with the target variable, the check in location. The test data contains 8.6 million observations where the check in location should be predicted based on the location, accuracy and timestamp. The train and test data set are split based on time. There is no concept of a person in this dataset. All the observations are events, not people. 

A ranked list of the top three most likely places is expected for all test records. The leaderboard score is calculated using the MAP@3 criterion. Consequently, ranking the actual place as the most likely candidate gets a score of 1, ranking the actual place as the second most likely gets a score of 1/2 and a third rank of the actual place results in a score of 1/3. If the actual place is not in the top three of ranked places, a score of 0 is awarded for that record. The total score is the mean of the observation scores.

{% include image.html url="/img/kaggle_screenshot-min.png" description="Check Ins where each place has a different color" %}

## <a name="explorAnalysis"><a> Exploratory analysis



## <a name="probDef"><a> Problem definition

## <a name="strategy"><a> Strategy

## <a name="candidateSel1"><a> Candidate selection 1

## <a name="featEng"><a> Feature engineering

### Location

### Time

### Accuracy

### Most important features

{% include image.html url="/img/meanXVariationVsAc.png" description="Mean variation from the median in x versus time and accuracy" %}

## <a name="candidateSel2"><a> Candidate selection 2

## <a name="firstLL"><a> First level learners

## <a name="secondLL"><a> Second level learners

## <a name="conclusion"><a> Conclusion
Ideas to improve approach & LB plot

{% include image.html url="/img/PrivateLB.png" description="Private leaderboard score (mean MAP@3) - two teams stand out from the pack" %}
