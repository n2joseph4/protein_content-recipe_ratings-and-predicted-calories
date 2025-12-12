# Analysis of Protein Content, Recipe Ratings, and Prediction of Calorie Amounts

by Nathan Joseph (n2joseph@ucsd.edu)

---
## Introduction

The dataset used for this project was modified from raw data obtained by scraping recipes and reviews since 2008 on the food.com website. On the food.com website, users post recipes with a unique ID number for that recipe, and other users can rate and review the recipe. Using this data, I will aim to answer the following pressing question: Do recipes for foods with high protein content as measured by higher percent daily value have higher ratings and can the amount of calories for a given recipe be predicted by using percent daily value of protein and the recipe’s rating or by the engineered features of protein density and rating per amount of steps to make the recipe? Answering this question will help in understanding how well protein is prioritized and regarded in recipes. Furthermore, this question will address how recipes should be selected based on calorie prediction, which is useful for dietitians, bodybuilders, and the average health-conscious person. The finalized dataframe has 211017 rows and 24 columns. Of the 24 columns, the most pertinent ones are protein_PDV, calories, rating, average rating per recipe, description, and date. The date at which the recipe was posted is provided in the date column, its corresponding rating and recipe description are present in the rating and description columns respectively. The original dataframe nutrition column contained the nutrition information in the form of a list: [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)], where PDV stands for percentage of daily value. In the final dataframe, this column was cleaned so that each value in the list has its own column and of those columns, the most important are calories and protein_PDV. These columns contain the respective calories and protein by percent daily value respectively. Lastly, the average rating per recipe column contains the average recipe rating for each respective recipe calculated based on the values in the rating column.

## Data Cleaning and Exploratory Data Analysis

| name                                 |   rating |   average rating per recipe |   protein_PDV |   calories | date                |
|:-------------------------------------|---------:|----------------------------:|--------------:|-----------:|:--------------------|
| 1 brownies in the world    best ever |        4 |                           4 |             3 |      138.4 | 2008-11-19 00:00:00 |
| 1 in canada chocolate chip cookies   |        5 |                           5 |            13 |      595.1 | 2012-01-26 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2008-12-31 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2009-04-13 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2013-08-02 00:00:00 |

In order to perform analysis using this dataframe, multiple data cleaning steps were performed. The first of which was to replace the rating of 0 with a NaN value. This was done since a rating of 0 indicates that the recipe was not given a rating yet and therefore by replacing a 0 with NaN, the average rating column can be unbiasedly created without being artificially lowered by the not missing at random missingness in the rating column. Next, the date column was converted to datetime objects for ease of use. The original dataframe nutrition column contained the percent daily value of calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates in the form of a list. In the final dataframe, this column was cleaned so that each value in the list has its own column so that it can be accessed for analysis without repetitive querying. Furthermore, all nutrition percent daily values lower than 0 and higher than 300 were excluded as these were deemed to be outliers in the data. Lastly, all rows containing NaN values were removed from the dataframe, as they accounted for only 14,292 entries, which is approximately 6% of the total data. 

<iframe
  src="assets/protein-pdv-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The univariate plot of protein in terms of percent daily value shows the distribution of the amount of protein in the recipes with regards to a 2000 calorie diet. The box plot shows a right skew  and 50% of the distribution of protein is less than 50% of the recommended percent daily value of protein intake.  

<iframe
  src="assets/protein-pdv-by-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The bivariate plot shows the distribution of protein in terms of percent daily value across different ordinal ratings. There is strict overlap of each of the 5 boxplots for each rating but when comparing medians, there seems to be a prominent peak at a rating of 4 in the distribution.


|   rating |   ('count', 'protein_PDV') |   ('median', 'protein_PDV') |   ('mean', 'protein_PDV') |   ('std', 'protein_PDV') |
|---------:|---------------------------:|----------------------------:|--------------------------:|-------------------------:|
|        1 |                       2672 |                        14   |                   31.5921 |                  38.8018 |
|        2 |                       2262 |                        17.5 |                   33.0619 |                  36.1072 |
|        3 |                       6900 |                        19   |                   33.1591 |                  35.6244 |
|        4 |                      36099 |                        20   |                   32.6051 |                  33.8447 |
|        5 |                     163084 |                        17   |                   30.8553 |                  34.4322 |

This is an interesting pivot table that shows the counts, medians, mean, and standard deviations of the percent daily value of protein in recipes across different ratings. Notably, as the recipe ratings increase, the number of recipes increases correspondingly but the standard deviation of protein percent daily value decreases. The inferences made in the previous bivariate plot is confirmed through the analysis of the median in this table which clearly shows the peak of the protein percent daily value distribution at a rating of 4. The average ratings remain largely uniform across the different rating levels.

## Assessment of Missingness

I believe that the missingness in the rating column is NMAR (not missing at random) since the missingness may be dependent on the actual value of the rating that was to be given. Recipe reviewers will be less likely to report a lower and thus negative rating out of politeness and consideration of the recipe author’s time and efforts. If I could collect data on user interaction with the recipe in terms of clicks or views, and also whether the recipe was shared with others, I could use this data to make the missingness in the rating column as MAR since now I have observed variables to explain the relationship between missingness and lower ratings.

<iframe
  src="assets/protein-pdv-by-rating-missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After performing a permutation test to analyze the missingness in average rating per recipe column in relation to the protein_PDV column, the following results were obtained by using the KS statistic as the test statistic:
Observed KS statistic: 0.0235
Permutation-based p-value: 0.0760
The null hypothesis states that there is no statistical relationship between the missingness in the average rating per recipe column and the protein_PDV column. The alternative hypothesis states that there is an association between the missingness in the average rating per recipe column and the protein_PDV column. These results indicate that there is no strong evidence that the missingness in the average rating per recipe column depends on the protein_PDV column. This is so because the p-value is greater than the cutoff value of 0.05, which means that we fail to reject the null hypothesis. Therefore, since the missing ratings are independent of protein content, the missingness mechanism is more consistent with MCAR than MAR.


## Hypothesis Testing

Null Hypothesis: Recipes with higher protein content (higher protein_PDV) have similar ratings on average to recipes with lower protein content (lower protein_PDV).
Alternative Hypothesis: Recipes with higher protein content (higher protein_PDV) have ratings that differ from recipes with lower protein content (lower protein_PDV).
I chose the ANOVA F-statistic since I will be comparing the mean ratings between two groups, which are the recipes with higher protein content and the recipes with lower protein content. Since the F-statistic compares the between group noise to the within group noise, it is the appropriate measure for determining whether the average ratings of the two protein groups differ significantly. The significance level chosen for the p-value is 0.05 since it is a standard threshold that controls the amount of false rejections of the null hypothesis while also allowing for detection of  differences between the two groups. The results of the test are stated below:
Observed ANOVA F-statistic: 26.237
Permutation test p-value = 0.0000
Since the p-value is less than the chosen critical value of 0.05, the null hypothesis can be rejected and therefore there is evidence that average rating differs between high-protein and low-protein recipes. Furthermore, the large F-statistic indicates that the variability between group means is much larger than the variability within groups, as shown in the figure below.
<iframe
  src="assets/permutation-anova-f.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Framing a Prediction Problem

Prediction Problem: Predict the response variable of calories of a recipe, which was extracted from the original nutrition column of the dataframe, using engineered features or the protein_PDV and the rating of that recipe, all of which are known at the time of prediction. This is a regression problem and not a classification problem since I will be predicting a numerical value. The calories of a recipe variable was chosen in order to help dietitians, bodybuilders, coaches, and health conscious folks to choose recipes that will fit in their diet plans and calorie goals. The metric that I chose to evaluate my model is R2. This metric was selected because it summarizes the proportion of variance in recipe calorie content, the response variable, that is explained by my linear regression model.

## Baseline Model

The baseline model uses the features of protein_PDV and the rating of that recipe to predict the response variable, which is recipe calorie content. The model was trained using an 80/20 train test split and a 5-fold cross-validation was used during model evaluation to ensure that the R² result was not dependent on a particular split of the dataset. The recipe protein_PDV feature, which was originally extracted from the nutrition column, is a quantitative value that was column transformed with a standard scaler. The recipe rating feature is an ordinal value that was also column transformed with a standard scaler. Standardization was done so that coefficients can be compared directly in the baseline model. The baseline model achieved a R2 of 0.4855 which means that around 49% of the variance in recipe calorie content is explained by the baseline model. Therefore, this baseline model is not great to predict calorie content since more than half of the variation in calorie content is not accounted for which indicates that the model lacks important predictive features. 

## Final Model

In my final model, I used two new engineered features: recipe protein density and rating per step of recipe. The protein density of the recipe feature measures how quickly a recipe delivers protein with respect to the number of minutes it takes to cook, which can be a proxy for recipe complexity. Thus, this engineered feature was made by dividing the protein_PDV column by the minutes column so that each recipe now has a protein density value. The rating per step of a recipe relates the rating of a recipe to the number of steps that the recipe has, which also can be a proxy for recipe complexity. Thus, this engineered feature was made by dividing the rating column by the number of steps so that each recipe now has a rating per step column. I believe that these two features improved my models performance because they individually combined two aspects of my data and therefore lends more predictive power to my model. In actuality, since higher protein sources (meat, fish, etc…) take less time to cook, it can help predict calories since more protein and its accompanying fat content increases calorie counts. Furthermore, simple recipes with a lower number of steps may have higher ratings and therefore it may better predict total calories since the recipe will likely use calorie dense ingredients to enhance the flavor. 

For my final model, I chose a random forest algorithm since it captures nonlinear relationships between my engineered features of recipe protein density and rating per step of recipe and calorie content. I used a grid search with 5-fold cross-validation to test combinations of hyperparameters, which include the number of decision trees, the maximum depth of each decision tree, and the minimum samples required to split. The best combination of hyperparameters used 200 decision trees, no maximum depth, and a minimum of 5 samples required to split a node. Compared to the baseline linear model, the final random forest model achieved a higher R² of 0.5417, which shows a clear improvement in explaining the variance in recipe calories and thus resulting in higher prediction power.


## Fairness Analysis

As part of my fairness analysis, I split my data into two groups and tested whether my model is fair for both groups. I separated the recipes into two groups based on date, specifically the first group are recipes submitted before January 1, 2011, which are older recipes, and the second group are recipes submitted on or after January 1, 2011, which are newer recipes. I used RMSE of predicted calories as my evaluation metric. My null hypothesis is that my final model’s RMSE is similar for recipes posted before January 1, 2011 and those recipes posted on or after January 1, 2011. My alternative hypothesis is that my final model’s RMSE differs between the two groups and so may have a slight bias. In this fairness analysis, I used the difference in RMSE between the two groups as my evaluation metric and a significance level of 0.05. My resulting p-value is 0.009 and since this is lower than my significance level of 0.05, I reject the null hypothesis and conclude that there is a significant difference in the RMSE of predicted calories between recipes posted before January 1, 2011 and recipes posted on or after January 1, 2011.
<iframe
  src="assets/rmse-diff-permutation-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>