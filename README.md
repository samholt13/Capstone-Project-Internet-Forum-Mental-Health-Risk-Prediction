**NOTE: currently in process of adding notebooks**

### Capstone-Project-Internet-Forum-Mental-Health-Risk-Prediction
Is it possible to identify internet forum users who might require support for mental health related issues based on their wider interests? 

In this project I used Reddit post data from all users in 2018 to predict whether a user is likely to post to mental health advice forums (forums are called 'subreddits') based on user activity/posting in other forums.

### Overall Summary

This was my capstone project undertaken for completion of the General Assembly Data Science Immersive Programme.

My goals for the project were to understand and predict mental health risk in users of the internet forum website Reddit.

Initially I wanted to look a potential time series analysis of the overall number of posts to mental health related subreddits based on sentiment & clustering of daily news stories from the major Reddit news forum. Unfortunately data was restricted to news headlines only and after some initial EDA it became apparent that more information per article would be required to perform adequate NLP and create a worthwhile analysis.

I then decided to pivot into looking into user behaviour in terms of posting to other, unrelated, forums and whether this can be used to predict mental health risk.

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

![Unique Variable Counts](https://github.com/samholt13/GA_Capstone_Project/blob/master/Images/unique_features_precleaning.png)

**Data Cleaning**
* Several cleaning steps were undertaken to prepare the data for EDA and analysis:
  * Converted author usernames to a numerical key
  * Checked and removed outliers in term of number of posts per user
    * High posters were often bots or spam and were removed
  * Removed users who only post to a small number of subreddits due to lack of information
  * Combined mental health related subreddits into boolean target variable
  * Merged single user and those with very few users into a single target variable

### EDA 
 * File: MKII_EDA
 
**Data Manipulation & Feature Selection**
 * I created a formula to create a sparse pivot table in order to conduct EDA, with authors for rows, subreddits for columns & number of posts as variables
 * Due to size of the database I resampled the data to get 80,000 (10%) samples
 * Baseline model accuracy was 95% (5% of samples were positive for my target variable) 
 * Given the sheer number of features I used a simple model-based feature selection process (RandomForestClassifier) to reduce the overall number and facilitate correlation calculations, extracting the top 500 features overall
 
 **Correlation with Target Variable**
* Given the sheer imbalance between classes, overall correlation with the target variable is low (between -5 to 5% correlation with positive target)
* The features with the highest correlation to the target variable were typically focused upon advice (Advice, AskDocs, relationships), drugs, loneliness and some mental health related subreddits not included within the target variable:

![Correlation with Target Variable](https://github.com/samholt13/GA_Capstone_Project/blob/master/Images/download.png)

### Data Collection & Cleaning 
**Proof of Concept Modelling**
* I created a function to run my models and ran logistic regression, KNN, decision tree, random forest classifier, gradient boost and SVC models to assess baseline results without parameter tuning:

![Baseline Modelling Results](https://github.com/samholt13/GA_Capstone_Project/blob/master/Images/download-2.png)
* Most of the evaluated models achieved higher accuracy than baseline, though low recall scores show missing the point of the project!
* Recall is important here as it reflects the number of positive results predicted by the model vs. all actual positive results (True positive / (True positive + False negative))
* High accuracy but low recall this implies the model is essentially ignoring the positive results and predicting everything as negative for the target, basically identifying very few users for mental health risk

 
**Optimising through Sampling**
* To attempt to improve the recall for my models I created a class (TinyTarget) to evaluate several under and over sampling techniques:
 * Random undersampling, TomekLinks, NeighbourhoodClean, NearMiss, SMOTE upsampling, random oversampling, ADASYN
 * Results show a significant improvement in recall in the majority of instances, though decreasing accuracy as recall improves:
 
 ![Modelling Results after Sampling Techniques Implemented](https://github.com/samholt13/GA_Capstone_Project/blob/master/Images/download-3.png)
 
 * The models I would take forward for further evaluation would be those with accuracy > 80% and recall > 50% 
 * The siginifcant increase in recall offers potential for utilising these models and given the insidious nature of mental health risk proactive identification (even in the case of false positives) is preferred to a higher proportion of false negatives
 
 
 ### Summary & Next Steps
 * Overall the evaluated models offer potential for identifying mental health risk in internet forum users
* In particular, the greatest potential use case is likely to be identifying users who may not currently be aware of, and post to, support forums but who could benefit from their supports & thus promotion on over popular subreddits
* The high recall at the expense at accuracy is too much in some cases, though a high number of false positives is not as much of an issue for a risk analyses, and indeed may reflect current sufferers of mental health issues who simply do not post to reddit about their issues
* Their are some potential risks & limitations for this research:
    * The demographic used in the sample is likely to be younger & focused on North America & Europe 
    * The majority of users on any website are ‘lurkers’ and do not post, this research assumes that they are represented & do not differ significantly in personality etc. than those who do post
    * Key assumption is that posting on mental health forums is indicative of mental health risk 
* Next steps would include:
    * Grid Searching & fine tuning identified models
    * Running identified models on full (non-resampled dataset)
    * Expanding data set to incorporate additional years for greater breadth of sample
    * Reaching out to reddit users for more information regarding background & mental health to inform additional features
    * Network analysis to identify out those subreddits most closely related to mental health 
    

