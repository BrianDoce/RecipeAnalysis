# Relationship Between Calories and Ratings
This is the final project for the UCSD class DSC80. For this project, students were tasked to conduct an open-ended investigation using everything we learned.

Author: Brian Docena

# Introduction
When it comes to losing weight, the main thought that goes through peoples' minds are going months or years eating
bland, tasteless food. This is the main obstacle that holds people back from reaching their goals as they consume food that they absolutely dread. This problem is what interested me into finding the relationship between calories and ratings as I wanted to see if there was any validity to the idea that healthy food can't taste good. For this I used data from Food.com, an online recipe platform where users can submit their recipes, with its nutritional information, user ratings, and reviews. Recipes is a subset of data from this website, which 
contains 83782 rows and the following columns:


| Column | Description |
| ----------- | ----------- |
| 'name' | Recipe name |
| 'id' | Recipe ID |
| 'minutes' | Minutes to Prepare Recipe |
| 'contributor_id' | User ID who submitted this recipe |
| 'submitted' | Date recipe was submitted |
| 'tags' | Food.com tags for recipe |
| 'nutrition' | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| 'n_steps' | Number of steps in recipe |
| 'steps' | Text for recipe steps, in order |
| 'description' | User-provided description |


Interactions is another subset of the data containing 731927 rows and the following columns:


| Column | Description |
| ----------- | ----------- |
| 'user_id' | User ID |
| 'recipe_id' | Recipe ID |
| 'date' | Date of interaction |
| 'rating' | Rating given |
| 'review' | Date text |

Given these datasets, I wanted to break down the information in the nutrition column into separate columns because
I wanted to predict calories given the nutrition values. I also added another column called calories_type, which
grouped recipes into either one of two groups: High Calories (if calories were greater than or equal to 500) 
or Low Calories (if calories were less than 500). The most significant columns were calories (#) and rating. Along
with all the values in nutrition information for predicting amount of calories a recipe would have.

# Data Cleaning and Exploration
For data cleaning, I did the following steps:
1. Left merge the recipes and interactions datasets on id and recipe_id
    - This makes it easier to work with recipes and their corresponding ratings.

2. Fill all ratings of 0 with np.nan
    - I filled all ratings of 0 with np.nan because prior to filling them, 0 represented the recipes were missing ratings.
    This can cause problems with my analysis as 0 represents the lowest rating and can indicate that users didn't enjoy the recipe.

3. Added column 'average_rating'.

4. Broke the nutrition column down into a column for each of its values (calories (#), total fat (PDV), etc.)
    - I broke the column down into separate columns because I believed that the numerical values it held would be important
    for analysis.

5. Dropped nutrition column
    - I didn't think it was necessary after breaking it down.

6.  Converted 'submitted' and 'date' columns into datetime object.
    - I wasn't sure if I would use these columns, but if I did, I wanted to make it easier to use.

7. Added 'calories_type' 
    - A column which labeled recipes as High Calories if their calories was greater than or equal to 500 and 
    Low Calories if they weren't.

The final result is a dataframe with 234428 rows and 25 columns.
(Note: the following visualization is a portion of the columns)

| name                                 |     id |   minutes |   n_steps |   rating |   calories (#) |   total fat (PDV) |   sugar (PDV) |   sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) | calories_type   |
|:-------------------------------------|-------:|----------:|----------:|---------:|---------------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|:----------------|
| 1 brownies in the world    best ever | 333281 |        40 |        10 |        4 |          138.4 |                10 |            50 |              3 |               3 |                    19 |                     6 | Low Calories    |
| 1 in canada chocolate chip cookies   | 453467 |        45 |        12 |        5 |          595.1 |                46 |           211 |             22 |              13 |                    51 |                    26 | High Calories   |
| 412 broccoli casserole               | 306168 |        40 |         6 |        5 |          194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 | Low Calories    |
| 412 broccoli casserole               | 306168 |        40 |         6 |        5 |          194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 | Low Calories    |
| 412 broccoli casserole               | 306168 |        40 |         6 |        5 |          194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 | Low Calories    |

# Univariate Analysis
For my univariate analysis, I wanted to see the distribution of calories to get a better understanding
of the values within the column. From the visualization, we can see that the distribution is skewed right 
and that most of the calories fall within the 250 - 750 range.

<iframe
  src="../assets/dist-calories.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

# Bivariate Analysis
For my bivariate analysis, I wanted to see what proportion of high caloric recipes and low caloric recipes were
in each rating. From the visualization, we can see that the proportion of high caloric recipes in the 1 - 3 rating 
range are slightly greater than low caloric recipes.

<iframe
  src="../assets/calories-rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

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

Level of Signficance: 0.05

I ran a permutation test by shuffling the calories 500 times and each time I collected the absolute difference in the average calories of the groups: missing ratings and not missing ratings.

<iframe
  src="../assets/calories-missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I calculated an observed statistic of 68.98 and a p-value of 0.0, which is less than the level of significance of 
0.05. This means that we reject the null hypothesis and it suggests that the missingness of rating does depend on calories.

# Hypothesis Testing
I conducted my hypothesis testing based on whether people rated high caloric recipes (greater than or equal to 500 calories) higher than low caloric recipes.

Null Hypothesis: The rating of high caloric recipes and low caloric recipes have the same distribution.

Alternate Hypothesis: The rating of high caloric recipes food is greater than the rating of low caloric recipes.

Test Statistic: Difference in mean rating between high caloric and low caloric recipes

Level of Significance: 0.05

When conducting the hypothesis test, I used a permutation test because I wanted to see if the ratings of high caloric recipes and low caloric recipes were rated the same. I thought that the high caloric recipes would result in
higher ratings because they have more flexibility in the amount of items they can put in their recipes, which would 
make the recipes more flavorful or people might rate desserts (which are high in calories) higher.

To perform my hypothesis test, I shuffled the rating 500 times and calculated the difference in the average rating 
of high caloric recipes and low caloric recipes each time. I then calculated the observed difference, which was 
-0.017. This tells me that high caloric recipes in my sample were rated lower than low caloric recipes. When I 
found the p_value, the value was 1.0. This causes us to fail to reject the null hypothesis.

<iframe
  src="../assets/highcalories-test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>










