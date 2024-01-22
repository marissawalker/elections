# Predicting 2020 Elections

This project creates and compares several machine learning models to predict the 2020 US election winner by county, based on a range of demographic and economic data for each county. This was created as the final project for Supervised Machine Learning: Classification, an IBM course offered through Coursera as part of the IBM Machine Learning Professional Certificate.

## Project Objectives
I attempt to classify each county in the US based on which candidate in 2020 carried the majority vote in that county, based on various data about the county's demographics and economics. I create models trained with and without results from previous election years, comparing the results from various classification techniques. 

Although I would like to get models that are good at predicting, I'm also interested in seeing what insights can be gleaned from the models, by seeing what features seem to be the most important, and by looking at misclassifications. This type of analysis is useful for politicians to know what factors are connected with one party or the other dominating in a locality.

## Data Preparation, Cleaning, and Exploration

I searched online for compilations of county-level data set that include the election winners from 2020. I chose this one, which compiles data from a lot of different sources: https://www.kaggle.com/datasets/evangambit/us-county-data/data

Link to the exact data file:
https://raw.githubusercontent.com/evangambit/JsonOfCounties/master/counties.csv

### Column Selection

The raw data includes MANY columns, so for the purpose of simplifying the data (as well as removing columns with many null entries), I reduced the data by excluding several categories of columns. I removed:
- Industry columns (related to specific breakdown of different industries)
- Covid data
- Health information
- Police Shootings
- Deaths

The remaining columns include several different types:
- Geographic data (e.g. latitude, longitude, weather)
- Race and gender demographics (separate columns for each race/gender breakdown)
- Education levels
- Ages (Different columns for age groups)
- Unemployment, income, poverty level
- Cost of Living
- Total population in different years
- Election results from 2008, 2012, 2016, and 2020 (the target)

### Null Data
The data had about 30 rows with null data in the election results, so I removed these entries. After this, there was no remaining null data.

### Creating Election Winner Columns
The raw data just had the number of votes for each party in separate columns, so I created new 'winner' columns defining which party won the majority votes in each county. Then I removed the columns with the number of votes for each party. 

### Exploratory Data Analysis
I created pair plots and box plots to explore the features before building any models. It is evident from these plot that there are some noticeable distinctions between counties that voted majority blue vs red. (See the notebook for plots) 

### Creating Training and Testing Sets
Because the the data is fairly imbalanced (many more counties voted for the GOP candidate, even though the popular vote was decisively Democrat), I used the stratify split when creating training and testing sets. Then I scaled the data, since I knew I would want to apply regularization in some of my models.

I also created two versions of the data - one including previous election year winners and one excluding them. I assumed that using the previous winners might get a more predictive model, but I wanted to see what other features would be important without relying on the previous results.

# Models

I created models with three different main techniques: logistic regression, decision trees, and random forest. I also compared these models to the baseline guess of just predicting that 2020 would be identical to 2016 (a reasonably accurate model).

I tried the default versions from sklearn and then used GridSearchCV to choose hyperparameters. See the jupyter notebook for more details on each model.

# Key Insights and Findings

In terms of model metrics as determined by the test set, the highest across the board is using random forest including previous election years (and using LogisticCV did pretty well also). This model also had the lowest number of misclassified counties in the test set. It has a slightly lower precision but higher recall than just copying the 2016 election column. 

While the random forest model with the election columns was the best on prediction, it was also useful to look across multiple models to see some common features that kept coming up. Two features jumped out at me that were found to be important by several models:
- The fraction of white males in the population 
- The percentage of the population with Bachelors+ education

These two features (race and education) are quite commonly mentioned as correlated with voting patterns, so I wasn't surprised to see them showing up again and again. Other race/gender columns also showed up quite often, and surprisingly the Age 10-14 column showed up a couple of times (but not consistently in all models).

# Next Steps

There were some common struggles in all of these models, particularly in recall (getting all the Dem counties predicted). That weakness could by applying some of the resampling techniques to create a more balanced set of data (downsampling, upsampling, combinantion of both)

None of the models actually did much better at prediction than just copying 2016. They were still useful for drawing some insights on correlated factors. I could try including additional features, such as the health and covid columns that might be interesting. I know that religion is also often mentioned as a factor, so it would be interesting if I could find county-level data related to religion (which was not part of any of these data columns). I could also introduce some combinations of columns, as it would make sense that the features can be interconnected in various ways that would contribute to voting patterns. 

I would be interested in focusing on a different classification question: Did the county change its majority from 2016 to 2020? Do those counties that flipped have anything in common? (Or four classes: voted Dem both times, voted GOP both times, flipped from Dem to GOP, flipped from GOP to Dem) This would be a very imbalanced data set, so I would need to apply resampling techniques. 
