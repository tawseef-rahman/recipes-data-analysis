# Welcome to my website...

## for the EECS 398 Final Project!

### In this project, I take a look at a recipes dataset. I will...

- propose a research question
- clean the dataset
- display plots of univariate analysis
- display plots of bivariate analysis
- propose a prediction problem
- train a baseline model
- tune my baseline model to create a final model.

## Introduction

### Introduction and Question Identification

#### Question and Reasoning

The question that I am investigating in my project is:

> Do recipes with more reviews (`review` count) have a higher average rating (`avg_rating`)?

Understanding this relationshp could help users make more informed decisions when selecting recipes. If there is a strong correlation between the number of reviews and the average rating, it may suggest that popular recipes tend to be better received - or that crowd consensus leads to more accurate ratings over time. This insight could also benefit food bloggers, content creators, and developers of food recommendation systems.

#### About the Dataset

The dataset I'm using contains information on **234,429** recipes and their reviews, compiled into a single DataFrame named `merged_recipes_df`. It includes a wide range of features related to recipe content, nutrition, user information, and review data.

The dataset contains the following columns:

- `name` (string): The name of a recipe
- `id` (int): The ID number corresponding to a recipe
- `minutes` (int): The length of time (in minutes) it takes to cook the recipe.
- `contributor_id` (int): The ID number for the author of the recipe post.
- `submitted` (YYYY-MM-DD): The date when a recipe post was published.
- `tags` (list of strings): Relevant tags related to the recipe post.
- `n_steps` (int): The number of steps in the recipe.
- `steps` (list of strings): The steps used to cook the recipe.
- `description` (string): A description of the recipe.
- `ingredients` (list of strings): All the ingredients for the recipe.
- `n_ingredients` (int): The number of ingredients for a recipe.
- `calories` (int): The number of calories for a recipe.
- `total_fat_PDV` (float): The percentage of daily value of total fat in a recipe.
- `sugar_PDV` (float): The percentage of daily value of sugar in a recipe.
- `sodium_PDV` (float): The percentage of daily value of sodium in a recipe.
- `protein_PDV` (float): The percentage of daily value of protein in a recipe.
- `saturated_fat_PDV` (float): The percentage of daily value of saturated fat in a recipe.
- `carbohydrates_PDV` (float): The percentage of daily value of carbohydrates in a recipe.
- `user_id` (int): The ID number for the author of the review poster.
- `recipe_id` (int): The ID number for the recipe for the corresponding review post.
- `date` (YYYY-MM-DD): The date of the review post.
- `rating` (float): The rating of a review
- `review` (string): The actual review
- `avg_rating` (float): The average value of ratings for a recipe.
- `year` (int): The year a recipe post was published.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

#### Data Cleaning and Preparation

Before analyzing the relationship between review count and average rating, it was essential for me to clean and preprocess the raw data. The original `RAW_recipes.csv` dataset contains metadata and nutritional information for each recipe. The original `RAW_interactions.csv` dataset contains individual user interactions, including ratings and written reviews. These raw datasets contained a variety of formats and inconsistencies that needed to be addressed before performing any meaningful analysis.

1. Loading the Datasets  
   I started by loading both datasets using `pandas`.
2. Converting and Expanding the Nutrition Data  
   In `RAW_recipes.csv`, the `nutrition` column is stored as a string representation of a list. To analyze nutritional content, I needed to convert it into an actual list and separate it into meaningful components.

I applied the `eval()` function to transform the string into a Python list.
I then expanded this list into individual nutrition columns: `calories`, `total_fat_PDV`, `sugar_PDV`, `sodium_PDV`, `protein_PDV`, `saturated_fat_PDV`, and `carbohydrates_PDV`.  
This step enabled me to treat nutritional data as structured numerical values, making it possible to include them in future quantitative analyses or visualizations.

1. Merging Recipe and Interaction Data
   I then joined the two datasets using a **left join** on `id` from the `RAW_recipes.csv` dataset and `recipe_id` from the `RAW_interactions.csv` dataset. This preserved all recipe entries, even those without user interactions.
   This step connected user-generated ratings and reviews to specific recipes, allowing me to measure popularity (`review` count) and perceived quality (`avg_rating`) for each recipe.
2. Handling Invalid Ratings
   Some ratings in the dataset had a value of `0`, which is not a valid rating on a typical 1-5 scale. These entries were likely due to user error or placeholder values; I replaced the `0` values with `NaN` to prevent skewing the results.
   This step ensured that the average rating calculations were accurate and not artificially deflated by invalid scores.
3. Calculating Average Rating Per Recipe
   I then calculated the mean rating for each recipe using the cleaned ratings and merged this value back into the main dataset as a new column, `avg_rating`.
   The `avg_rating` column is central to my project question. By aggregating this value for each recipe, I can compare it meaningfully to the number of reviews it received.
4. Extracting the Year of Submission
   The `date` column in the interactions dataset records the timestamp of each interaction. I converted the date into a `year` column to make it easier for me to analyze trends over time.
   This step allowed for temporal analyses - like examining whether newer recipes tend to receive higher ratings or more reviews.

## Framing a Prediction Problem

## Baseline Model

## Final Model
