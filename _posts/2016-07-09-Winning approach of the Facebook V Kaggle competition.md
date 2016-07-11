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

The R source code is available on [GitHub](https://github.com/ttvand/Facebook-V/). This [thread](https://www.kaggle.com/c/facebook-v-predicting-check-ins/forums/t/22081/1st-place-winning-solution) on the Kaggle forum discusses the solution on a higher level and is a good place to start if you participated in the challenge.

## <a name="introduction"><a> Introduction

{% include image.html url="/img/FB5_banner.png" description="Competition banner" %}

From the competition page: The goal of this competition is to predict which place a person would like to check in to. For the purposes of this competition, Facebook created an artificial world consisting of more than 100,000 places located in a 10 km by 10 km square. For a given set of coordinates, your task is to return a ranked list of the most likely places. Data was fabricated to resemble location signals coming from mobile devices, giving you a flavor of what it takes to work with real data complicated by inaccurate and noisy values. Inconsistent and erroneous location data can disrupt experience for services like Facebook Check In.

The training data consists of approximately 29 million observations where the location (x,y), accuracy, and timestamp is given along with the target variable, the check in location. The test data contains 8.6 million observations where the check in location should be predicted based on the location, accuracy and timestamp. The train and test data set are split based on time. There is no concept of a person in this dataset. All the observations are events, not people. 

A ranked list of the top three most likely places is expected for all test records. The leaderboard score is calculated using the MAP@3 criterion. Consequently, ranking the actual place as the most likely candidate gets a score of 1, ranking the actual place as the second most likely gets a score of 1/2 and a third rank of the actual place results in a score of 1/3. If the actual place is not in the top three of ranked places, a score of 0 is awarded for that record. The total score is the mean of the observation scores.

{% include image.html url="/img/kaggle_screenshot-min.png" description="Check Ins where each place has a different color" %}

## <a name="explorAnalysis"><a> Exploratory analysis

Location analysis of the train check ins revealed interesting patterns between the variation in x and y. There appears to be way more variation in x than in y. It was suggested that this could be related to the streets of the simulated world. The difference in variation between x and y is however different for all places and there is no obvious spatial (x-y) pattern in this relationship.

It was quickly established by the community that time is measured in minutes and could thus be converted to relative hours and days of the week. This means that the train data covers 546 days and the test data spans 153 days. All places seem to live in independent time zones with clear hourly and daily patterns. No spatial pattern was found with respect to the time patterns. There are however two clear dips in the number of check ins during the train period.

Accuracy was by far the hardest feature to tackle. It was expected that it would be clearly correlated with the variation in x and y but the pattern is not as obvious. Halfway through the competition I cracked the code and the details will be discussed in the [Feature engineering](#featEng) section.

I wrote an [interactive Shiny application](https://tvdwiele.shinyapps.io/Facebook-V/) to research these interactions for a subset of the places. Feel free to explore the data yourself!

## <a name="probDef"><a> Problem definition
The main difficulty of this problem is the extended number of classes (places). With 8.6 million test records there are about a trillion (10^12) place-observation combinations. Luckily, most of the classes have a very low conditional probability given the data (x, y, time and accuracy). The major strategy on the forum to reduce the complexity consisted of calculating a classifier for many x-y rectangular grids. It makes much sense to make use of the spatial information since this shows the most obvious and strong pattern for the different places. This approach makes the complexity manageable but is likely to lose a significant amount of information since the data is so variable. I decided to model the problem with a single binary classification model in order to avoid to end up with many high variance models. The lack of any major spatial patterns in the exploratory analysis supports this approach.

## <a name="strategy"><a> Strategy

Generating a single classifier for all place-observation combinations would be infeasible even with a powerful cluster. My approach consists of a stepwise strategy in which the conditional place probability is only modeled for a set of place candidates. A simplification of the overall strategy is shown below

{% include image.html url="/img/Strategy4.png" description="High level strategy" %}

The given raw train data is split in two chronological parts, with a similar ratio as the ratio between the train and test data. The summary period contains all given train observations of the first 408 days (minutes 0-587158). The second part of the given train data contains the next 138 days and will be referred to as the train/validation data from now on. The test data spans 153 days as mentioned before.

The three raw data groups (train, validation and test) are first sampled down into batches that are as large as possible but can still be modeled with the available memory. I ended up using batches of approximately 30,000 observations on a 48GB workstation. The sampling process is fully random and results in train/validation batches that span the entire 138 days train range.

Next, a set of models is built to reduce the number of candidates to 20 using 15 XGBoost models in the second candidate selection step. The conditional probability P(place_match&#124;features) is modeled for all ~30,000*100 place-observation combinations and the mean predicted probability of the 15 models is used to select the top 20 candidates for each observation. These models use features that combine place and observation measures of the summary period. 

The same features are used to generate the first level learners. Each of the 100 first level learners are again XGBoost models that are built using ~30,000*20 feature-place_match pairs. The predicted probabilities P(place_match&#124;features) are used as features of the second level learners along with 21 manually selected features. The candidates are ordered using the mean predicted probabilities of the 30 second level XGBoost learners.

All models are built using different train batches. Local validation is used to tune the model hyperparameters.

## <a name="candidateSel1"><a> Candidate selection 1
The first candidate selection step reduces the number of potential classes from >100K to 100 by considering nearest neighbors of the observations. I considered the neighbor counts of the 2500 nearest neighbors where y variations are 2.5 times more important than x variations. Ties in the neighbor counts are resolved by the mean time difference since the observations. Resolving ties with the mean time difference is motivated by the shifts in popularity of the places. 

The nearest neighbor counts are calculated efficiently by splitting up the data in overlapping rectangular grids. Grids are created as small as possible while still guaranteeing that the 2500 nearest neighbors fall within the grid in the worst case scenario.

## <a name="featEng"><a> Feature engineering

### Feature engineering strategy
Three weeks into the competition, I climbed to the top of the public leaderboard with about 50 features. Ever since I kept thinking of new features to capture the underlying patterns of the data. I also added features that are similar to the most important features in order to capture the more subtle patterns. The final model contains 430 numeric features and this section is intended to discuss the most important ones.

All features are rescaled if needed in order to result in similar interpretations for the train and test features.

### Location

Different grids

### Time

### Accuracy

### Z-scores

### Most important features

{% include image.html url="/img/meanXVariationVsAc.png" description="Mean variation from the median in x versus time and accuracy" %}

## <a name="candidateSel2"><a> Candidate selection 2
The features from the previous section are used to generate binary classification models on 15 different train batches using XGBoost models. With 100 candidates for each observation, this is a slow process and it made sense to me to narrow down the number of candidates to 20 at this stage. The 15 candidate selection models are built with the top 142 features. The feature importance order is obtained by considering the XGBoost feature importance ranks of 20 other models. Hyperparameters were selected using the local validation batches. The 15 second candidate selection models all generate a predicted probability of P(place_match|data), I average those to select the top 20 candidates in the second candidate selection step.

At this point I also dropped observations that belong to places that only have observations in the train/validation period. This filtering was also applied to the test set.

## <a name="firstLL"><a> First level learners
The first level learners are very similar to the second candidate selection models other than the fact that they are fit on one fifth of the data. 100 base learners were fit on different random parts of the training period. Deep trees gave me the best results here (depth 11) and the eta constant was set to (11 or 12)/500 for 500 rounds. Colsampling also helped (0.6) and subsampling the observations (0.5) did not hurt but of course resulted in a fitting speed increase. I included either all 430 features or a random pick of the ordered features by importance in a desirable feature count range (100-285 and 180-240).

## <a name="secondLL"><a> Second level learners


## <a name="conclusion"><a> Conclusion
The private leaderboard standing below, used to rank the teams, shows the top 30 teams. It was a very close competition in the end and Markus would have been a well-deserved winner as well. We were very close to each other ever since the third week of the eight week contest and pushed each other forward. The fact that the test data contains 8.6 million records and that it was split randomly for the private and public leaderboard resulted in a very confident estimate of the private standing given the public leaderboard. I was most impressed by the approaches of Markus and Jack (Japan) who finished in third position. You can read more about their approaches on the [forum](https://www.kaggle.com/c/facebook-v-predicting-check-ins/forums). Many others also contributed valuable insights.

{% include image.html url="/img/PrivateLB.png" description="Private leaderboard score (MAP@3) - two teams stand out from the pack" %}

Running all steps on my 48GB workstation would take about a month. That seems like a ridiculously long time but it is explained by the extended computation time of the nearest neighbor features. While calculating the NN features I was continuously working on other parts of the workflow so speeding the NN logic up would not have resulted in a better final score.

Generating a ~.62 score could however be achieved in about two weeks by focusing on the most relevant NN features. I would suggest to consider 3 of the 7 distance constants (1, 2.5 and 4) and omit the mid KNN features. Cutting the first level models from 100 to 10 and the second level models from 30 to 5 would also not result in a strong performance decrease (estimated decrease of 0.1%) and cut the computation time to less than a week. You could of course run the logic on multiple instances and further speed things up.

I really enjoyed working on this competition even though it was already one of the busiest periods of my life. The competition was launched while I was in the middle of writing my Master's Thesis in statistics in combination with a full time job. The data shows many interesting noisy and time dependent patterns which motivated me to play with the data before and after work. It was definitely worth every second of my time! I was inspired by the work of other [Kaggle winners](http://blog.kaggle.com/2014/08/01/learning-from-the-best/) and succesfully implemented my first two level model. Winning the competition is a nice extra but it's even better to have learnt a lot from the other competitors, thank you all!



I look forward to your comments and suggestions!


