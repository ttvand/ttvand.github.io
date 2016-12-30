---
layout: post
title: "Santander Product Recommendation"
subtitle:   "Second place and $20K prize in my second featured Kaggle competition!"
date:       2016-12-30 00:00:00
author:     "Tom Van de Wiele"
header-img: "img/Santander office 3 shaded.jpg"
comments: true
---



The <a href="https://www.kaggle.com/c/santander-product-recommendation" target="_blank">Santander Product Recommendation data science competition</a> where the goal was to predict which new banking products customers were most likely to buy has just ended. After my <a href="https://ttvand.github.io/Winning-approach-of-the-Facebook-V-Kaggle-competition/" target="_blank">earlier success</a> in the <a href="https://www.kaggle.com/c/facebook-v-predicting-check-ins" target="_blank">Facebook recruiting competition</a> I decided to have another go at competitive machine learning by competing with over 2,000 data scientists. This time I finished 2nd out of 1785 teams! In this post, I’ll explain my approach.

## Overview

This blog post will cover all sections to go from the raw data to the final submissions. Here's an overview of the different sections. If you want to skip ahead, just click the section title to go there.

* *[Introduction](#introduction)*
* *[Exploratory analysis](#explorAnalysis)*
* *[Strategy](#strategy)*
* *[Feature engineering](#featEng)*
* *[Base models](#baseModels)*
* *[Base model combination](#baseModelComb)*
* *[Post-processing](#postProcessing)*
* *[Ensembling](#Ensembling)*
* *[Conclusion](#conclusion)*

The R source code is available on <a href="https://github.com/ttvand/Santander-Product-Recommendation/" target="_blank">GitHub</a>. This <a href="https://www.kaggle.com/c/santander-product-recommendation/forums/t/26824/2nd-place-solution" target="_blank">thread</a> on the Kaggle forum discusses the solution on a higher level and is a good place to start if you participated in the challenge.

## <a name="introduction"><a> Introduction
**From the competition page**: Ready to make a downpayment on your first house? Or looking to leverage the equity in the home you have? To support needs for a range of financial decisions, Santander Bank offers a lending hand to their customers through personalized product recommendations.
{% include image.html url="/img/santander-banner-ts-660x.png" description="" %}
Under their current system, a small number of Santander’s customers receive many recommendations while many others rarely see any resulting in an uneven customer experience. In their second competition, Santander is challenging Kagglers to predict which products their existing customers will use in the next month based on their past behavior and that of similar customers. With a more effective recommendation system in place, Santander can better meet the individual needs of all customers and ensure their satisfaction no matter where they are in life.

## <a name="explorAnalysis"><a> Exploratory analysis


## <a name="strategy"><a> Strategy


## <a name="featEng"><a> Feature engineering


## <a name="baseModels"><a> Base models


## <a name="baseModelComb"><a> Base model combination


## <a name="postProcessing"><a> Post-processing


## <a name="Ensembling"><a> Ensembling


## <a name="conclusion"><a> Conclusion
The private leaderboard standing below, used to rank the teams, shows the top 30 teams. It was a very close competition on the public leaderboard between the top three teams but idle_speculation was able to generalise better making him a well deserved winner of the competition. I am very happy with the second spot, especially given the difference between second, third and fourth, but I would be lying if I said that I hadn’t hoped for more for a long time. There was a large gap between first and second for several weeks but this competition lasted a couple of days too long for me to secure the top seat. I was able to make great progress during my first 10 days and could only achieve minor improvements during the last four weeks. Being on top for such a long time tempted me to make small incremental changes where I would only keep those changes if they improved my public score. With a 30-70% public/private leaderboard split this approach is prone to overfitting and in hindsight I wish that I had put more trust in my local validation. Applying trend detection and MAP optimization steps in all of my submissions would have improved my final score to about 0.03136 but idle_speculation would still have won the contest. I was impressed by the insights of many of the top competitors. You can read more about their approaches on the <a href="https://www.kaggle.com/c/santander-product-recommendation/forums/t/26831/all-solutions" target="_blank">Kaggle forum</a>.

{% include image.html url="/img/PrivateLBSant2.png" description="Private leaderboard score (MAP@7) - idle_speculation stands out from the pack" %}

Running all steps on my 48GB workstation would take about a week. Generating a ~0.031 private leaderboard score (good for 11th place) could however be achieved in about 90 minutes by focusing on the most important base model features using my feature ranking and only using one model for each product-lag combination. I would suggest to consider only the top 10 features in the base model generation and omit the folds from the model generation if you are mostly interested in the approach rather than the result.

I really enjoyed working on this competition although I didn't compete as passionately as I did in the Facebook competition. The funny thing is that I would actually never have participated had I not quit my pilgrimmage on the famous Spanish "Camino del Norte" because of food poisoning in... Santander. I initially considered the Santander competition as a great way to keep busy whereas I saw the Facebook competition as a way to change my professional career. Being ahead for a long time also made me a little complacent but the final days on this competition brought back the great feeling of close competition. The numerous challenges at <a href="https://deepmind.com/" target="_blank">Google DeepMind</a> will probably keep me away from Kaggle for a while but I hope to compete again in a couple of years with a greater toolbox!


I look forward to your comments and suggestions.


