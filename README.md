### Capstone-Project-Internet-Forum-Mental-Health-Risk-Prediction
Is it possible to identify internet forum users who might require support for mental health related issues based on their wider interests? 

In this project I used Reddit post data from all users in 2018 to predict whether a user is likely to post to mental health advice forums (forums are called 'subreddits') based on user activity/posting in other forums.

### Overall Summary

This was my capstone project undertaken for completion of the General Assembly Data Science Immersive Programme.

My goals for the project were to understand and predict mental health risk in users of the internet forum website Reddit, initially I wanted to estimate the impact of news topics and sentiment on mental health risk, before pivoting to look at user interests and the relationship with mental health risk. 
 * Mental heatlh risk in this context is whether a user posts to the various mental health related subreddits
 * Users of these subreddits are typically looking to vent or to ask for advice from fellow users

Initially I wanted to look a potential time series analysis of the overall number of posts to mental health related subreddits based on sentiment & clustering of daily news stories from the major Reddit news forum. Unfortunately data was restricted to news headlines only and after some initial EDA it became apparent that more information per article would be required to perform adequate NLP and create a worthwhile analysis.

I then decided to pivot into looking into user behaviour in terms of posting to other, unrelated, forums and whether this can be used to predict mental health risk. I created a boolean target variable of whether a user had posted to a mental health related subreddit over the time period as my estimator for mental health risk in a specific individual.

### Problem Statement & Project Goals 
**Background**
* The forum website reddit hosts a wide variety of different themed forums (subreddits), including a large number of mental health dedicated subreddits
* The goal of these subreddits is to provide users support and information relating to various topics relevant to mental health
* Understanding the link between mental health dedicated subreddits and seemingly unrelated subreddits could help websites like Reddit identify users who may be suffering in terms of mental health and who could benefit from promotion of advice forums and other resources which offer support

**Hypotheses & Goals**
* This projects seeks to identify internet forum users who may be at risk of mental health disorders based upon their use of other internet forum types
* The underlying hypothesis states that there is a link between mental health and wider interests

**Problem Statement**
* Identification or diagnoses of at risk patients from a wider population is a key goal for many machine learning applications
* However, for the majority of disorders only a small minority of the sample will be positive for the disorder we wish to identify
* Additionally a significant proportion of a sample may be unidentified as at risk
* Applying standard ML techniques is likely to lead to models to ignoring the minority class, resulting in low to no identification of these individuals
* A key component for this project is how to extract the most relevant information from a severely imbalanced sample

### Data Collection & Cleaning 
 * File: MKII_Data_Extraction_Cleaning
 
**Data Collection**
* Data was pulled from the pushshift.io dataset stored on BigQuery via SQL
* The data covered 2018 and included post author and the subreddit posted to, as well as the count of total posts per subreddit per author 
* In cases where the author or subreddit were deleted these were excluded from the SQL query
  * Due to dataset size the pull was split into three sections and then combined via joining
  * Post-joining any null values were removed
  * Author names were converted to numbers to both save memory and avoid delving into specific author behaviour on reddit
* Before any further cleaning steps were taken there were 12.1M unique users, posting to 1.4M unique subreddits with a total of 125M posts in 2018:
![Unique Variable Counts]
(https://github.com/samholt13/GA_Capstone_Project/blob/master/Images/unique_features_precleaning.png)
