

# Writeup for bigdata project

**Group members:** Xinyu Wang, Yixuan Huang, Jingsi Wu

-----

## Sumamry

For this final project we focused on analyzing the dataset comes from a website calls `Hacker News`. The dataset is about 4.0GB. We used Spark SQL to clean and analyze these two datasets. Visualizing and select relative variables to complete feature engineering, then use Pyspark machine learning to build regression, random forest and k-means cluster model to predict stories' score.

## Introduction

Data source: <a href="https://www.kaggle.com/hacker-news/hacker-news" target="_blank">Hacker News</a>

The dataset contains data from 2006 to 2015, including all the articles and comments information from `Hacker News`. `Hacker News` is a social news website focusing on computer science and entrepreneurship. It is run by Paul Graham's investment fund and startup incubator, Y Combinator. Story and Comment are two types of the post. There are 200 million story rows and 980 million comment rows in the datasets.

Inspired by the Kaggle website, we want to analyze whether Hacker News tends to be dominated by a very small fraction of users, whether Hacker News' story has certain dominated websites as story sources, whether there is a bias towards Y Combinator, the overall user behavior, eg: the most active hours in submitting the story,ect. We also predict stories' score by using certain features.



## Code files

The data preparation part is included in `Preprocessing.ipynb`

The modeling part is included in `Modeling.ipynb`

## Methods

###  Hypothesis

The Hacker News as a forum tends to be dominated by a very small fraction of users, which is typical in other forums.

The Hacker News' story has certain dominated websites as story sources. 

There is a bias towards Y Combinator.

### Data preparation

We clean the data by first set the data type of certain features and then by filtering the not null value in certain variables, like id, score, time_ts, title, URL, descendants, and author.

 We then use regex expression to extract the hostname from the url and then perform the basic analysis of the dataset.

#### Basic analyzing

For ***stories*** basic analyzations we want to focus on:

- Stories' number and average score distribution (over years/months/day of week/hours)

![p1](http://ww4.sinaimg.cn/large/006tNc79ly1g5dt6ucgblj30ln0a2aaf.jpg)



The result shows that from 2006-2012, the total story number for each year grows rapidly, but after 2012 it starts to decrease. One reason can be that Hacker News is losing its users and become less active. However, the same pattern doesn’t apply to the average score. The average score increased at first and then stay constant, meaning that the story quality is becoming more stable at recent year.

![p2](http://ww4.sinaimg.cn/large/006tNc79ly1g5dt7tmd0nj30l00a0gm5.jpg)

 

The result below shows the total story number for each month stays around 120000 to 140000. The average score stay almost constant.

![p3](http://ww1.sinaimg.cn/large/006tNc79ly1g5dt8vyu9cj30k408sdgf.jpg)

 ![p4](http://ww4.sinaimg.cn/large/006tNc79ly1g5dt9oaqjxj30jq096t9c.jpg)



The result below shows that for per day of week, Monday has the lowest story number and  highest average score, number of story reach to the highest on Wednesday, and the average score remain almost the same between Wednesday and Saturday. Monday is a higher quality story weekday among the week days.

![p5](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtg5npmoj30kc0943yw.jpg) 

![p6](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtgpigiej30k8098mxi.jpg)



From the time distribution showing as below, we can see that most articles were posted in the afternoon or in the evening, a few parts of them were posted in the morning, which means that people usually use this forum after work/study.

![p7](http://ww3.sinaimg.cn/large/006tNc79ly1g5dthpyeg5j30mu0ag758.jpg)

![p8](http://ww3.sinaimg.cn/large/006tNc79ly1g5dti1hq3rj30iq08q756.jpg)

 

- Stories' sources (most frequent/with highest scores/with highest average scores)

  In this part, we first use regex expression to extract the hostname from the url, then count the story number and get top 10 of it showing as below.

  We can see that most stories come from Github, Techcrunch and NYTimes.

  ![p9](http://ww3.sinaimg.cn/large/006tNc79ly1g5dtiwrulvj30q00d20th.jpg)

  

  We also calculate the total story score from all website and get the top 10 of it showing as below. The top 10 highest total score story websites are the same as the top 10 highest number website list, however, the ranking has a slight difference, meaning that certain website doesn't have the same quality as others in top 10 story sources.

  ![p10](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtk15sb4j30q00degma.jpg)

  

  Website sources with the highest average score showing as below are Heartbleed, Bekkelund and Spritelamp, which shows us that not all articles from the most popular resources have high quality.

  ![p11](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtljmhm2j30q00eo754.jpg)

   

- Average score of the stories

  The maximum score of the dataset is 4339, the minimum is 0 and the average is 11.06, which means that a lot of articles don't get a very high score. 

 

- Top authors(with highest scores/most stories)

  We collect authors' id order by the total score they get, total stories they published and average score they get separately. The results of the first 2 are very similar, authors with the highest total score usually do have the highest total stories, but not the highest average score.

  ![p12](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtoabxdbj30q00d4t9m.jpg) 

  ![p13](http://ww2.sinaimg.cn/large/006tNc79ly1g5dtoyld01j30q00co3zb.jpg)

  ![p14](http://ww1.sinaimg.cn/large/006tNc79ly1g5dtph5j77j30q00e0aaw.jpg)

  

- Whether there's a bias toward Y Combinator

  We can see that although the total number of articles which mentioned Y Combinator takes only a few percentage, but the average score of these articles are quite high, so we can suspect that Hacker News does have bias toward Y Combinator

  ![p15](http://ww3.sinaimg.cn/large/006tNc79ly1g5dtpzkfr6j30bu090glv.jpg)

  ![p16](http://ww3.sinaimg.cn/large/006tNc79ly1g5dtqbo1w2j30840aegn2.jpg)

  

- Stories of major companies(Apple/Google/Uber)

  Apple 

  Story number of Apple reach to the highest in 2012, and then began to decrease, the total score reach to its highest in 2011.

  ![p17](http://ww2.sinaimg.cn/large/006tNc79ly1g5dtqxptz0j30q00bydge.jpg)

  Google

  Story number of Google reach to the highest in 2012, and then stay stable and began to decreas from 2014, the total score reach to its highest in 2013.

  ![p18](http://ww1.sinaimg.cn/large/006tNc79ly1g5dtr8gx58j30q00bygm8.jpg)

  Uber

  Story number of Uber reach to the highest in 2015, and the total score reach to its highest in 2015, too.  

  ![p19](http://ww2.sinaimg.cn/large/006tNc79ly1g5dtrotn9fj30q00c8q3j.jpg)

  

- The Hacker News as a forum tends to be dominated by a very small fraction of users: around 32% of the stories are coming from 1000 users (0.58%).
- Total story number: 1,621,236
- Total user number: 171,351
- Story number from top 1000 users: 515,253

 

- The Hacker News' story has certain dominated websites as story sources: around 26.3% of the stories are coming from 50 website (0.029%).
- Total story number: 1,621,236
- Total website number: 167,904
- Story number from top 50 website: 426,972

   

For ***comments*** basic analyzations we want to focus on:

 

- Top users in the Hacker News

  From the result we can see that *tptacek*, *jacquesm* and *DanBC* are the top 3 active users in Hacker News, they comment a lot over these years.

  ![p20](http://ww3.sinaimg.cn/large/006tNc79ly1g5dts6nuzmj30o40ccwf3.jpg)

   

- Comments distribution(over years/hours)

  Similar to the stories distribution pattern, comments grow rapidly from 2006 to 2013, and then start to decrease.

  ![p21](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtsm19bcj30pg0bw74v.jpg)

  

  Also, most of the comments were posted in the afternoon and in the evening, similar to the stories.

  ![p22](http://ww2.sinaimg.cn/large/006tNc79ly1g5dtu7k842j30q00beq42.jpg)

  

- Most hottest comments

  By counting which parent comments have most followup comments, we can find the most popular comments, as below.

  ![p23](http://ww4.sinaimg.cn/large/006tNc79ly1g5dtup95quj30q00cujs8.jpg)

 

- Users distribution in top 1000(the highest total story number/the highest total story score/the highest average story score)

  We find the top 1000 users who have the highest total story number, the highest total story score, the highest average story score separately. 

  By counting the overlap between the 3 subsets, we can see that there are 16 common users both in the top 1000 highest total score and highest average score, 184 common users both in the top 1000 highest total score and highest comments number, only 4 common users both in the top 1000 highest average score and highest comments number, and only 1 common users which are both in the 3 subsets, as below.

  ![p24](http://ww3.sinaimg.cn/large/006tNc79ly1g5dtvcspedj30as07yglt.jpg)

  

  This result shows that in 1000 users, who has the highest total score may be more active in posting a comment, however, this does not necessarily mean that they are the users with high quality story posting.

### Feature engineering

In this part we create 11 features for future modeling

1. ***year***, ***month***, ***dayofweek*** from ***time_ts*** 

2. ***active_user*** by selecting top 3000 active users from dataset and encode original column into binary variable

3. ***title length*** by split and count how many words in title

4. ***from_top_web*** by selecting top 50 website sources and encode the original column into binary variable

5. ***text_length*** by split text of each article and count the word number

6. ***title_hot_words*** by split title and remove stop words in title, then choose top 300 most frequently appeared words as hot words, count how many top words appears in each story's title.

7. ***title_vec*** by transforming title to word vectors

   We also use ***author*** and ***descendants*** as input features in following model processes.

### Modeling

1. **Linear regression model**

Input features: ***descendants, year, dayofweek, month, text_length, title_hot_words, title_vec***

Evaluation: R2: 0.0046  RMSE: 42.43 MAE: 15.66

![p26](http://ww1.sinaimg.cn/large/006tNc79ly1g5dtvzdcdrj30ps08g40w.jpg)



2. **Random forest  model**

Input features: ***des_scaled, year, dayofweek, month, title_length, text_length, title_hot_words, title_vec, active_userVec, from_top_webVec***

des_scaled is a normalized descendants variable

active_userVec  and from_top_webVec are variables encoded as categorical from the original ones.

Evaluation:

![p27](http://ww1.sinaimg.cn/large/006tNc79ly1g5dtwiul5dj30cc0383yj.jpg)

3. **Logistic regression model**

Input features: ***year, dayofweek, month, title_length, text_length, title_hot_words, title_vec, active_userVec, from_top_webVec***

Label feature: ***if_highscore_scaled***

Scaled the score and then encode it into binary variable using median value as threshold

Evaluation:

Area under ROC = 1.0

Two category classification is too blurry for score variable



4. **K-means cluster model**

Input features: ***score***, ***descendants***, ***year, dayofweek***, ***month***, ***text_length***, ***t******itle_hot_wor*ds**, ***title_vec, active_us*er**, ***from_top_web***

Model performance is really good with k=5, which means that
for our future multiple classification we can try to split score into 5
categories.

![p28](http://ww2.sinaimg.cn/large/006tNc79ly1g5dtwyeyymj30as0b8q4g.jpg)



## Conclusions

The Hacker News does be dominated by a very small fraction of users: around 32% of the stories are coming from 1000 users (0.58%).

The Hacker News' story does have certain dominated websites as story sources: around 26.3% of the stories are coming from 50 website (0.029%).

There is a bias towards Y Combinator.

Hacker news has passed its most prosperous time and starting to lose active users, maybe it’s because many articles in this website fall in the low score area, improving story quality may help Hacker News to recover.

In order to predict score using given information (most of the data are strings), we did some feature engineering, encoded them into vectors and categorical variables.

## Future work

May continue on multiple classification on score, and add text sentiment analysis towards article to see whether it will affect score.





