### Capstone-Project-Internet-Forum-Mental-Health-Risk-Prediction

### Overall Summary

Capstone Project undertaken for the General Assembly Data Science Immersive Programme.
My goals for the project were to understand and predict mental health risk in users of the internet forum website Reddit, initially at the wider user base level before changing to user individual users.

I extracted post data via SQL from a BigQuery data repository.

Project MKI files refer to EDA looking into a potential time series analyses of number of posts to mental health related subreddits based on sentiment & clustering of daily news stories from the major Reddit news forum. Unfortunately data was restricted to news headlines only and was thus limited in terms of predictive potential.

I then decided to pivot to looking into user behaviour in regards to posting to other forums and whether this can be used to predict a user posting to mental health related forums (as a proxy for mental health risk). The overall sample was significantly skewed (approx. 3% of users posted to mental health related forums) and thus I applied a number of under and over sampling methodologies to the train set to improve the predictive of my models. 

**_The below summary focuses upon the MKII file looking into prediction of mental health risk in a given user_** 

### Problem Statement & Project Goals 
**Overview**
* The forum website reddit hosts a wide variety of different themed forums (subreddits), including a large number of mental health dedicated subreddits
* The goal of these subreddits is to provide users support and information relating to various topics relevant to mental health

**Hypotheses & Goals**
* The underlying hypothesis for this project is that posting to these subreddits can act as an estimation of mental health risk in an individual
* This projects seeks to identify internet forum users who may be at risk of mental health disorders based upon their use of other internet forum types
* Further, understanding the link between seemingly unrelated subreddits and mental health can lead to promotion of these subreddits to users who may benefit from support

**Problem Statement**
* Identification or diagnoses of at risk patients from a wider population is a key goal for many machine learning (ML) applications
* However, for the majority of disorders only a small minority of the sample will be positive for the disorder we wish to identify
* Our dataset consists of all users and posts to subreddits in the full year 2018
* The vast majority of these users will not have posted  to mental health themed subreddits
* Applying standard ML techniques is likely to lead to models to ignoring the minority class, resulting in low to no identification of these individuals
* A key component for this project is how to extract the most relevant information from a severely imbalanced sample


### Data Collection & Cleaning
**Dataset**
* Data was pulled from the bigquery pushshift.io dataset via SQL, with author, subreddit and count of number of posts  from 2018
* Pulled data included author and subreddits, as well as count per subreddit to allow for total number of posts per user to subreddit
* In cases where the author or subreddit was deleted these were excluded from the SQL query:
  * Due to dataset size the pull was split into three sections and then combined via joining
  * Post-joining any null values were removed
  * Author names were converted to numbers to both save memory and avoid delving into specific author behaviour on reddit


### EDA
* The dataset incorporated 12.1M unique users posting to 1.4M unique subreddits with a total of 125M posts in 2018
  * After null values were removed but prior to additional cleaning steps and further EDA
* Further cleaning steps & EDA included:
  * Assessment and removal of outliers
  * Removal of users who posted a small number of subreddits 
  * Condensing of mental health themed reddits into a single target variable
  * Assessment of correlation between our target & the top subreddits

**Correlation with Top Subreddits**
* Post EDA we wanted to understand the correlation between our target variable and each subreddit
* Given there were over 50k subreddits included we focused on the top ones
* In the majority of instances correlation is very low (between -5 to 5% correlation with positive target)
* This is unsurprising given the significant imbalance within our target class and the fact that these are the most popular subreddits overall
* However, in some instances we do see greater correlation, such as r/advice, r/legaladvice & r/askdoctors, suggesting that those users which may have greater impulse to seek help are also those which may be suffering from mental health disorders

### Initial Modelling
* Initial modelling results show that only random forests & gradient boost were able to beat baseline with all instances essentially ignoring the minority class
* Despite high accuracy across all models, precision and recall scores are both low, showing that utilisation of these models, with accuracy the scoring metric, would be poor predictors of mental health risk

### Assessment of Sampling Techniques
* To improve the models in terms of identifying the minority class, several under and oversampling methodologies were applied to the training set
* Employing each sampling technique, scored by recall, we see a significant increase in identification of the majority class but a high level of false positives 
* Visualisation of accuracy vs. recall for these models show a distinct trade-off, the model(s) to take forward are the ones which minimize the accuracy recall trade-off
* Due to time constraints one logistic regression model was taken forward for analysis & gridsearch on the full dataset

### Conclusions & Next Steps
* Overall the evaluated models offer potential for identifying mental health risk in internet forum users
* In particular, the greatest potential use case is likely to be identifying users who may not currently be aware of, and post to, support forums but who could benefit from their supports & thus promotion on over popular subreddits
* The high recall at the expense at accuracy is too much in some cases, though a high number of false positives is not as much of an issue for a risk analyses, and indeed may reflect current sufferers of mental health issues who simply do not post to reddit about their issues
 * Their are some potential risks & limitations for this research:
  * The demographic used in the sample is likely to be younger & focused on North America & Europe 
  * The majority of users on any website are ‘lurkers’ and do not post, this research assumes that they are represented & do not differ significantly in personality etc. than those who do post
  *  Key assumption is that posting on mental health forums is indicative of mental health risk 
* Next steps would include:
  * Grid Searching & fine tuning remaining models
  * Expanding data set to incorporate additional years for greater breadth of sample
  * Network analysis to flesh out those subreddits most closely related to mental health 
