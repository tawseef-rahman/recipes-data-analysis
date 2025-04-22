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

3. Merging Recipe and Interaction Data
   I then joined the two datasets using a **left join** on `id` from the `RAW_recipes.csv` dataset and `recipe_id` from the `RAW_interactions.csv` dataset. This preserved all recipe entries, even those without user interactions.  
   This step connected user-generated ratings and reviews to specific recipes, allowing me to measure popularity (`review` count) and perceived quality (`avg_rating`) for each recipe.
4. Handling Invalid Ratings
   Some ratings in the dataset had a value of `0`, which is not a valid rating on a typical 1-5 scale. These entries were likely due to user error or placeholder values; I replaced the `0` values with `NaN` to prevent skewing the results.  
   This step ensured that the average rating calculations were accurate and not artificially deflated by invalid scores.
5. Calculating Average Rating Per Recipe
   I then calculated the mean rating for each recipe using the cleaned ratings and merged this value back into the main dataset as a new column, `avg_rating`.  
   The `avg_rating` column is central to my project question. By aggregating this value for each recipe, I can compare it meaningfully to the number of reviews it received.
6. Extracting the Year of Submission
   The `date` column in the interactions dataset records the timestamp of each interaction. I converted the date into a `year` column to make it easier for me to analyze trends over time.  
   This step allowed for temporal analyses - like examining whether newer recipes tend to receive higher ratings or more reviews.

#### `head` of the cleaned DataFramed

Here's the head of the cleaned `merged_recipes_df` DataFrame:

| name                               |     id | minutes | contributor_id | submitted  | tags                                                                                                                                                                                                                        | n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | description                                                                                                                                                                                                                                                                                                                                                                      | ingredients                                                                                                                                                                    | n_ingredients | calories | total_fat_PDV | sugar_PDV | sodium_PDV | protein_PDV | saturated_fat_PDV | carbohydrates_PDV |     user_id | recipe_id | date       | rating | review                                                                                                                                                                                                                                                                                                                                           | avg_rating | year |
| :--------------------------------- | -----: | ------: | -------------: | :--------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------: | -------: | ------------: | --------: | ---------: | ----------: | ----------------: | ----------------: | ----------: | --------: | :--------- | -----: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------: | ---: |
| 1 brownies in the world best ever  | 333281 |      40 |         985201 | 2008-10-27 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings'] |      10 | ['heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes', 'remove from the oven and cool completely before cutting']                                                  | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                             | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |             9 |    138.4 |            10 |        50 |          3 |           3 |                19 |                 6 |      386585 |    333281 | 2008-11-19 |      4 | These were pretty good, but took forever to bake. I would send it ended up being almost an hour! Even then, the brownies stuck to the foil, and were on the overly moist side and not easy to cut. They did taste quite rich, though! Made for My 3 Chefs.                                                                                       |          4 | 2008 |
| 1 in canada chocolate chip cookies | 453467 |      45 |        1848091 | 2011-04-11 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                               |      12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy', 'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !'] | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                           | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |            11 |    595.1 |            46 |       211 |         22 |          13 |                51 |                26 |      424680 |    453467 | 2012-01-26 |      5 | Originally I was gonna cut the recipe in half (just the 2 of us here), but then we had a park-wide yard sale, & I made the whole batch & used them as enticements for potential buyers ~ what the hey, a free cookie as delicious as these are, definitely works its magic! Will be making these again, for sure! Thanks for posting the recipe! |          5 | 2012 |
| 412 broccoli casserole             | 306168 |      40 |          50969 | 2008-05-30 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |       6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |             9 |    194.8 |            20 |         6 |         32 |          22 |                36 |                 3 |       29782 |    306168 | 2008-12-31 |      5 | This was one of the best broccoli casseroles that I have ever made. I made my own chicken soup for this recipe. I was a bit worried about the tsp of soy sauce but it gave the casserole the best flavor. YUM!                                                                                                                                   |          5 | 2008 |
|                                    |        |         |                |            |                                                                                                                                                                                                                             |         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                |               |          |               |           |            |             |                   |                   |             |           |            |        | The photos you took (shapeweaver) inspired me to make this recipe and it actually does look just like them when it comes out of the oven.                                                                                                                                                                                                        |            |      |
|                                    |        |         |                |            |                                                                                                                                                                                                                             |         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |                                                                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                |               |          |               |           |            |             |                   |                   |             |           |            |        | Thanks so much for sharing your recipe shapeweaver. It was wonderful! Going into my family's favorite Zaar cookbook :)                                                                                                                                                                                                                           |            |      |
| 412 broccoli casserole             | 306168 |      40 |          50969 | 2008-05-30 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |       6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |             9 |    194.8 |            20 |         6 |         32 |          22 |                36 |                 3 | 1.19628e+06 |    306168 | 2009-04-13 |      5 | I made this for my son's first birthday party this weekend. Our guests INHALED it! Everyone kept saying how delicious it was. I was I could have gotten to try it.                                                                                                                                                                               |          5 | 2009 |
| 412 broccoli casserole             | 306168 |      40 |          50969 | 2008-05-30 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                        |       6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside', 'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce', 'pour into baking dish , sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes']                                                                                                                                                                                                                                                                                                                              | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ['frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions']          |             9 |    194.8 |            20 |         6 |         32 |          22 |                36 |                 3 |      768828 |    306168 | 2013-08-02 |      5 | Loved this. Be sure to completely thaw the broccoli. I didn&#039;t and it didn&#039;t get done in time specified. Just cooked it a little longer though and it was perfect. Thanks Chef.                                                                                                                                                         |          5 | 2013 |

### Univariate Analysis

#### `plotly` plot 1: **Histogram** that displays the distribution of average ratings (`avg_rating`) for recipes:

The histogram shows that the distribution of average recipe ratings is **left-skewed**, with most recipes receiving high ratings. However, there is a noticable drop between the number of recipes with an **average rating of 4.5** and those with a **perfect 5.0**, suggesting that while many recipes are well-received, far fewer consistently earn top marks.

#### `plotly` plot 2: **Histogram** that displays the distribution of the number of steps (`n_steps`) for recipes:

The histogram of recipe steps is **right-skewed**, indicating that while some recipes are quite complex, the majority involve a manageable **5 to 10 steps**. This suggests that most users tend to submit relatively simple recipes - which may attract more reviews and higher ratings due to their accessibility, a factor worth considering when exploring the relationship between review count and average rating.

### Bivariate Analysis

#### `plotly` plot 3: **Scatter plot** that displays the relationship between the number of reviews (`review` count) and average rating (`avg_rating`)

The scatter plot shows that most recipes cluster around **20 to 40 reviews** with **average ratings between 4 and 5**. While this suggests that popular recipes tend to be highly rated, the relatively flat trend line indicates **no strong linear relationship** between review count and average rating - implying that simply having more reviews doesn't necessarily mean a recipe is rated higher.

#### `plotly` plot 4: **Bar chart** that displays the distribution of the average percentage of daily value in sugar (`sugar_PDV`) for recipes at or below 2,000 calories in recipes by year (`year`)

The bar chart reveals that the **average sugar percentage of daily value** in recipes has remained relatively stable since 2008, but shows a noticable **increase beginning around 2015**, with peaks in **2015 and 2018**, which suggests that in recent years, users may be submitted **sweeter or less health-conscious recipes**. To ensure a more accurate representation, this analysis includes only recipes with **2,000 calories or fewer**, as those above that threshold contained **extreme outliers in sugar content** that skewed the data.

### Interesting Aggregates

Here's a pivot table that summarizes the yearly trends (`year`) in recipe reviews (`review` count) and average ratings (`avg_rating`):

| year | avg_rating | review |
| ---: | ---------: | -----: |
| 2008 |    4.62743 |  31593 |
| 2009 |    4.67466 |  48420 |
| 2010 |    4.70223 |  36970 |
| 2011 |    4.70723 |  29187 |
| 2012 |    4.73037 |  23827 |
| 2013 |    4.70944 |  21849 |
| 2014 |    4.68466 |  12580 |
| 2015 |     4.6112 |   8206 |
| 2016 |    4.59224 |   5999 |
| 2017 |    4.59481 |   8825 |
| 2018 |    4.59397 |   6915 |

The pivot table summarizes **yearly trends in both average recipe ratings and total review counts**, providing a broader view of how user engagement and user perceptions have evolved ofer time. By tracking these metrics from year to year, I can identify periods of **increased actiivty or shifting user preferences** - which helps contextualize my main question about the relationship between **review count** and **average rating**. For example,a rise in review volume without a corresponding increase in average rating could suggest that more reviews don't always translate to higher recipe approval.

### Imputation

Only the missing values in the `rating` column needed to be imputed with a value of `Na.N` because the `rating` column was the only column from the `RAW_interactions.csv` file that contained missing values.

## Framing a Prediction Problem

## Baseline Model

## Final Model
