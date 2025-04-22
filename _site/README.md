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

## Framing a Prediction Problem

## Baseline Model

## Final Model
