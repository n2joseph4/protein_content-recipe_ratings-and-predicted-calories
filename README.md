# Analysis of Protein Content, Recipe Ratings, and Prediction of Calorie Amounts

by Nathan Joseph (n2joseph@ucsd.edu)

---
## Introduction

The dataset used for this project was modified from raw data obtained by scraping recipes and reviews since 2008 on the food.com website. On the food.com website, users post recipes with a unique ID number for that recipe, and other users can rate and review the recipe. Using this data, I will aim to answer the following pressing question: Do recipes for foods with high protein content as measured by higher percent daily value have higher ratings and can the amount of calories for a given recipe be predicted by using percent daily value of protein and the recipe’s rating or by the engineered features of protein density and rating per amount of steps to make the recipe? Answering this question will help in understanding how well protein is prioritized and regarded in recipes. Furthermore, this question will address how recipes should be selected based on calorie prediction, which is useful for dietitians, bodybuilders, and the average health-conscious person. The finalized dataframe has 211017 rows and 24 columns. Of the 24 columns, the most pertinent ones are protein_PDV, calories, rating, average rating per recipe, description, and date. The date at which the recipe was posted is provided in the date column, its corresponding rating and recipe description are present in the rating and description columns respectively. The original dataframe nutrition column contained the nutrition information in the form of a list: [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)], where PDV stands for percentage of daily value. In the final dataframe, this column was cleaned so that each value in the list has its own column and of those columns, the most important are calories and protein_PDV. These columns contain the respective calories and protein by percent daily value respectively. Lastly, the average rating per recipe column contains the average recipe rating for each respective recipe calculated based on the values in the rating column.
Data Cleaning and Exploratory Data Analysis

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
></iframe>The univariate plot of protein in terms of percent daily value shows the distribution of the amount of protein in the recipes with regards to a 2000 calorie diet. The box plot shows a right skew  and 50% of the distribution of protein is less than 50% of the recommended percent daily value of protein intake.  

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



## Framing a Prediction Problem



## Baseline Model



## Final Model



## Fairness Analysis

