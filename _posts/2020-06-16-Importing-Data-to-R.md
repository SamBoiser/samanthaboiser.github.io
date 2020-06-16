---
layout: post
title: Importing Data to R
---

*For the exercise last week, I took 500 tweets with the words 'Education Reform'. I've compiled a simple scatter plot of favorites to retweets with the additional analysis of replies to gauge holistic interaction.*

![](/images/Tweets%20with%20'Education%20Reform'.png)

While we can analyze trends, I wanted to detail a few specific cases, namely some visual outliers.
The most favorited and retweeted tweet was _simply_:
>We need education reform pls
We can postulate why this tweet garnered the most interactions from this day. Its simplicity? Its timing? The account?

Additionally, the most replied tweet with 'education reform' from the day, from @globaltimesnews, was:
>Also, #HongKong police intercepted a 14- and a 15-yr-old student in Tsuen Wan who possess gears such as helmets and gas masks. Some local residents said they are worried about more youngsters taking part in riots, which shows an education reform is a must.

This poses the question - are favorites & retweets more common with simple, generic tweets while more detailed, focused tweets evoke more replies.
Of course, there are many variables to take into account, such as the fact that the most replied tweet comes from a news account.

Additionally, I've compiled data from @TeachThought, a popular education twitter account with a similar analysis.

![](/images/Teach%20Thoughts%20Tweets%20Final.png)

These tweets were taken from a month's span of time (May 7, 2020 to June 7, 2020). Despite significant momentum in the Black Lives Matter movement, it seems little correlation exists.

The data brings forth major points of further research. Find my R code below:

'''R
install.packages("RCurl")
library(RCurl)
library(ggraph)
library(tidyverse)

x <- getURL("https://raw.githubusercontent.com/SamBoiser/samboiser.github.io/master/project2.csv")
project2 <- read.csv(text=x)

project
project2

Frequencyplot2 <- ggplot(data=project2) +
  geom_point(mapping=aes(x=Date,y=Frequency,color=Favorites,size=Retweets))

mynamestheme <- theme(plot.title = element_text(hjust=0.5, face = "bold", size = (15)))
                     
Frequencyplot2 + mynamestheme + ggtitle("Frequency of @TeachThought's Tweets")

#Project2
Frequencyplot <- ggplot(data=project) +
  geom_point(mapping=aes(x=favorites,y=retweets,size=replies), position="jitter")

mynamestheme1 <- theme(plot.title = element_text(hjust=0.5, face = "bold", size = (15)))

Frequencyplot + mynamestheme1 + ggtitle("Tweets with 'Education Reform'")
'''
