# Analysis of Protein Content, Recipe Ratings, and Prediction of Calorie Amounts

by Nathan Joseph (n2joseph@ucsd.edu)

---
## Introduction

The dataset used for this project was modified from raw data obtained by scraping recipes and reviews since 2008 on the food.com website. On the food.com website, users post recipes with a unique ID number for that recipe, and other users can rate and review the recipe. Using this data, I will aim to answer the following pressing question: Do recipes for foods with high protein content as measured by higher percent daily value have higher ratings and can the amount of calories for a given recipe be predicted by using percent daily value of protein and the recipe’s rating or by the engineered features of protein density and rating per amount of steps to make the recipe? Answering this question will help in understanding how well protein is prioritized and regarded in recipes. Furthermore, this question will address how recipes should be selected based on calorie prediction, which is useful for dietitians, bodybuilders, and the average health-conscious person. The finalized dataframe has 211017 rows and 24 columns. Of the 24 columns, the most pertinent ones are protein_PDV, calories, rating, average rating per recipe, description, and date. The date at which the recipe was posted is provided in the date column, its corresponding rating and recipe description are present in the rating and description columns respectively. The original dataframe nutrition column contained the nutrition information in the form of a list: [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)], where PDV stands for percentage of daily value. In the final dataframe, this column was cleaned so that each value in the list has its own column and of those columns, the most important are calories and protein_PDV. These columns contain the respective calories and protein by percent daily value respectively. Lastly, the average rating per recipe column contains the average recipe rating for each respective recipe calculated based on the values in the rating column.
Data Cleaning and Exploratory Data Analysis
## Assessment of Missingness

| name                                 |   rating |   average rating per recipe |   protein_PDV |   calories | date                |
|:-------------------------------------|---------:|----------------------------:|--------------:|-----------:|:--------------------|
| 1 brownies in the world    best ever |        4 |                           4 |             3 |      138.4 | 2008-11-19 00:00:00 |
| 1 in canada chocolate chip cookies   |        5 |                           5 |            13 |      595.1 | 2012-01-26 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2008-12-31 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2009-04-13 00:00:00 |
| 412 broccoli casserole               |        5 |                           5 |            22 |      194.8 | 2013-08-02 00:00:00 |

<iframe
  src="assets/protein-pdv-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis