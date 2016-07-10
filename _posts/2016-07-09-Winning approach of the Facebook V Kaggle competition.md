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

The source code is available on [GitHub](https://github.com/ttvand/Facebook-V/). This [thread](https://www.kaggle.com/c/facebook-v-predicting-check-ins/forums/t/22081/1st-place-winning-solution) on the Kaggle forum discusses the solution on a higher level and is a good place to start if you participated in the challenge.

## <a name="introduction"><a> Introduction

{% include image.html url="/img/FB5_banner.png" description="Competition banner" %}

From the competition page: The goal of this competition is to predict which place a person would like to check in to. For the purposes of this competition, Facebook created an artificial world consisting of more than 100,000 places located in a 10 km by 10 km square. For a given set of coordinates, your task is to return a ranked list of the most likely places. Data was fabricated to resemble location signals coming from mobile devices, giving you a flavor of what it takes to work with real data complicated by inaccurate and noisy values. Inconsistent and erroneous location data can disrupt experience for services like Facebook Check In.

The training data consists of approximately 29 million observations where the location (x,y), accuracy, and timestamp is given along with the target variable, the check in location. The test data contains 8.6 million observations where the check in location should be predicted based on the location, accuracy and timestamp. The train and test data set are split based on time. There is no concept of a person in this dataset. All the observations are events, not people. 

A ranked list of the top three most likely places is expected for all test records. The leaderboard score is calculated using the MAP@3 criterion. Consequently, ranking the actual place as the most likely candidate gets a score of 1, ranking the actual place as the second most likely gets a score of 1/2 and a third rank of the actual place results in a score of 1/3. If the actual place is not in the top three of ranked places, a score of 0 is awarded for that record. The total score is the mean of the observation scores.

{% include image.html url="/img/kaggle_screenshot-min.png" description="Check Ins where each place has a different color" %}

## <a name="explorAnalysis"><a> Exploratory analysis

Location analysis of the train check ins revealed interesting patterns between the variation in x and y. There appears to be way more variation in x than in y. It was suggested that this could be related to the streets of the simulated world. The difference in variation between x and y is however different for all places and there is no obvious spatial (x-y) pattern in this relationship.

It was quickly established by the community that time is measured in minutes and could thus be converted to relative hours and days of the week. This means that the train data covers 546 days and the test data spans 153 days. All places seem to live in independent time zones with clear hourly and daily patterns. No spatial pattern was found with respect to the time patterns.

Accuracy was by far the hardest feature to tackle. It was expected that it would be clearly correlated with the variation in x and y but the pattern is not as obvious. Halfway through the competition I cracked the code and the details will be discussed in the [Feature engineering](#featEng) section.

I wrote an [interactive Shiny application](https://tvdwiele.shinyapps.io/Facebook-V/) to research these interactions for a subset of the places. Feel free to explore the data yourself!

## <a name="probDef"><a> Problem definition


## <a name="strategy"><a> Strategy

{% include image.html url="/img/Strategy4.png" description="High level strategy" %}

## <a name="candidateSel1"><a> Candidate selection 1

## <a name="featEng"><a> Feature engineering

### Location

### Time

### Accuracy

### Z-scores

### Most important features

{% include image.html url="/img/meanXVariationVsAc.png" description="Mean variation from the median in x versus time and accuracy" %}

## <a name="candidateSel2"><a> Candidate selection 2

## <a name="firstLL"><a> First level learners

## <a name="secondLL"><a> Second level learners

## <a name="conclusion"><a> Conclusion
The private leaderboard standing below, used to rank the teams, shows the top 30 teams. It was a very close competition in the end and Markus would have been a well-deserved winner as well. We were very close to each other ever since the third week of the eight week contest and pushed each other forward. The fact that the test data contains 8.6 million records and that it was split randomly for the private and public leaderboard resulted in a very confident estimate of the private standing given the public leaderboard. I was impressed by the approaches of Markus and Jack (Japan) who finished in third position. You can read more about their approaches on the [forum](https://www.kaggle.com/c/facebook-v-predicting-check-ins/forums). Many others also contributed valuable insights.
{% include image.html url="/img/PrivateLB.png" description="Private leaderboard score (MAP@3) - two teams stand out from the pack" %}

I really enjoyed working on this competition even though it was already one of the busiest periods of my life. The competition was launched while I was in the middle of writing my Master's Thesis in statistics in combination with a full time job. The data shows many interesting noisy and time dependent patterns which motivated me to play with the data before and after work. It was definitely worth every second of my time! I was inspired by the work of other [Kaggle winners](http://blog.kaggle.com/2014/08/01/learning-from-the-best/) and succesfully implemented my first two level model. Winning the competition is a nice extra but it's even better to have learnt a lot from the other competitors, thank you all!




