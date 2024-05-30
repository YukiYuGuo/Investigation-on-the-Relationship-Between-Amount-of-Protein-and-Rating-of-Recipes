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

*Annotate : PDV (Predicted Daily Value) is a marker used to assess the extent to which a nutrient in a food contributes to the required daily intake.PDV is calculated based on the Recommended Daily Intake (RDI) or Daily Reference Value (DRV). Values). This type of labeling helps consumers understand the extent to which the amount of a nutrient in a single food or beverage complements their daily nutritional needs.
PDV calculation formula：$$
\text{PDV \%} = \left( \frac{\text{Nutrient Content in Food}}{\text{Recommended Daily Intake}} \right) \times 100
$$

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

- First we convert the submitted column to the Datatime type. This is because the data in this column represents the month and year when the recipe was submitted. Converting to Datatime type facilitates subsequent exploration of the data.

- Second, combine our concerns. The protein content in each recipe needs to be obtained. So we need to take the 7 types of information contained in nutrition **('calories (#)', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)')** and extract and each became a new column.

(2) To 'interactions'
- We convert the date data in the interaction to the datatime type. This is convenient for use in subsequent processes.

### 2.Left merge the recipes and interactions datasets on id and recipe_id.
Record the merged table as **all_df**.The merged and converted data is shown below:
| Column                  | Description    |
| :---------------------- | :------------- |
| `'name'`                | object         |
| `'id'`                  | int64          |
| `'minutes'`             | int64          |
| `'contributor_id'`      | int64          |
| `'submitted'`           | datetime64[ns] |
| `'tags'`                | object         |
| `'nutrition'`           | object         |
| `'n_steps'`             | int64          |
| `'steps'`               | object         |
| `'description'`         | object         |
| `'ingredients'`         | object         |
| `'n_ingredients'`       | int64          |
| `'user_id'`             | float64        |
| `'recipe_id'`           | float64        |
| `'date'`                | datetime64[ns] |
| `'rating'`              | float64        |
| `'review'`              | object         |
| `'calories (#)'`        | float64        |
| `'total fat (PDV)'`     | float64        |
| `sugar (PDV)'`          | float64        |
| `'sodium (PDV)'`        | float64        |
| `'protein (PDV)'`       | float64        |
| `'saturated fat (PDV)'` | float64        |
| `'carbohydrates (PDV)'` | float64        |

### 3.Fill all ratings of 0 with np.nan.
Rating is generally on a scale from 1 to 5, 1 meaning the lowest rating while 5 means the highest rating. With that being said, a rating of 0 indicates missing values in rating. With that being said, a rating of 0 indicates missing values in rating. Replacing all values in the merged dataset that have a rating of 0 with np.nan clearly indicates missing data, rather than incorrectly using 0 as the actual rating. When calculating averages or other statistics, using 0 as a rating may pull down the average, resulting in an inaccurate analysis. Replacing 0 with NaN avoids this.

### 4.Calculate the average rating for each recipe as `'average_rating'`
Different users may have different feelings about the same recipe and thus give different ratings. We calculate the average score of a recipe to get a more objective understanding of the rating of a given recipe.

### 5.Calculate the `'pro_proportion'`
''pro_proportion'' is the percentage of protein to total calories in the recipe. To calculate it, we divide the value in the Protein (PDV) column by 100% to get the value in decimal form. Then, we multiply by 50 to convert the value to grams of protein, since 50 grams of protein is 100% of the daily intake value (PDV). We know from literature data that 1 gram of protein contains 4 grams of calories, so we multiply by 4. After all of these conversions, we get the number of calories in protein, so we can divide the total calories in the recipe by the number of calories in protein to get the percentage of protein in total calories. This data cleaning step is critical and allows us to compare the protein content of recipes in parallel without having to worry about the values being too large, since all values are between 0 and 1.

### Result
After the above steps, we can get the information of each column of the final processed all_df data:
| Column                  | Description    |
| :---------------------- | :------------- |
| `'name'`                | object         |
| `'id'`                  | int64          |
| `'minutes'`             | int64          |
| `'contributor_id'`      | int64          |
| `'submitted'`           | datetime64[ns] |
| `'tags'`                | object         |
| `'nutrition'`           | object         |
| `'n_steps'`             | int64          |
| `'steps'`               | object         |
| `'description'`         | object         |
| `'ingredients'`         | object         |
| `'n_ingredients'`       | int64          |
| `'user_id'`             | float64        |
| `'recipe_id'`           | float64        |
| `'date'`                | datetime64[ns] |
| `'rating'`              | float64        |
| `'review'`              | object         |
| `'average rating'`      | object         |
| `'calories (#)'`        | float64        |
| `'total fat (PDV)'`     | float64        |
| `sugar (PDV)'`          | float64        |
| `'sodium (PDV)'`        | float64        |
| `'protein (PDV)'`       | float64        |
| `'saturated fat (PDV)'` | float64        |
| `'carbohydrates (PDV)'` | float64        |
| `'pro_proportion'`          | float64           |

## Assessment of Missingness ##
## Hypothesis Testing ##


## Framing a Prediction Problem ##
## Baseline Model ##
## Final Model ##
## Fairness Analysis ##
