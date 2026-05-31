# UCSD ExtraSensory User Working Patterns

by: Yufei Huang (wuliuqi-123@outlook.com)

This is the final project for DSC80 created by Yufei Huang at UCSD. It uses the ExtraSensory Data collected for UCSD users and investigates the relationships between the users working patterns and sleeping, partying, and daily exercise patterns. This project creates a model to predict working patterns given the information mentioned above.

# Introduction

The dataset used for this project was collected on UCSD campus, containing the smartphone and smartwatch telemetry from 60 different users on average over 20 days. It was originally built to see how well mobile devices can recognize everyday behavioral context. For each user, it contains tens of thousands of samples, usually taken in the interval of minutes. Every example contains measurements from sensors (from the user's personal smartphone and from a smartwatch that the collector has provided), and most examples also have context labels self-reported by the user, which is what this project is focusing on. After observing the labels that has been self reported by the users, I've decided to investigate on the question:==How is the sleeping time, working time, and entertainment time correlated to each other?==, though later on I added another feature of daily activity times to make the correlations more interpretable. This is an interesting and worth discussing question. Knowing that I have all the sleeping, working and entertainment patterns of various users of UCSD, it allows me to predict the users working habits and patterns if we know their sleeping or entertainment pattern, which can be useful when assessing someones working/studying abilities. It is important to point out that not only do we know whether the person is sleeping or working at some point of the day, but we can see patterns of it, as the data is collected in intervals of minutes.

The whole dataset containing information about all 60 users has 377346 rows and 279 columns. The columns that I'm interested in are all boolean columns that can be briefly categorized into the following 4 categories:
- Time of day of report: 4 columns that splits the day into 4 sections of 6 hours starting from 12AM, each containing 1 or 0 representing when this information was reported/collected.
- Sleeping: a single column that is self-reorted by the user containing 1 or 0 representing whether this user is sleeping or not.
- Working: a list of 4 columns including \[Lab work, In class, In a meeting, Computer work\] which are columns that I believe can represent whether this user is working or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.
- Partying: a list of 3 columns including \[Drinking alcohol, At a party, At a bar\] which are columns that I believe can represent whether this user is partying or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.


# Data Cleaning and Exploratory Data Analysis



# Assessment of Missingness

# Hypothesis Testing

# Framing a Prediction Problem

# Baseline Model

# Final Model

# Fairness Analysis