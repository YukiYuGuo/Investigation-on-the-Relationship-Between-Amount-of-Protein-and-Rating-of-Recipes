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

## Data Cleaning and Exploratory Data Analysis ##
## Assessment of Missingness ##
## Hypothesis Testing ##
## Framing a Prediction Problem ##
## Baseline Model ##
## Final Model ##
## Fairness Analysis ##
