# Capstone-Project-Internet-Forum-Mental-Health-Risk-Prediction
Capstone Project undertaken for the General Assembly Data Science Immersive Programme
My goals for the project were to understand and predict mental health risk in users of the internet forum website Reddit, initially at the wider user base level before changing to user individual users.

I extracted post data via SQL from a BigQuery data repository.

Project MKI files refer to EDA looking into a potential time series analyses of number of posts to mental health related subreddits based on sentiment & clustering of daily news stories from the major Reddit news forum. Unfortunately data was restricted to news headlines only and was thus limited in terms of predictive potential.

I then decided to pivot to looking into user behaviour in regards to posting to other forums and whether this can be used to predict a user posting to mental health related forums (as a proxy for mental health risk). The overall sample was significantly skewed (approx. 3% of users posted to mental health related forums) and thus I applied a number of under and over sampling methodologies to the train set to improve the predictive of my models. 
