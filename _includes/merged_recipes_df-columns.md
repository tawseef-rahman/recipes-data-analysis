| Variable Name       | Variable Type    | Variable Purpose                                               |
| :------------------ | :--------------- | :------------------------------------------------------------- |
| `name`              | `str`            | The name of the recipe                                         |
| `id`                | `int`            | The ID number corresponding to the recipe                      |
| `minutes`           | `int`            | The length of time (in minutes) it takes to cook the recipe    |
| `contributor_id`    | `int`            | The ID number for the author of the recipe post                |
| `submitted`         | `YYYY-MM-DD`     | The date when the recipe post was published                    |
| `tags`              | `list` of `str`s | Relevant tags related to the recipe post                       |
| `n_steps`           | `str`            | The number of steps in the recipe                              |
| `steps`             | `list` of `str`s | The steps used to cook the recipe                              |
| `description`       | `str`            | The description of the recipe                                  |
| `ingredients`       | `list` of `str`s | All the ingredients for the recipe                             |
| `n_ingredients`     | `int`            | The number of ingredients for the recipe                       |
| `calories`          | `int`            | The number of calories for the recipe                          |
| `total_fat_PDV`     | `float`          | The percentage of daily value of total fat in the recipe       |
| `sugar_PDV`         | `float`          | The percentage of daily value of sugar in the recipe           |
| `sodium_PDV`        | `float`          | The percentage of daily value of sodium in the recipe          |
| `protein_PDV`       | `float`          | The percentage of daily value of protein in the recipe         |
| `saturated_fat_PDV` | `float`          | The percentage of daily value of saturated fat in the recipe   |
| `carbohydrates_PDV` | `float`          | The percentage of daily value of carbohydrates in the recipe   |
| `user_id`           | `int`            | The ID number for the author of the review post                |
| `recipe_id`         | `int`            | The ID number for the recipe for the corresponding review post |
| `date`              | `YYYY-MM-DD`     | The date of the review post                                    |
| `rating`            | `float`          | The rating (1, inclusive to 5, inclusive) of the review        |
| `review`            | `str`            | The actual review                                              |
| `avg_rating`        | `float`          | The average value of ratings for the recipe                    |
| `year`              | `int`            | The year the recipe post was published                         |
