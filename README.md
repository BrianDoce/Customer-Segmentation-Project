# Customer-Segmentation-Project
A project I created implementing customer segmentation using an artificial dataset from Kaggle.

Author: Brian Docena

# Introduction
The motivation for this project was during class when I learned K-Means Clustering. I thought the concept of creating clusters based on points' feature values was simple, but interesting and I thought about how it would be used in a real life scenario. This situation led me to want to implement clustering based on customer data. It would be hard to come across a dataset for this application because of privacy concerns, so I just searched for data on Kaggle that had interesting features. Sadly, this dataset was artificially generated so the insights and analysis were not as interesting as I wanted it to be. ecommerce_customer_data_large is the data from Kaggle that was artificially generated, which 
contains 250,000 rows and the following columns:

| Column | Description |
| ----------- | ----------- |
| 'Customer ID' | ID Number of the Customer |
| 'Purchase Date' | The Date and Time of the Purchase |
| 'Product Category' | The Type of Category the Product Belongs to |
| 'Product Price' | The Price of the Product |
| 'Quantity' | Amount the User Bought of the Product |
| 'Customer Age' | The Age of The Customer |
| 'Returns' | Values were assigned 1 or 0 where 0 represented No Return and 1 represented Return |
| 'Customer Name' | Name of the Customer |
| 'Age' | Age of the Customer |
| 'Gender' | Gender of the Customer |
| 'Churn' | Values were assigned 0 if they were retained or 1 if they were churned |

# Data Cleaning and Exploration
Since this dataset was artificially generated, there wasn't much needed to do for data cleaning. However, I did transform the data with the following steps:
1. I used probabilistic imputation on the return column to fill missing values.
    - I saw that the only column that had null values were the returns column. To handle this I wrote code to see how the values were distributed. I saw that the returns were randomly distributed between no return and returned. I thought the best decision was to fill in the return column using imputation, keeping the randomness intact.

2. I renamed the columns so that instead of spaces there were underscores.
    - This was just a personal preference. I am used to words being separated by underscores instead of spaces.

3. I dropped Customer_Name and Age columns.
    - I thought the Customer_Name was not valuable and I would have done this for privacy concerns. The Age column was repeated, so I dropped it.

4. I used the Purchase_Date column to create a new column (Season) that represented the season a customer made their purchase.
    - I thought that 

The final dataframe has 250,000 rows and 13 columns.
(Note: the following visualization is a portion of the columns)

|   Customer_ID | Purchase_Date       | Product_Category   |   Product_Price |   Quantity |   Total_Purchase_Amount | Payment_Method   |   Customer_Age |   Returns | Customer_Name   |   Age | Gender   |   Churn |
|--------------:|:--------------------|:-------------------|----------------:|-----------:|------------------------:|:-----------------|---------------:|----------:|:----------------|------:|:---------|--------:|
|         44605 | 2023-05-03 21:30:02 | Home               |             177 |          1 |                    2427 | PayPal           |             31 |         1 | John Rivera     |    31 | Female   |       0 |
|         44605 | 2021-05-16 13:57:44 | Electronics        |             174 |          3 |                    2448 | PayPal           |             31 |         1 | John Rivera     |    31 | Female   |       0 |
|         44605 | 2020-07-13 06:16:57 | Books              |             413 |          1 |                    2345 | Credit Card      |             31 |         1 | John Rivera     |    31 | Female   |       0 |
|         44605 | 2023-01-17 13:14:36 | Electronics        |             396 |          3 |                     937 | Cash             |             31 |         0 | John Rivera     |    31 | Female   |       0 |
|         44605 | 2021-05-01 11:29:27 | Books              |             259 |          4 |                    2598 | PayPal           |             31 |         1 | John Rivera     |    31 | Female   |       0 |

# Exploratory Data Analysis
For analysis, I was disappointed with the results. Since the data was artificially generated, the analysis was not as insightful as it could have been.

The first thing I did was to see the statistics on the numeric columns of this dataset:

|                       |   count |       mean |        std |   min |   25% |   50% |   75% |   max |
|:----------------------|--------:|-----------:|-----------:|------:|------:|------:|------:|------:|
| Product_Price         |  250000 |  254.743   |  141.738   |    10 |   132 |   255 |   377 |   500 |
| Quantity              |  250000 |    3.00494 |    1.41474 |     1 |     2 |     3 |     4 |     5 |
| Total_Purchase_Amount |  250000 | 2725.39    | 1442.58    |   100 |  1476 |  2725 |  3975 |  5350 |
| Age                   |  250000 |   43.7983  |   15.3649  |    18 |    30 |    44 |    57 |    70 |

Here we can see that the average transaction is around $255 and customers usually buy 3 items.

Next, I wanted to see the amount that customers bought from each category and the payment method used. Sadly, because of the nature of the dataset, the values within the columns were evenly distributed.

Here is the amount bought for each category:

![Amount Bought For Each Category](image.png)

Here is the amount of customers that use each payment method:
![Amount Used For Each Payment Method](image.png)

Continuing with my analysis, I thought that it would be useful to use the purchase date of customers to see the season each customer bought their item. This provides us with insight on what seasons are popular amongst customers.
![Count of Seasons](image-1.png)

Finally, I wanted to see how age correleated with how much a customer purchased in a single transaction.
![Age vs. Transaction](image-2.png)

Here we can see that there is a positive correlation between age and amount purchased in a single transaction.

# RFM Analysis
Since I was doing a customer segmentation project, I thought it was fitting to also do a RFM analysis on my dataset. RFM stands for recency, frequency, and monetray value. These values represent how recent customers bought their product, how often they buy, and how much they spend. This is useful in identifying valuable customers.

For this analysis I calculated recency based on today's date and a customer's most recent transaction date. I then used count to see how many transactions customers had and summed the total amount of their transactions.

This has led me to the result dataframe:

|   Recency |   Frequency |   Monetary |   Recency_Score |   Frequency_Score |   Monetary_Score | RFM_Group   |   RFM_Score_Total |
|----------:|------------:|-----------:|----------------:|------------------:|-----------------:|:------------|------------------:|
|       397 |           3 |       6290 |               2 |                 1 |                1 | 2-1-1       |                 4 |
|       181 |           6 |      16481 |               4 |                 4 |                4 | 4-4-4       |                12 |
|       331 |           4 |       9423 |               3 |                 2 |                2 | 3-2-2       |                 7 |
|       550 |           5 |       7826 |               1 |                 3 |                2 | 1-3-2       |                 6 |
|       533 |           5 |       9769 |               2 |                 3 |                2 | 2-3-2       |                 7 |

I then used regular expressions and a function to map labels accordingly onto customers' RFM Score.

Here is the final result:

![Count of Labeled Customers](image-3.png)

Here is the result as a pie chart:

![Pie Chart](image-4.png)
# Interesting Aggregates
For my aggregates, I decided to group by rating and get the median, max, and standard deviation for calories.
This allows me to see what type of values of calories each rating consists of. I chose not to include mean because 
I noticed that the max calories of each rating would not make mean a good indicator of the central value.

|   ('rating', '') |   ('calories (#)', 'median') |   ('calories (#)', 'max') |   ('calories (#)', 'std') |
|-----------------:|-----------------------------:|--------------------------:|--------------------------:|
|                1 |                        316   |                   17551.6 |                   757.481 |
|                2 |                        326.3 |                    7585   |                   526.058 |
|                3 |                        309.6 |                   13101.5 |                   547.385 |
|                4 |                        302   |                   16894.9 |                   487.187 |
|                5 |                        298.2 |                   45609   |                   580.018 |

# Assessment of Missingness
## NMAR Analysis
In my dataframe, the three columns that have a significant amount of missing values are description, rating, and review. I believe that the missingness of review is NMAR because the probability of the data being missing is tied 
to the value itself as people who experience positive or negative feelings with the recipe are more likely to 
write a review.

## Missingness Dependency
I wanted to see if the missingness of ratings depended on calories 

Null Hypothesis: The missingness of rating does not depend on calories.

Alternate Hypothesis: The missingness of rating does depend on calories.

Test Statistic: Absolute difference of average calories

Level of Signficance: 0.05

I ran a permutation test by shuffling the calories 500 times and each time I collected the absolute difference in the average calories of the groups: missing ratings and not missing ratings.

<iframe
  src="assets/calories-missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I calculated an observed statistic of 68.98 and a p-value of 0.0, which is less than the level of significance of 
0.05. This means that we reject the null hypothesis and it suggests that the missingness of rating does depend on calories.

# Hypothesis Testing
I conducted my hypothesis testing based on whether people rated low caloric recipes (recipes less than 500 calories) higher than high caloric recipes.

Null Hypothesis: The rating of high caloric recipes and low caloric recipes have the same distribution.

Alternate Hypothesis: The rating of low caloric recipes is greater than the rating of high caloric recipes.

Test Statistic: Difference in mean rating between low caloric and high caloric recipes

Level of Significance: 0.05

When conducting the hypothesis test, I used a permutation test because I wanted to see if the ratings of high caloric recipes and low caloric recipes were rated the same. I was curious on whether low caloric recipes would 
receive a higher average rating than high caloric recipes.

To perform my hypothesis test, I shuffled the rating 500 times and calculated the difference in the average rating 
of low caloric recipes and high caloric recipes each time. I then calculated the observed difference, which was 
0.017. This tells me that low caloric recipes in my sample were rated slightly higher than high caloric recipes. When I found the p_value, the value was 0.0. This causes us to reject the null hypothesis.

<iframe
  src="assets/highcalories-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


# Framing a Prediction Problem
For my prediction problem, I wanted to use regression to predict the amount of calories a recipe would have.
I chose to predict the amount of calories because calories are essential for people who have certain dietary needs 
or personal fitness goals they want to achieve.

Since I planned on using regression, I chose to use root mean squared error to evaluate my model because I thought 
it was more interpretable in the context of my problem as I can tell the average error my predicted calories was 
to the actual calories. My other option was using R^2^, but I felt like knowing the quality of my prediction line 
wasn't as meaningful.

At the time of the prediction, we wouldn't have access to the nutritional values. We would only have access to 
information about the recipe such as the number of steps, minutes take to prepare, etc.

# Baseline Model
For my baseline model, I used a linear regression model that uses three features: is_dessert (nominal), is_dietary (nominal), and minutes (quantitative). is_dessert and is_dietary are both nominal columns extracted from the tags column, which consists of 
True and False values. For my model I used one hot encoding for these columns and used the standardized version 
of minutes.

For testing of my model, I split my sample into training and testing sets. With this method, I calculated a 
root mean squared error of 586 for my training set and 568 for my testing set. I believe this model is terrible 
because for each recipe, its prediction is more than 500 calories away.

# Final Model
For my final model, I added several columns: average_rating, is_vegetarian, n_ingredients, and meal_time

## n_ingredients
I added n_ingredients because as I was doing bivariate analysis, I grouped n_ingredients and aggregated on calories and saw that at some point as the number of steps increased, the median calories was consistently high. I thought 
that this could be useful to predict calories.

## is_vegetarian
For this column I extracted if recipes had vegetarian within its tags. Recipes were given the values True or False, which I then one hot encoded. I believed that this feature would improve my model because recipes labeled with vegetarian should have lower calories compared to ones that aren't.

## meal_time
For this column I extracted if the tags had breakfast, dinner, etc. within it and made it into a column so I could
use one hot encoding within it. I believed this feature would improve 

The modeling algorithm I chose was Lasso with hyperparameters of alpha = 100 and fit_intercept = True as it performed slightly better on the testing set. To find the best hyperparameters, I used GridSearchCV with a 
5-fold cross-validation to find the best alpha and fit_intercept parameters.

## Results
My final model performed slightly better than my baseline model with my final model having a root mean squared error 
of 558.74 on the testing set compared to my baseline model's root mean squared error of 565.26. My final thoughts about my model was that it was extremely difficult trying to improve my linear regression model to predict 
calories. I believe that a linear regression model was too simple to predict calories, which led to high bias and 
high root mean squared error.

# Fairness Analysis
For my fairness analysis, I wanted to evaluate my model's root mean squared error parity. To do this, I wanted to 
see if it predicted both high caloric recipes and low caloric recipes the same. 

Null Hypothesis: The model is fair. Its RMSE for high caloric recipes and low caloric recipes are roughly the same.

Alternative Hypothesis: The model is unfair. Its RMSE for high caloric recipes is not the same for low caloric recipes.

Test Statistic: The absolute difference in Root mean Squared Error for the two groups

Significance Level: 0.05 

p-value: 0.00

Conclusion: For my fairness analysis, I observed a p value of 0.00, which causes me to reject the null hypothesis.

<iframe
  src="assets/fairness-dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>











