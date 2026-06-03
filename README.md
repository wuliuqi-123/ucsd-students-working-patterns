# UCSD ExtraSensory User Working Patterns

by: Yufei Huang (wuliuqi-123@outlook.com)

This is the final project for DSC80 created by Yufei Huang at UCSD. It uses the ExtraSensory Data collected for UCSD users and investigates the relationships between the users working patterns and sleeping, partying, and daily exercise patterns. This project creates a model to predict working patterns given the information mentioned above.

# Introduction

College students constantly balance academic work, sleep, and leisure activities. Understanding how these behaviors interact can provide valuable insight into student lifestyles and time-management patterns. With the increasing availability of wearable devices and mobile sensing technologies, behavioral datasets allow researchers to study these patterns using real-world observations rather than relying entirely on surveys.

This project uses the UCSD ExtraSensory dataset, which contains sensor-based and self-reported information about participants' daily activities. This dataset contains the smartphone and smartwatch telemetry from 60 different users on average over 20 days. After cleaning and aggregating the data, the dataset provides measures of sleeping, working, partying, exercising, and other behavioral patterns. Observing the labels that has been self reported by the users, I've decided to investigate on the question:<mark>How is the sleeping time, working time, and entertainment time correlated to each other?</mark>, though later on I added another feature of daily activity times to make the correlations more interpretable. The primary goal of this project is to investigate whether students' sleeping habits, entertainment activities, and overall daily behavior patterns are associated with the amount of time they spend working. Specifically, we examine how sleep duration, party participation, and activity frequency relate to working time and whether these behavioral characteristics can be used to predict students' average working schedules. Understanding these relationships may provide insight into how students allocate their time across competing activities and whether daily behavioral routines contain meaningful information about productivity-related outcomes.

The whole dataset containing information about all 60 users has 377346 rows and 279 columns. The columns that I'm interested in are all boolean columns that can be briefly categorized into the following 4 categories:
- Time of day of report: 4 columns that splits the day into 4 sections of 6 hours, `\[0-6, 6-12, 12-18, 18-24\]` each containing 1 or 0 representing when this information was reported/collected.
- Sleeping: a single column that is self-reorted by the user containing 1 or 0 representing whether this user is sleeping or not.
- Working: a list of 4 columns including `\[Lab work, In class, In a meeting, Computer work\]` which are columns that I believe can represent whether this user is working or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.
- Partying: a list of 3 columns including `\[Drinking alcohol, At a party, At a bar\]` which are columns that I believe can represent whether this user is partying or not. Each column contains 1 or 0 that is self-reported by the user, representing if the user is performing this activity or not.


# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

To make sure that my data only contains columns I needed, I first filtered and kept only the columns described in the Introduction section along with the user_id column, which will be important for later analyzing each user's activity patterns. After filtering, there are two steps that I conducted to make sure my dataset is ready for analysis and hypothesis testing related to my investigation question.

1. Filling in all the missing values with imputation:
Since all these columns that I'm interested in are self-reported labels other than the time_of_report, there were plenty of missing values in all these labels. These missing values can happen due to various reasons, such as the user forgetting to report, the user was not doing that activity and decided not to report 0, or simply the device that the user uses is out of battery. Because this is just a first stage cleaning, for simplicity I will assume that all missing values occur because the user was not doing that activity at that time, so I imputed 0 into the corresponding cell.

2. Grouping information together based on categories:
Since I'm interested in whether the user is working/partying/sleeping or not, I decided to create 3 new columns `Sleeping`, `Working` and `Partying` which contains true or false, based on if any of the working labels or partying labels were self-reported to be true for that row. Since all of the activities that I'm interested in are parallel activities, meaning that a user can only be doing one at a time, I've also created 3 new columns `Sleep`, `Work` and `Party` that contains the actual event the user is doing if `Sleeping` `Working` or `Partying` is true. The other two possible values in these 3 columns are "MISSING" and "Not_Sleeping/Working/Partying" corresponding to False values in `Sleeping`, `Working` and `Partying` column. It contains "MISSING" if all the label columns in that category are missing, and otherwise it be "Not_Sleeping/Working/Partying". This way I've kept the raw data of what actual activities the user is doing, while making the information a lot more interpretable and clean. I've also cleaned the time of report labels into 1 column that contains the time range of the report.

Below is the head of my `cleaned_df` Data Frame:

<iframe
  src="assets/cleaned_df_table.html"
  width="600"
  height="550"
  frameborder="0">
</iframe>

## Univariate Analysis

I will perform univariate analysis on the `Party` and `Work` column, observing and analyzing the proportion of each activity in each category.

### Univariate Analysis of `Party`
<iframe
  src="assets/uni_party_dis.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>
The pie diagram shows that more than 96% of the Party activities are marked as "MISSING", meaning the data is missing for some of the 3 partying labels in the category. This shows a signal that the data about Party can be unreliable as the sample size is so small, it questions whether useful conclusions can be drawn out of such a small sameple size. Note that it is possible to deselect the "MISSING" section by clicking on the legends of the graph, and see that "Not_Partying" and "Drinking_Alcohol" has the largest proportion out of the partying activities. It still shows that even out of reported information, most user's in this dataset don't like to party.

### Univariate Analysis of `Work`
<iframe
  src="assets/uni_work_dis.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>
The pie diagram shows that around 85% of the Work activities are marked as "MISSING", meaning the data was missing for some of the 4 working labels in the category. Though it still show a small sample size in proportions, but recognizing that we have over 370,000 rows in the dataset, these information should give me some reliability to draw useful conclusions. Note that other than "MISSING", "Computer Work" is the activity that has the largest propotion, having a larger proportion even than "Not_Working". This shows that most users in this dataset have lots of computer work to do, showing some working patterns related to UCSD being such a large engineering school.

## Bivariate Analysis

I will perform bivariate analysis on the time of report column and the categorical activity columns. I will perform 3 bivariate analysis in total, each showing the proportion of of time sections for one of each of `Partying`, `Working` and `Sleeping`. For convinience and simplicity of interpretation, I will combine the results of the 3 bivarate analysis into 1 graph and analyze them together to observe interesting patterns and draw useful conclusions.

<iframe
  src="assets/bi_prop_time_dis.html"
  width="600"
  height="500"
  frameborder="0"
></iframe>

For `Working`, we see that most of the working activities happen during 12-18 or 6-12, showing the healthy working patterns of most of the users in UCSD. <br>
For `Sleeping`, we see that around 66% of the sleeping reports shows sleeping during 0-6, which is a normal sleeping time. However, it is surprising that around 20% of the sleeping reports happens during 6-12, more than what is reported to be during 18-24, which shows a late sleep, late wake pattern among UCSD users. <br>
For `Partying`, we see that 75% of the partying activities happen during 18-24, instead of 0-6, which is what most night clubs are the most popular. This shows that UCSD students are less into parties that happen late at night.<br>

Together, we see a clear split of time of report of the 3 categories. Only 6-12 is a time period with a pretty mixed types of activities done. This shows a clear pattern of activities of the users during times of day, which can be useful to draw conclusions.

## Interesting Aggregates

Below are some interesting aggregates that can be done with the `cleaned_df`:
<iframe
  src="assets/interesting_agg_table.html"
  width="650"
  height="250"
  frameborder="0">
</iframe>

To better understand the temporal patterns of the data, I grouped observations by `time-of-report` and computed the mean and total count of sleeping, working, and partying activities. Because the activity variables are binary indicators, their means represent the proportion of reports associated with each activity during a given time period. The results show clear behavioral patterns: sleeping activity is concentrated between midnight and 6 AM, working activity peaks between noon and 6 PM, and partying activity is most common during the evening. These findings suggest that `time-of-day` is strongly associated with user behavior and support the use of temporal activity features in subsequent predictive modeling.

# Assessment of Missingness

## NMAR Analysis

There are many missing values in most of my columns other than the time_of_report, as everything else are label information that is self-reported by the user. One column that I believe to be **NMAR** is the `label: SLEEPING` column. This column is self-reported by the user, and unlike other label columns, a user can't be reporting whether they are sleeping or not while they are asleep. So I would assume that the reported 1s in the sleeping column is reported by the user after they wake up, and they recorded the time that they believe they were asleep. Therefore missingness in this column could've happened because the user forgot to report that they were asleep after they woke up, because they couldn't report it while they were doing it. Therefore the missingness in this column should be independent of any other columns but only be depended on its own value (whether the user was actually sleeping or not).

An additional data I could've obtained to make the sleeping column **MAR** is `discrete: battery_state: missing` which indicates that the device it out of battery, which means that the user couldn't report whether or not they are sleeping or not, which is another reason that leads to missingness. This way the missingness in `label: sleeping` will be dependent on this column.

## Missingness Dependence Analysis

To analyze missingness dependency, I will focus on the `Party` column, where I've marked out "MISSING" to reports that are missing for all labels in the partying category during my data cleaning stage. I will analyze the missingness dependency of `Party` to `Time_of_report` column and the `Sleep` column. Note that to make the analysis of the `Sleep` column possible, I dropped all the missing values and transmitted all the "Not_Sleeping" and "Sleeping" values to boolean values only for this test.

### Time of report Missingness
First I will perform a permutation test on `Party` and `Time_of_report`, and see if the missingness in `Party` depends on `Time_of_report`

**Null Hypothesis**: The missingness in the `Party` column is independent of the `Time_of_report` column

**Alternative Hypothesis**: The missingness in the `Party` column is dependent of the `Time_of_report` column

**Significance level**: 0.05

**Test statistic**: absolute difference in the mean time of report
<iframe
  src="assets/permutation_dis_missingness_time.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

After performing a permutation test, we found an observed absolute difference in the mean time of report to be 0.572 and a p-value of 0.0. As shown by the distribution graph, it suggests that our observed statistic is highly unprobable under the null hypothesis. Since the p-vlaue is less than the significant level, we reject the null hypothesis that the missingness in the `Party` column is independent from the `Time_of_report` column. This makes the missingness in the `Party` column likely MAR when we only have two columns, `Party` and `Time_of_report`.

### Sleep Missingness
Next I will perform a permutation test on `Party` and `Sleep"`(With all missing values droped), and see if the missingness in `Party` depends on `Sleep`.

**Null Hypothesis**: The missingness in the `Party` column is independent of the `Sleep` column

**Alternative Hypothesis**: The missingness in the `Party` column is dependent of the `Sleep` column

**Significance level**: 0.05

**Test statistic**: absolute difference in the mean sleeping rate
<iframe
  src="assets/permutation_dis_missingness_sleep.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

After performing a permutation test, we found an observed absolute difference in the mean sleep rate to be around 0.006 and a p-value of about 0.77. As shown by the distribution graph, it suggests that our observed statistic is highly probable under the null hypothesis. Since the p-vlaue is greater than the significant level, we fail to reject the null hypothesis that the missingness in the `Party` column is independent from the `Sleep` column. This makes the missingness in the `Party` column likely NMAR when we only have two columns, `Party` and `Sleep`.

# Hypothesis Testing

I will be testing whether there is linear association between user's average sleeping report time and average working report time. The relevant columns for this test in the `cleaned_df` are `\["user_id", "Time_of_report", "Sleeping", "Working"\]`. Where I will compute the average report time of each user when `Sleeping` is true and when `Working` is true, and see if there is a linear association. Because I'm grouping my cleaned_df, to make my hypothesis testing clearer, I will show the first few rows of the grouped `user_df` for this hypothesis test.
<iframe
  src="assets/grouped_user_df_hypo_test.html"
  width="650"
  height="250"
  frameborder="0">
</iframe>

**Null Hypothesis**: There is no linear association between users’ average sleeping report times and average working report times.

**Alternative Hypothesis**: There is a linear association between users’ average sleeping report times and average working report times.

**Test Statistic**: The correlation coefficient between the users' average sleeping report times and average working report times.

**Significance Level**: 5%

I first computed the observed statistic by grouping the `cleaned_df` by `user_id`, calculated the mean report time separately for when `Sleeping` is true and when `Working` is true, then calculated the correlation coefficient using the OLS estimator. A graph to show the result of the observed statistic is as follow, and the observed statistic is a correlation coefficient of -0.0514 (3 Sig. Fig.)
<iframe
  src="assets/observed_corr_hypo_test.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

Then I performed a permutation test of 1000 simulations to test my pair of hypothesis, and got a p-value of 0.696. With a significance level of 5%, I would surprisingly fail to reject the null hypothesis and suggest evidence of no association between sleeping and working schedules accross users. The plot below shows the observed statistic compared to the empirical distrubtion of correlations from the permutation test. It shows that the observed statistic lies very close to the center.
<iframe
  src="assets/hypo_test_distribution.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

# Framing a Prediction Problem

From the last section, we surprisingly found that evidence suggest that there might be no linear correlation between the users' average sleeping report time and working report time. However, I still believe that there actually is a relationship, but the reason that my hypothesis test suggests no relationship is because there were too many ommited variables that could also affect the average working report time of the user.

To address my question and surprise, I will build a model to predict the users' average working report time given the users' average sleeping report times, average partying report times and average daily activities report times, which will be a regression model. I chose mean_work_time as the response variable because working behavior is one of the most common and meaningful activities in the ExtraSensory dataset, and it is reasonable to investigate whether users' sleeping, partying, and daily routines can help explain their working schedules. To clearly define my prediction problem, I will define the categories `Sleeping`, `Working`, `Partying` and `Daily Activities` as following:

- Sleeping report times is the measure of the average time of sleeping.

- Working report times include the reported times of categories including `\[LAB_WORK, IN_CLASS, IN_A_MEETING, COMPUTER_WORK\]`.

- Partying report times include the reported times of categories including `\[DRINKING__ALCOHOL_, AT_A_PARTY, AT_A_BAR\]`.

- Daily Exercise report times include the reported times of categories including `\[COOKING, BATHING_-\_SHOWER, DOING_LAUNDRY, CLEANING, WASHING_DISHES, SURFING_THE_INTERNET, EATING, TOILET\]`.

I will evalute my model performance from 2 perspective, one evaluating how well my model fits the data that I used to train it using the coefficient of determination (R²). R² measures the proportion of variation in the response variable that is explained by the predictors. The other perspective is evaluating how well my model can predict on unseen data by using a five-fold cross validation method and finding the mean R². Because this is a regression problem, R² is more appropriate than classification metrics such as accuracy, precision, or F1-score.

At prediction time, the model assumes that information about a user's average sleeping, partying, and daily activity reporting times is available, while the user's average working report time is unknown and must be predicted. Therefore, all predictors used in the model would be known before the prediction is made.

# Baseline Model

In order to implement my Baseline Model, I had to renew my `cleaned_df` and `user_df` showed previously. In order to clarify my process of building the baseline model, I will show the first few rows of the updated `cleaned_df` and `user_df` for this baseline model.

### Updated cleaned_df
<iframe
  src="assets/updated_cleaned_df_baseline.html"
  width="650"
  height="350"
  frameborder="0">
</iframe>

### Updated user_df
<iframe
  src="assets/updated_user_df_baseline.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

My baseline model will be a `LinearRegressor` model using the features `mean_sleep_time`, `mean_party_time` and `mean_daily_time` to predict the `mean_working_time` of the users. These features may help explain variation in students' working patterns and provide insight into how daily
activities are associated with working behavior. All 3 features used to predict are quantitative, as they are mean values of time of the day (an integer hour that is assumed to be the starting time of the `time_of_report column`）.

The baseline model achieved an in-sample R² of approximately 0.40, indicating that the selected features explain about 40% of the variation in users’ average working times. However, evaluation on unseen data produced substantially lower and often negative R² values. Five-fold cross-validation yielded a mean R² of approximately -3.30, suggesting that the model does not generalize well to new users. Note that the dataset contains only around 60 users after aggregation, which can lead to high variance in cross-validation estimates. However,this still indicates that average sleeping, partying, and daily activity times alone are insufficient predictors of average working time, and that additional features or more sophisticated models may be necessary. I would conclude that my current baseline model is not very good in predicting the target feature using the 3 features I have now. 

# Final Model

### Introduction of Final Model
For the final model, I attempted to improve upon my baseline linear regression model by engineering additional features that better describe each user's behavioral patterns.

The baseline model used only the mean times at which a user reported sleeping, partying, and daily activities to predict the user's mean working time. While these features captured when activities tended to occur, they did not describe how frequently the activities occurred or how consistent the user's schedule was.

To address this limitation, I created six additional features:

- `sleep_freq` – proportion of reports in which the user was sleeping.
- `party_freq` – proportion of reports in which the user was partying.
- `daily_freq` – proportion of reports in which the user was performing daily activities.
- `sleep_std` – standard deviation of sleeping times.
- `party_std` – standard deviation of partying times.
- `daily_std` – standard deviation of daily activity times.

The frequency features capture how often users engage in particular activities, while the standard deviation features measure schedule consistency. Users with highly regular schedules may have different working patterns than users whose activities occur at highly variable times.

Because many users never performed certain activities, some engineered features contained missing values. Rather than simply replacing it with zeros or removing those users and drastically reducing the dataset size, I used a `SimpleImputer` with a constant value of -1 to preserve the information that a particular activity never occurred for that user.

I trained a `RandomForestRegressor` and tuned its hyperparameters using `GridSearchCV` with 5-fold cross-validation. The hyperparameters searched were:

Number of trees (n_estimators)<br>
Maximum tree depth (max_depth)<br>
Minimum samples per leaf (min_samples_leaf)

The best hyperparameter combination found was:

n_estimators = 200<br>
max_depth = 2<br>
min_samples_leaf = 4

### Evaluation of Final Model
By using the best parameters that I got from using a `GridSearchCV` with 5-fold cross-validation, The final `RandomForestRegressor` model achieved:

R² = 0.133
MAE = 1.796
RMSE = 2.144

Compared to the baseline model, which achieved a negative R² on unseen test data, the final model produced a positive R² score. This indicates that the final model is able to explain some of the variation in users' working times and performs better than simply predicting the average working time for all users.

Although the improvement is modest, the results suggest that activity frequencies and schedule variability contain useful information about working behavior. The feature importance analysis supports this conclusion.

<iframe
  src="assets/importance_of_cols_graph.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>
As shown in the graph above, the most important features were:

- `daily_freq`
- `mean_sleep_time`
- `daily_std`
- `sleep_freq`
- `sleep_std`
This suggests that users' daily activity habits and sleeping patterns are more predictive of working times than partying-related variables. In contrast, `party_freq` and `party_std` contributed very little to the model's predictions.

Overall, the final model improves upon the baseline by incorporating richer behavioral features and by using a more flexible nonlinear learning algorithm. However, the relatively low R² value indicates that a large amount of variation in working time remains unexplained. This is likely because working behavior is influenced by many factors not captured in the available dataset. Additional features describing location, weekday versus weekend effects, or other contextual information may further improve predictive performance.

# Fairness Analysis

For fairness analysis of my final model, I will assess if my model is fair among high daily activity frequency users and low daily activity frequency users. We can observe from the feature importance plot from the previous section that `daily_freq` is the most important feature in predicting the `mean_work_time` of users. This means that different frequencies of doing daily activities amongst users can significantly impact my final model's prediction. Therefore, it is natural that I want to test whether my final model perform worse for users with less frequent daily activity records. I will break down the group of users to high-daily-frequency users and low-daily-frequency users based on the median of the daily frequency column.

**Null Hypothesis**: The model’s RMSE is the same for high-daily-frequency users and low-daily-frequency users. Any difference is due to random chance.

**Alternative Hypothesis**: The model’s RMSE is higher for one group than the other, meaning the model performs worse for that group.

**Significance Level**: 5%

**Test Statistics**: Absolute difference between the RMSE of high-daily-frequency users and low-daily-frequency-users

<iframe
  src="assets/fairness_hypo_dis_graph.html"
  width="650"
  height="650"
  frameborder="0">
</iframe>

The observed p-value was 0.428. Using a significance level of α = 0.05, we fail to reject the null hypothesis because the p-value is substantially larger than 0.05. Therefore, we do not find statistically significant evidence that the model's prediction error differs between high-daily-frequency users and low-daily-frequency users. Based on this fairness analysis, the model appears to perform similarly for the two groups with respect to RMSE. While this does not prove that the model is perfectly fair, it suggests that any performance differences observed between the two groups are small enough that they could reasonably be explained by random variation in the data.


