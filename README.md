# Recipe Finder
**Recipe Finder** is a command-line python tool that helps users discover personalized recipes based on their diet, nutrition goals, and ingredient preferences.

## Project Purpose
Many people including myself stick to the same recipes everyday. That is why the goal behind this project was to build a tool that helps users discover new recipes that match their personal preferences and help bring back some variety into their meal plan.

## Video Demo: https://youtu.be/NQexvBLxPq8

## Key Features:
- Uses the **spoonacular API** to fetch recipe data from a huge database
- Supports user-defined **filters**:
    - **Diets** (e.g. vegan, vegetarian. gluten free)
    - **Macronutrient limits** (e.g. maxCalories)
    - Included or excluded **ingredients**
- Lets users choose **the number** of recipes (1–3) they want
- Lets users choose if they want the **most popular** or **random** recipes
- Saves the formatted recipe info to **.txt files**

## Spoonacular API
Spoonacular API is a food and recipe API that provides access to a large database of recipes, including detailed nutritional information, ingredients and instructions. It also allows user to filter for specific recipes based on various criteria.

## File Overview:

### project.py

This is the main file of the project and contains:

- a **main** function that guides the user through five input steps, collecting the user-defined recipe filters and passing them to the helper functions which handle the input validation and information extraction. The main function also contains loops that reprompt the user until a valid input is provided. In some cases the main function has to furthermore format the returned parameters slightly for the API call. In step six the resulting query parameters are then passed to two additional functions that handle the API call and output the formatted recipe data to text files.

**Helper Functions**:

- a **parse_diet_preferences** function that checks for optional dietary choices and ensures only supported values are accepted. It ensures that the diet choices are separated by a comma, as otherwise the information can't be processed. It validates each entry against a defined list of allowed diets (vegan, vegetarian, gluten free). If the user provides both vegan and vegetarian at the same time (which is logically inconsistent) the function returns None, displays a corresponding message and the user will be reprompted by main. If the user input is valid the function returns a cleaned **string** suitable for the API query. If the user input is invalid the function returns 'None' and the user will be reprompted by main. If the user types 'no', the function returns 'no' as a string and skips diet preferences.

- a **parse_macro_limits** function that checks the user's input for optional macro-nutrient limits (calories, protein, fat, carbs). It accepts minimum and maximum values (maxCalories=600, minProtein=20) and validates the input via a regular expression. The regular expression ensures correct formatting by requiring a valid parameter (e.g maxCalories), an equal sign and an appropriate numerical range (e.g 0-999 kcal for calories, 0-99 g for other macros). If the user input is valid, the function returns a cleaned **string** suitable for the API query. If the user input has spelling mistakes or is wrongly formatted the function  returns 'None' and the user will be reprompted by main. If the user types 'no', the function returns 'no' as a string and skips macro limits.

- a **parse_ingredient_filters** function that checks the user's input for ingredients to include or exclude in the recipe. It uses a regular expression to distinguish between included and excluded ingredients. The regular expression looks for a '+' for included or a '-' for excluded ingredients, directly followed by the name of the ingredient(e.g. +chicken, -tomato). Ingredients must be separated by a comma. If valid matches are found, the function returns a cleaned **list** of included and/or excluded ingredients. If the user input contains no valid ingredient filters, the function returns 'None' and the user will be reprompted via main. If the user types 'no', the function returns 'no' as a string and skips ingredient filtering.

- a **get_valid_recipe_number** function that checks if the given user input for the count of desired recipes is a number in the range of 1-3. The function uses a try-except block to handle invalid inputs, such as non numeric strings. If the user input is valid, the function returns a cleaned **string** suitable for the API query. Otherwise, it returns 'None' and the user will be reprompted by main.

- a **get_valid_sorting_preference** function that checks if the given user input for the sorting preference matches one of the accepted sorting options: 'popular' or 'random'. If the input is valid, the function returns a cleaned **string** suitable for the API query. Otherwise, it returns 'None' and the user will be reprompted by main.

- a **get_recipe_json** function that is responsible for sending two API requests to the spoonacular API using the query parameters collected from the user input. In a first step, it uses the 'ComplexSearch' API to search for recipes which match the selected filters from the user. The result is converted to JSON and the corresponding recipe ids are extracted. In a second step these recipe IDs are then used in the 'informationBulk' API endpoint to retrieve full recipe details, including nutritional data. The information is unfiltered and unformatted at this point. The function returns the resulting **JSON object**, which contains all detailed information for every collected recipe.

- a **recipe_output** function that handles the final step of the tool by taking the detailed recipe JSON returned from the 'get_recipe_json' function and formatting it into human-readable text files. For each recipe the function extracts the title, number of servings, nutritional information, ingredients and instructions (if available). It also removes any leftover HTML tags from the instruction text. Each recipe is then saved in a separate **text file**, named after its title. The function itself returns **'None'**.


### test_project.py
This file contains unit tests for all user input handling functions:
    - parse_diet_preferences
    - parse_macro_limits
    - parse_ingredient_filters
    - get_valid_recipe_number
    - get_valid_sorting_preference

All tests can be executed with pytest and ensure proper behaviour for valid as well as invalid user inputs.

### requirements.txt
The project uses the built in libraries 're' and 'sys', but it also uses the 'requests' module, which has to be installed via pip.

## Author
Created by **Tom Rosenberger** as a final project for the course **CS50’s Introduction to Programming with Python**.

## Note:
This repository only contains documentation and a video demo due to CS50’s academic honesty policy. The source code is not publicly available.
