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

The training data consists of nearly 1 million users with monthly historical user and product data between January 2015 and May 2016. User data consists of 24 predictors including the age and income of the users. Product data consists of boolean flags for all 24 products and indicates whether the user owned the product in the respective months. The goal is to predict which **new** products the 929,615 test users are most likely to buy in June 2016. A product is considered new if it is owned in June 2016 but not in May 2016. The next plot shows that most users in the test data set were already present in the first month of the train data and that a relatively large share of test users contains the first training information in July 2015. Nearly all test users contain monthly data between their first appearance in the train data and the end of the training period.

{% include image.html url="/img/First observation test.png" description="First occurence of the test users in the training data" %}

A ranked list of the top seven most likely new products is expected for all users in the test data. The leaderboard score is calculated using the <a href="https://www.kaggle.com/wiki/MeanAveragePrecision" target="_blank">MAP@7 criterion</a>. The total score is the mean of the scores for all users. When no new products are bought, the MAP score is always zero and new products are only added for about 3.51% of the users. This means that the public score is only calculated on about 9800 users and that the perfect score is close to 0.035.

The test data is split randomly between the public and private leaderboard using a 30-70% random split. For those who are not familiar with Kaggle competitions: feedback is given during the competition on the public leaderboard whereas the private leaderboard is used to calculate the final standings.

## <a name="explorAnalysis"><a> Exploratory analysis
I wrote an <a href="https://tvdwiele.shinyapps.io/Santander-Product-Recommendation/" target="_blank">interactive Shiny application</a> to research the raw data. Feel free to explore the data yourself! This interactive analysis revealed many interesting patterns and was the major motivation for many of the base model features. The next plot shows the new product count in the training data for the top 9 products. 

{% include image.html url="/img/product frequency.png" description="New product counts by time for the top 9 products" %}

The popularity of products evolves over time but there are also yearly seasonal causes that impact the new product counts. June 2015 (dotted line in the plot above) is especially interesting since it contains a quite different new product distribution (*Cco_fin* and *Reca_fin* in particular) compared to the other months, probably because June marks the end of the tax year in Spain. It will turn out later on in the analysis that the June 2015 new product information is by far the best indicator of new products in June 2016, especially because of divergent behavior in the tax product (*Reca_fin*) and the checking account (*cco_fin*). The <a href="https://www.kaggle.com/c/santander-product-recommendation/forums/t/25579/when-less-is-more" target="_blank"> most popular forum post</a> suggests to restrict the modeling effort to new product records in June 2015 to predict June 2016. A crucial insight which changed the landscape of the competition after it was made public by one of the top competitors.

The interactive application also reveals that there is an important relation between the new product probability and the products that were owned in the previous month. The *Nomina* product is an extreme case: it only bought if *Nom_pens* was owned in the previous month or if it is bought together with *Nom_pens* in the same month. Another interesting insight of the interactive application relates to the products that are frequently bought together. *Cno_fin* is frequently bought together with *Nomina* and *Nom_pens*. Most other new product purchases seem fairly independent. A final application of the interactive application shows the distribution of the continuous and categorical user predictors for users who bought new products in a specific month.

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


