---
layout: spec
---

# What Do We Think of the Recipes?

<br>
<br>
By
<br>
[Tawseef Rahman](https://www.tawseefrahman.com)
<br>
[tawseefr@umich.edu](mailto:tawseefr@umich.edu)

This is my work for the Final Project in the EECS 398: Practical Data Science course at the University of Michigan - Ann Arbor.

In this project, I take a look at a recipes dataset. I will...

- propose a research question
- clean the dataset
- display plots of univariate analysis
- display plots of bivariate analysis
- propose a prediction problem
- train a baseline model
- tune my baseline model to create a final model

## Introduction

### Introduction and Question Identification

#### Question and Reasoning

The question that I am investigating in my project is:

> Do recipes with more reviews (`review` count) have a higher average rating (`avg_rating`)?

Understanding this relationship could help users make more informed decisions when selecting recipes. If there is a strong correlation between the number of reviews and the average rating, it may suggest that popular recipes tend to be better received - or that crowd consensus leads to more accurate ratings over time. This insight could also benefit food bloggers, content creators, and developers of food recommendation systems.

#### About the Dataset

The dataset I'm using contains information on **234,429** recipes and their reviews, compiled into a single DataFrame named `merged_recipes_df`. This DataFrame includes a wide range of features related to recipe content, nutrition, user information, and review data.

The DataFrame `merged_recipes_df` contains the following columns:

- `name` (string): The name of a recipe
- `id` (int): The ID number corresponding to a recipe
- `minutes` (int): The length of time (in minutes) it takes to cook the recipe
- `contributor_id` (int): The ID number for the author of the recipe post
- `submitted` (YYYY-MM-DD): The date when a recipe post was published
- `tags` (list of strings): Relevant tags related to the recipe post
- `n_steps` (int): The number of steps in the recipe
- `steps` (list of strings): The steps used to cook the recipe
- `description` (string): A description of the recipe
- `ingredients` (list of strings): All the ingredients for the recipe
- `n_ingredients` (int): The number of ingredients for a recipe
- `calories` (int): The number of calories for a recipe
- `total_fat_PDV` (float): The percentage of daily value of total fat in a recipe
- `sugar_PDV` (float): The percentage of daily value of sugar in a recipe
- `sodium_PDV` (float): The percentage of daily value of sodium in a recipe
- `protein_PDV` (float): The percentage of daily value of protein in a recipe
- `saturated_fat_PDV` (float): The percentage of daily value of saturated fat in a recipe
- `carbohydrates_PDV` (float): The percentage of daily value of carbohydrates in a recipe
- `user_id` (int): The ID number for the author of the review poster
- `recipe_id` (int): The ID number for the recipe for the corresponding review post
- `date` (YYYY-MM-DD): The date of the review post
- `rating` (float): The rating (1, inclusive to 5, inclusive) of a review
- `review` (string): The actual review
- `avg_rating` (float): The average value of ratings for a recipe
- `year` (int): The year a recipe post was published

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

#### Data Cleaning and Preparation

Before analyzing the relationship between review count and average rating, it was essential for me to clean and preprocess the raw data. The original `RAW_recipes.csv` dataset contains metadata and nutritional information for each recipe. The original `RAW_interactions.csv` dataset contains individual user interactions, including ratings and written reviews. These raw datasets contained a variety of formats and inconsistencies that needed to be addressed before performing any meaningful analysis.

1. Loading the Datasets
   <br>
   a. I started by loading both datasets using `pandas`.
2. Converting and Expanding the Nutrition Data
   <br>
   a. In `RAW_recipes.csv`, the `nutrition` column is stored as a string representation of a list. To analyze nutritional content, I needed to convert the string representation of a list into an actual list and separate that list into meaningful components.
   <br>
   b. I applied the `eval()` function to transform the string into a Python list.
   <br>
   c. I then expanded this list into individual nutrition columns: `calories`, `total_fat_PDV`, `sugar_PDV`, `sodium_PDV`, `protein_PDV`, `saturated_fat_PDV`, and `carbohydrates_PDV`.
   <br>
   d. Step 2c. enabled me to treat nutritional data as structured numerical values, making it possible to include them in future quantitative analyses or visualizations.
3. Merging Recipe and Interaction Data
   <br>
   a. I then joined the `RAW_recipes.csv` dataset and the `RAW_interactions.csv` dataset using a **left join** on `id` from the `RAW_recipes.csv` dataset and `recipe_id` from the `RAW_interactions.csv` dataset. This data merging step on the two datasets preserved all recipe entries, even those without user interactions.
   <br>
   b. Step 3a. connected user-generated ratings and reviews to specific recipes, allowing me to measure popularity (`review` count) and perceived quality (`avg_rating`) for each recipe.
4. Handling Invalid Ratings
   <br>
   a. Some ratings in the merged dataset had a value of `0`, which is not a valid rating on a typical 1-5 scale. These entries were likely due to user error or placeholder values; I replaced the `0` values with `NaN` to prevent skewing the results.
   <br>
   b. Step 4a. ensured that the average rating calculations were accurate and not artificially deflated by invalid scores.
5. Calculating Average Rating Per Recipe
   <br>
   a. I then calculated the mean rating for each recipe using the cleaned ratings and merged this value back into the main dataset as a new column, `avg_rating`.
   <br>
   b. The `avg_rating` column is central to my project question. By aggregating this value for each recipe, I can copare it meaningfully to the number of reviews it received.
6. Extracting the Year of Submission
   <br>
   a. The `date` column in the `RAW_interactions.csv` dataset records the timestamp of each interaction. I converted the date into a `year` column to make it easier for me to analyze trends over time.
   <br>
   b. Step 6a. allowed for temporal analyses - like examining whether newer recipes tend to receive higher ratings or more reviews.

#### `head` of the Cleaned `merged_recipes_df` DataFrame

Here's the `head` of the cleaned `merged_recipes_df` DataFrame:

{% include head-merged_recipes_df.md %}

### Univariate Analysis

#### `plotly` Plot 1: **Histogram**: Distribution of average ratings (`avg_rating`):

The histogram shows that the distribution of average recipe ratings is **left-skewed**, with most recipes receiving high ratings. However, there is a noticable drop between the number of recipes with an **average rating of 4.5** and those with a **perfect 5.0**, suggesting that while many recipes are well-received, far fewer consistently earn top marks.

<iframe
src="assets/fig1.html"
width="800"
height="600"
frameborder="0"
></iframe>

#### `plotly` Plot 2: **Histogram**: Distribution of the number of steps (`n_steps`):

The histogram of recipe steps is **right-skewed**, indicating that while some recipes are quite complex, the majority involve a manageable **5 to 10 steps**. The aforementioned trend suggests that most users tend to submit relatively simple recipes - which may attract more reviews and higher ratings due to their accessibility, a factor worth considering when exploring the relationship between review count and average rating.

<iframe
src="assets/fig2.html"
width="800"
height="600"
frameborder="0"
></iframe>

### Bivariate Analysis

#### `plotly` Plot 3: **Scatter Plot**: Relationship between the number of reviews (`review` count) and average rating (`avg_rating`)

The scatter plot shows that most recipes cluster around **20 to 40 reviews** with **average ratings between 4 and 5**. While this aforementioned trend suggests that popular recipes tend to be highly rated, the relatively flat trend line indicates **no strong linear relationship** between review count and average rating - implying that simply having more reviews does not necessarily mean a recipe is rated higher.

<iframe
src="assets/fig3.html"
width="800"
height="600"
frameborder="0"
></iframe>

#### `plotly` Plot 4: **Bar Chart**: Distribution of the average percentage of daily value in sugar (`sugar_PDV`) for recipes at or below 2,000 calories in recipes by year (`year`)

The bar chart reveals that the **average sugar percentage of daily value** in recipes has remained relatively stable since 2008, but shows a noticable **increase beginning around 2015**, with peaks in **2015 and 2018**. The aforementioned trend suggests that in recent years, users may have submitted **sweeter or less health-conscious recipes**. To ensure a more accurate representation of recipes, this analysis only includes recipes with **2,000 calories or fewer** as those above that threshold contained **extreme outliers in sugar content** that skewed the data.

<iframe
src="assets/fig4.html"
width="800"
height="600"
frameborder="0"
></iframe>

### Interesting Aggregates

Here's a pivot table that summarizes the year trends (`year`) in recipe reviews (`review` count) and average ratings (`avg_rating`):

{% include yearly_pivot.md %}

The pivot table summarizes **yearly trends in both average recipe ratings and total review counts**, providing a broader view of how user engagement and user perceptions have evolved over time. By tracking these metrics from year to year, I can identify periods of **increased activity or shifting user preferences** - which helps contextualize my main question about the relationship between **review count** and **average rating**. For example, a rise in review volume without a corresponding increase in average rating could suggest that more reviews don't always translate to higher recipe approval.

### Imputation

Only the missing values in the `rating` column needed to be imputed with a value of `Na.N` because the `rating` column was the only column from the `RAW_interactions.csv` dataset that contained missing values for some of the rows.

## Framing a Prediction Problem

### Problem Identification

The goal of my prediction task is to **predict the average rating** (`avg_rating`) that a recipe receives based on various features such as the number of reviews, ingredients, nutritional values, and more. Because `avg_rating` is a **continuous numerical variable**, this is a **regression problem**.

- Response Variable: The response variable I am predicting is `avg_rating`.
  <br>
  I chose this variable because it reflects how positively a recipe is perceived by users, and predicting the average rating can help identify what kinds of recipes are most likely to be rated highly.

- Evaluation Metric: I am using the **coefficient of determination (R<sup>2</sup>)** to evaluate my model's performance.
  <br>
  The R<sup>2</sup> value measures the proportion of variance in the response variable that can be explained by the features used in the model. I chose R<sup>2</sup> over other regression metrics (like Mean Absolute Error (MAE) or Root Mean Square Error (RMSE)) because it provides an interpretable measure of **how well the model captures the variability in recipe ratings**, which is especially useful when comparing different models or feature sets.

### Baseline Model

For my baseline model, I used a **linear regression** approach to predict the `avg_rating` of a recipe based on two features:

- `review_count`: The number of user-submitted reviews per recipe
- `calories`: The total calorie content of the recipe

Both features are **quantitative variables**. Because the model uses only numeric data, no encoding (such as one-hot encoding) was necessary.

After splitting the data into training and testing sets (80% train, 20% test), I evaluated model performance using the **R<sup>2</sup>** metric. This metric tells me how well the model explains variability in the response variable.

The R<sup>2</sup> score value for the baseline model is **0.0003**; this value is extremely low - close to zero - which means that the model explains **virtually none of the variance** in average recipe ratings. The aforementioned interpretation of the R<sup>2</sup> score value for the baseline model suggests that simply knowing how many reviews a recipe has and its calorie content is **not sufficient** to accurately predict the recipe's rating. Therefore, I do **not** consider this baseline model to be good. This model serves primarily as a starting point for comparison as I explore more complex models and richer sets of features.

### Final Model

For my final model, I introduced two additional features: `protein_PDV` (percentage of daily value of protein) and `n_ingredients` (number of ingredients) in a recipe. I chose these features because they both provide valuable nutritional and complexity-related information about each recipe. Recipes that are high in protein or require more ingredients may influence user satisfaction and perceived quality, which in turn could affect their average ratings. Including these variables helps capture underlying patterns in the data that go beyond the number of calories and review frequency for a recipe, which were used in the baseline model.

To model the prediction task, I used a **Random Forest Regressor**, a non-linear ensemble method that is well-suited for capturing complex relationships in the data. The aforementioned choice was motivated by the fact that user ratings can be influenced by a combination of interactions that a linear model might not be able to capture.

I applied preprocessing using a `ColumnTransformer`:

- I **standardized** numeric features (`calories`, `protein_PDV`, and `n_ingredients`) using `StandardScaler`.
- I **normalized** the highly skewed `review_count` using `QuantileTransformer` with a normal output distribution.

I then used **GridSearchCV** to tune the hyperparameters of the Random Forest model. The best hyperparameters from the grid search were:

- `n_estimators`: 100
- `max_depth`: None
- `min_sample_split`: 2

I selected the best model by evaluating its performance using 5-fold cross-validation, with the R<sup>2</sup> metric as the evaluation metric. The R<sup>2</sup> metric is an appropriate metric for regression tasks because it quantifies how much of the variance in the response variable is explained by the model.

The final model achieved an R<sup>2</sup> score of **0.4502**, a substantial improvement over the baseline model's R<sup>2</sup> score of 0.0003. The improvement in the R<sup>2</sup> score suggrsts that incorporating domain knowledge through new features and using a more expressive model significantly improved the model's ability to predict average recipe ratings.
