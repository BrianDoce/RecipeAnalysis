# Relationship Between Calories and Ratings
This is the final project for the UCSD class DSC80. For this project, students were tasked to conduct an open-ended investigation using everything we learned.

Author: Brian Docena

# Introduction
When it comes to losing weight, the main thought that goes through peoples' minds are going months or years eating
bland food. This is the main obstacle that holds people back from reaching their goals as they believe that
finding recipes that are low calories and delicious are impossible. This is what drove my project to see 
the relationship between calories and ratings as I wanted to see if there was any validity to the idea that 
healthy food can't taste good. For this I used data from Food.com, an online recipe platform where users can submit their recipes, 
 with nutritional information, user ratings, and reviews. Recipes is a subset of data from this website, which 
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