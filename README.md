# Investigation on the Relationship Between Amount of Protein and Rating of Recipes

Author:Haoyu Guo
## Overview 

This is a data science project for DSC80 at UCSD focusing on exploring the relationship between the rating of a recipe and the proportion of protein contributing to the total calories of the given recipe.

## Introduction 
Protein is an essential nutrient for human health, vital for the formation of cells, tissues and organs and plays a role in various biochemical processes within the body. In today's health-conscious society, protein plays a central role in increasing muscle mass and strength. During strength training or other forms of resistance training, muscle fibers can suffer minor damage. In order to repair these damages and gradually build muscle, the body needs protein. In this context, **we aimed to explore whether there is a correlation between the amount of protein in people's diets and the scoring of recipes.** We wanted to determine whether people rate recipes with higher protein content for faster muscle growth.

For this purpose, we analyzed two datasets, including recipes and interactions posted on  [food.com](https://www.food.com/). The original purpose of the datasets is for the recommender system research paper, [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) by Majumder et al.

For the first dataset —— `recipe`, contains 83782 rows, indicating 83782 unique recipes, with 10 columns . It contains the basic information contained in a recipe screen on the [food.com](https://www.food.com/) Web site. The columns information is as follows:

| Column             | Description                                                                           
| :----------------- | :-------------------------------------------------------------------------------------
| `'name'`           | Recipe name                                                                           
| `'id'`             | Recipe ID                                                                             
| `'minutes'`        | Minutes to prepare recipe                                                             
| `'contributor_id'` | User ID who submitted this recipe                                                     
| `'submitted'`      | Date recipe was submitted                                                             
| `'tags'`           | Food.com tags for recipe                                                              
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                             
| `'steps'`          | Text for recipe steps, in order                                                       
| `'description'`    | User-provided description                                                             
| `'ingredients'`    | Text for recipe ingredients                                                           
| `'n_ingredients'`  | Number of ingredients in recipe 


The second dataset —— 'interactions', contains 731,927 rows and four columns. Contains basic information about user comments. The information in each column is as follows:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

## Data Cleaning and Exploratory Data Analysis 
### 1. Data cleansing and organization of the two Datasets respectively
(1) To 'recipes'
The data types of the columns of 'recipes' are shown below:
- | Column             | Description |
     | :----------------- | :---------- |
     | `'name'`           | object      |
     | `'id'`             | int64       |
     | `'minutes'`        | int64       |
     | `'contributor_id'` | int64       |
     | `'submitted'`      | object      |
     | `'tags'`           | object      |
     | `'nutrition'`      | object      |
     | `'n_steps'`        | int64       |
     | `'steps'`          | object      |
     | `'description'`    | object      |
-First we convert the submitted column to the Datatime type. This is because the data in this column represents the month and year when the recipe was submitted. Converting to Datatime type facilitates subsequent exploration of the data.

-Second, combine our concerns. The protein content in each recipe needs to be obtained. So we need to take the 7 types of information contained in nutrition ('calories (#)', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)') and extract and each became a new column.

Annotate:PDV (Predicted Daily Value) is a marker used to assess the extent to which a nutrient in a food contributes to the required daily intake.PDV is calculated based on the Recommended Daily Intake (RDI) or Daily Reference Value (DRV). Values). This type of labeling helps consumers understand the extent to which the amount of a nutrient in a single food or beverage complements their daily nutritional needs.
PDV calculation formula：$$
\text{PDV \%} = \left( \frac{\text{Nutrient Content in Food}}{\text{Recommended Daily Intake}} \right) \times 100
$$

## Assessment of Missingness ##
## Hypothesis Testing ##
## Framing a Prediction Problem ##
## Baseline Model ##
## Final Model ##
## Fairness Analysis ##
