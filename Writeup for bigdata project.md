# Writeup for bigdata project

**Group members:** Xinyu Wang, Yixuan Huang, Jingsi Wu

-----

## Sumamry

For this final project we focused on analyzing the dataset comes from a website calls `Hacker News`. The dataset is about 4.0GB. We used spark sql to clean and analyze these two datasets. Visualizing and select realative variables to complete feature engineering, then use pyspark machine learning package to build regression, random forest and k-means cluster model to predict stories' score.



## Introduction

Data source: <a href="https://www.kaggle.com/hacker-news/hacker-news" target="_blank">Hacker News</a>

The dataset contains data from 2006 to 2015, including all the articles and comments information from `Hacker News`. `Hacker News` is a social news website focusing on computer science and entrepreneurship. It is run by Paul Graham's investment fund and startup incubator, Y Combinator. ***stories*** and ***comments*** are 2 separate datasets each contains certain information about Hacker News website's articles and comments. Inspired by the kaggle website, we want to analyze whether Hacker News is tend to be dominated by a very small fraction of users, the overall user behavior, eg: the most active hours in submitting the story and whether there is a bias towards Y Combinatoralso, ect. We also predict stories' score by using certain features.



## Code files

The data preparation part is included in 

The modeling part is included in

## Methods

###  Hypothesis



### Data preparation

#### Basic analyzing

For ***stories*** basic analyzations we want to focus on:

- Stories's sources (most frequent/with highest scores)

  In this part we first use regex expression to extract hostname from url, then count the number and get top 20 of it.

  图

  We can see that most articles comes from Github, Techcrunch and NYTimes.

  图

  Website sources with highest average score are Heartbleed, Bekkelund and Spritelamp, which shows us that not all articles from the most popular resources have high quality.

- Stories' distribution(over years/hours)

  The result shows that from 2006-2012, total stroy number for each year grows rapidly, but after 2012 it starts to decrease. One reason can be that Hacker News is losing its users and become less active.

  

  From the time distribution we can see that most articles were posted in the afternoon or in the evening, a few parts of them were posted in the morning, which means that people usually use this forum after work/study.

  图

- Average score of the stroties

  The maximum score of the dataset is 4339, the minimum is 0 and the average is 11.06, which means that a lot of articles don't get very high score. 

- Top authors(with highest scores/most stories)

  We collect authors' id order by total score they get, total stories they published and average score they get separately. The results of first 2 are very similar, authors with highest  total score usually do have highest total stories, but not highest average score.

  图

  图

  图

- Whether there's a bias toward  Y Combinator

  We can see that although the total number of articles which mentioned Y Combinator takes only a few percentage, but the average score of these articles are quiet high, so we can suspect that Hacker News does have bias toward Y Combinator

  图 

- Stories of major companies(Apple/Google/Uber)

  

For ***comments*** basic analyzations we want to focus on:

- Top users in the website

  From the result we can see that *tptacek*, *jacquesm* and *DanBC* are the top 3 active users in Hacker News, they comments a lot over these years.

  (写这个的时候突然想到用 avg_comments 是不是更好，可能只是加入的时间早)

  图

- Comments distribution(over years/hours)

  Similar to the stories distribution pattern, comments grows rapidly from 2006 to 2013, and then start to decrease.

  图

  Also, most of the comments were posted in the afternoon and in the evening, similar to the stories.

- Most hottest comments

  By counting which parent comments have most followup comments, we can find the most popular comments, as below.

  图 

#### Feature engineering

In this part we create 10 features for future modeling

1. ***year***, ***month***, ***dayofmonth*** from ***time_ts*** 

2. ***active_user*** by selecting top 3000 active users from dataset and encode original column into binary variable

3. ***title length*** by split and count how many words in title

4. ***from_top_web*** by selecting top 50 website sources and encode the original column into binary variable

5. ***text_length*** by split text of each article and count the word number

6. ***title_hot_words*** by split title and remove stop words in title, then choose top 300 most frequently appeared words as hot words, count how many top words appears in each story's title.

   We also use ***author*** and ***descendants*** as input features in following model processes.

### Modeling



## Conclusions



## Future work





