# Relationship Between Calories and Ratings
This is the final project for the UCSD class DSC80. For this project, students were tasked to conduct an open-ended investigation using everything we learned.

# Introduction
When it comes to losing weight, the main thought that goes through peoples' minds are going months or years eating
bland food. This is the main obstacle that holds people back from reaching their goals as they believe that
finding recipes that are low calories and delicious are impossible. This is what drove my project to see 
the relationship between calories and ratings as I wanted to see if there was any validity to the idea that 
healthy food can't taste good. For this I used data from Food.com, an online recipe platform where users can submit their recipes, 
complete with nutritional information, user ratings, and reviews. Recipes is a subset of data from this website, which 
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