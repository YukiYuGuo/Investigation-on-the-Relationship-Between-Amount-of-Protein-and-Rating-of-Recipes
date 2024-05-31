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
1. Data cleansing and organization of the two Datasets respectively
(1) To 'recipes'

- First we convert the submitted column to the Datatime type. This is because the data in this column represents the month and year when the recipe was submitted. Converting to Datatime type facilitates subsequent exploration of the data.

- Second, combine our concerns. The protein content in each recipe needs to be obtained. So we need to take the 7 types of information contained in nutrition **('calories (#)', 'total fat (PDV)', 'sugar (PDV)', 'sodium (PDV)', 'protein (PDV)', 'saturated fat (PDV)', 'carbohydrates (PDV)')** and extract and each became a new column.

(2) To 'interactions'
- We convert the date data in the interaction to the datatime type. This is convenient for use in subsequent processes.

2.Left merge the recipes and interactions datasets on id and recipe_id.
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

3.Fill all ratings of 0 with np.nan.
Rating is generally on a scale from 1 to 5, 1 meaning the lowest rating while 5 means the highest rating. With that being said, a rating of 0 indicates missing values in rating. With that being said, a rating of 0 indicates missing values in rating. Replacing all values in the merged dataset that have a rating of 0 with np.nan clearly indicates missing data, rather than incorrectly using 0 as the actual rating. When calculating averages or other statistics, using 0 as a rating may pull down the average, resulting in an inaccurate analysis. Replacing 0 with NaN avoids this.

4.Calculate the average rating for each recipe as `'average_rating'`
Different users may have different feelings about the same recipe and thus give different ratings. We calculate the average score of a recipe to get a more objective understanding of the rating of a given recipe.

5.Calculate the `'pro_proportion'`
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
| `'sugar (PDV)'`          | float64        |
| `'sodium (PDV)'`        | float64        |
| `'protein (PDV)'`       | float64        |
| `'saturated fat (PDV)'` | float64        |
| `'carbohydrates (PDV)'` | float64        |
| `'pro_proportion'`          | float64           |



<iframe
  src="assets/assetspic1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness ##
## Hypothesis Testing ##


## Framing a Prediction Problem ##
Since Hypothesis Testing found that how much protein is in a recipe does not affect how people rate the recipe. So we won't delve into this issue in the prediction session.
People don't seem to be too concerned about the amount of protein in a recipe, though. However, at a time when the risk of various diseases is increasing, more and more people are focusing on eating healthy and paying attention to the calorie content of their diet. In our daily life, no matter which supermarket we go to for shopping, the outer package of the product will clearly inform customers of the calorie content of the product.
Therefore, we would like to predict 'the calories of the recipes in the website' by using the existing data. Since calories are quantitative data, this prediction problem is considered to be a regression problem. We choose a ''multiple linear regression model'' for prediction by ''coefficient of determination (R-squared)''.
## Baseline Model ##
In the baseline model, we selected ''season'' and ''carbohydrates(PDV)'' as the variables for constructing the multiple regression equation. We consider them based on the following perspectives.
1.carbohydrates(PDV)
Among the six major nutrients contained in ''nutrition'', carbohydrates are one of the important sources of energy for the human body. In our daily diet, carbohydrates are converted into glucose by the body, and glucose is the main fuel for cells to produce energy. Most foods contain a certain amount of carbohydrates, such as grains, fruits, vegetables, and legumes. When you consume too many carbohydrates, the body converts the excess glucose into fat for storage, which can lead to weight gain. However, insufficient carbohydrate intake can lead to inadequate bodily functions. For these reasons, we believe that carbohydrates can effectively help predict calorie content.
2.season
The ''season'' column is derived from the ''submitted'' column(contains the dates the recipes were uploaded). 4-5 are classified as Spring, 6-8 as Summer, 9-11 as Autumn, and 12-2 as Winter. We consider that seasonal changes also affect the caloric intake of the human body. Firstly, the types of food available and their nutritional content vary with the seasons. Summer provides an abundance of fruits and vegetables, which are high in water content but low in calories. In contrast, winter offers more root vegetables, which are higher in calories. Additionally, the human body requires more calories during the autumn and winter months to maintain body temperature. People tend to choose high-calorie foods during these seasons, while in summer, the preference shifts to lighter, water-rich, low-calorie foods. Therefore, the caloric characteristics of the recipes uploaded vary with the seasons.
In the process of building the model, we standardized the data in the 'carbohydrates(PDV)' column. Since the 'season' column contains categorical data, we processed it using OneHotEncoding. Then, we constructed a multiple linear regression equation as follows:

After training and testing, the model's R-squared for the training set is approximately 0.6602, and for the testing set, it is about 0.6624. The model seems to be quite accurate. Next, we will consider other perspectives to enhance its accuracy.
## Final Model ##

1.level 1 variables 
Considering that different combinations of the six major nutrients might have a more significant impact on calorie variations, we decided to pair and standardize the following nutrients: total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV). We then predicted calories and calculated the R-squared for each combination. The final results are as follows:

 |Variable Pair | R² Score|
 | :----------------- | :-------------|
 |(total fat(PDV), carbohydrates(PDV)) | 0.971969|
 |(total fat(PDV), sugar(PDV)) | 0.893037|
 |(saturated fat(PDV), carbohydrates(PDV)) | 0.868305|
 |(protein(PDV), carbohydrates(PDV)) | 0.807730|
 |(total fat(PDV), protein(PDV)) | 0.786403|
 |(total fat(PDV), saturated fat(PDV)) | 0.769829|
 |(total fat(PDV), sodium(PDV)) | 0.764258|
 |(sugar(PDV), saturated fat(PDV)) | 0.763484|
 |(sugar(PDV), protein(PDV)) | 0.723927|
 |(protein(PDV), saturated fat(PDV)) | 0.709994|
 |(sodium(PDV), carbohydrates(PDV)) | 0.664501|
 |(sugar(PDV), carbohydrates(PDV)) | 0.663821|
 |(sodium(PDV), saturated fat(PDV)) | 0.652906|
 |(sugar(PDV), sodium(PDV)) | 0.481095|
 |(sodium(PDV), protein(PDV)) | 0.339117|
 
We discovered that the combination of **(total fat (PDV), carbohydrates (PDV))** has an exceptionally high R-squared score, making the model nearly perfect. However, an overly high R-squared value may indicate that the model is overfitting the training data. Therefore, we have selected combinations with an R-squared greater than 0.8 as **Level 1 variables**. We will add new variables to make the model more robust.

2.level 2 variables
In addition to the **season** variable from the Baseline model, Level 2 variables also include the following:
-minutes
Different ingredients and cooking methods can affect the cooking time of a dish. For example, fried foods usually contain more calories and require more cooking time than baked or steamed foods because frying involves the use of a large amount of fat. Additionally, prolonged cooking can lead to changes in certain nutrients. For instance, long periods of stewing may cause more fat to dissolve from meat, increasing the total calories of the dish. Dishes that require a long cooking time often use ingredients that need to be cooked longer to achieve the desired texture. These ingredients themselves might be high in calories, such as fatty meats or high-sugar vegetables. Therefore, we consider cooking time as a factor that affects the caloric content of food. In our model setup, we standardize this variable.
-n_steps
The diversity and complexity of recipe steps often involve multiple cooking techniques. For example, a recipe may include frying, boiling, and baking steps, each of which can contribute to the final caloric content of the dish. Cooking methods that use a lot of fat, such as frying and sautéing, generally increase the total calories of a dish. Complex recipe steps may require more ingredient processing, such as slicing, chopping, or marinating, which can make the ingredients more likely to absorb the fats and seasonings added during cooking, thus increasing calories. Therefore, we believe that the number of steps (n_steps) is also a factor affecting the caloric content of food.

3.Final result
Based on these considerations, we combine variables from Level 1 and Level 2 to construct a multiple linear regression model and perform cross-validation to reduce the randomness and bias brought about by specific data splits. This approach makes the model's evaluation results more reliable and stable. The R-squared scores and RMSE values obtained for each combination are as follows:

 |Variable Pair | R² Score| RMSE |
 | :--------------------------| :-------- |：---------|
 |carbohydrates(PDV) & Level 2 | 0.6532 |340.21 |
 |(total fat(PDV), carbohydrates(PDV)) & Level 2 |0.9712 | 97.28 |
 |(total fat(PDV), sugar(PDV)) & Level 2| 0.8847 | 197.50 |
 |(saturated fat(PDV), carbohydrates(PDV)) & Level 2| 0.8688 |210.23 |
 |(protein(PDV), carbohydrates(PDV)) & Level 2| 0.8055 | 256.38 |
 
Based on the results, our final model has significantly outperformed the baseline model, indicating that predictions using multiple nutrients are more effective than those using a single nutrient. However, the results are not much different from those predicted by Level 1 variables alone. This suggests that among the variables that significantly affect the caloric content of recipes, the combination of various nutrients is a key variable or has a more decisive influence.
## Fairness Analysis ##
