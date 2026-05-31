# UCSD ExtraSensory User Working Patterns

by: Yufei Huang (wuliuqi-123@outlook.com)

This is the final project for DSC80 created by Yufei Huang at UCSD. It uses the ExtraSensory Data collected for UCSD users and investigates the relationships between the users working patterns and sleeping, partying, and daily exercise patterns. This project creates a model to predict working patterns given the information mentioned above.

# Introduction

The dataset used for this project was collected on UCSD campus, containing the smartphone and smartwatch telemetry from 60 different users on average over 20 days. It was originally built to see how well mobile devices can recognize everyday behavioral context. For each user, it contains tens of thousands of samples, usually taken in the interval of minutes. Every example contains measurements from sensors (from the user's personal smartphone and from a smartwatch that the collector has provided), and most examples also have context labels self-reported by the user, which is what this project is focusing on. After observing the labels that has been self reported by the users, I've decided to investigate on the question:<mark>How is the sleeping time, working time, and entertainment time correlated to each other?</mark>, though later on I added another feature of daily activity times to make the correlations more interpretable. This is an interesting and worth discussing question. Knowing that I have all the sleeping, working and entertainment patterns of various users of UCSD, it allows me to predict the users working habits and patterns if we know their sleeping or entertainment pattern, which can be useful when assessing someones working/studying abilities. It is important to point out that not only do we know whether the person is sleeping or working at some point of the day, but we can see patterns of it, as the data is collected in intervals of minutes.

The whole dataset containing information about all 60 users has 377346 rows and 279 columns. The columns that I'm interested in are all boolean columns that can be briefly categorized into the following 4 categories:
- Time of day of report: 4 columns that splits the day into 4 sections of 6 hours starting from 12AM, each containing 1 or 0 representing when this information was reported/collected.
- Sleeping: a single column that is self-reorted by the user containing 1 or 0 representing whether this user is sleeping or not.
- Working: a list of 4 columns including \[Lab work, In class, In a meeting, Computer work\] which are columns that I believe can represent whether this user is working or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.
- Partying: a list of 3 columns including \[Drinking alcohol, At a party, At a bar\] which are columns that I believe can represent whether this user is partying or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.


# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

To make sure that my data only contains columns I needed, I first filtered and kept only the columns described in the Introduction section along with the user_id column, which will be important for later analyzing each user's activity patterns. After filtering, there are two steps that I conducted to make sure my dataset is ready for analysis and hypothesis testing related to my investigation question.

1. Filling in all the missing values with imputation:
Since all these columns that I'm interested in are self-reported labels other than the time_of_report, there were plenty of missing values in all these labels. These missing values can happen due to various reasons, such as the user forgetting to report, the user was not doing that activity and decided not to report 0, or simply the device that the user uses is out of battery. Because this is just a first stage cleaning, for simplicity I will assume that all missing values occur because the user was not doing that activity at that time, so I imputed 0 into the corresponding cell.

2. Grouping information together based on categories:
Since I'm interested in whether the user is working/partying/sleeping or not, I decided to create two new columns "Working" and "Partying" which contains true or false, based on if any of the working labels or partying labels were self-reported to be true for that row. Since all of the activities that I'm interested in are parallel activities, meaning that a user can only be doing one at a time, I've also created two new columns "Work" and "Party" that contains the actual event the user is doing if "Working" or "Partying" is true, and contains "Not_Working" or "Not_Partying" if "Working" or "Partying" is False. This way I've kept the raw data of what actual activities the user is doing, while making the information a lot more interpretable and clean. I've also cleaned the time of report labels into 1 column that contains the time range of the report.

Below is the head of my cleaned_df Data Frame:

| user_id     						  | Time_of_report  | Sleeping    | Working	| Work        | Partying    | Party       |
| -------------------------			  | --------------- | ----------- | --------| ----------- | ----------- | ----------- |
| A7599A50-24AE-46A6-8EA6-2576F1011D81| 12-18			| False		  | True    | IN_A_MEETING| False       | Not_Partying|
| A7599A50-24AE-46A6-8EA6-2576F1011D81| 12-18        	| False 	  | True    | IN_A_MEETING| False       | Not_Partying|
| A7599A50-24AE-46A6-8EA6-2576F1011D81| 12-18        	| False 	  | True    | IN_A_MEETING| False       | Not_Partying|
| A7599A50-24AE-46A6-8EA6-2576F1011D81| 12-18        	| False 	  | True    | IN_A_MEETING| False       | Not_Partying|
| A7599A50-24AE-46A6-8EA6-2576F1011D81| 12-18        	| False 	  | True    | IN_A_MEETING| False       | Not_Partying|

## Univariate Analysis

## Bivariate Analysis

## Interesting Aggregates

# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis