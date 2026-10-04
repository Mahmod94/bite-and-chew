# Bite & Chew

A relational database for managing recipes, ingredients, cooking instructions, menus and business advertisements.

Developed as a university database project to practise relational modelling, SQL and data integrity.

## Technologies

- MariaDB — database server
- SQL — schema definition, data manipulation and queries
- DBeaver — database development
- XAMPP and phpMyAdmin — local environment and database administration

## Database Structure

The database contains 13 tables:

| Table | Purpose |
|---|---|
| zipcode | Swedish postal codes and cities |
| business | Businesses and their addresses |
| advertisement | Advertisements associated with businesses |
| author | Recipe authors |
| source | Recipe sources |
| recipe | Recipe details, preparation times and servings |
| ingredient | Ingredient names |
| recipeingredient | Ingredients, quantities and units for each recipe |
| instruction | Ordered cooking instructions |
| menu | Menus that group recipes |
| menurecipes | Relationships between menus and recipes |
| category | Recipe categories |
| categoryrecipes | Relationships between categories and recipes |

## Database Design

Many-to-many relationships are represented by junction tables with composite primary keys:

- Recipes and ingredients: `recipeingredient`
- Menus and recipes: `menurecipes`
- Categories and recipes: `categoryrecipes`

Foreign keys maintain relationships between tables. Additional constraints control required values, uniqueness and valid data.

## Design Decisions

- **Business advertisements:** deleting a business also deletes its advertisements through `ON DELETE CASCADE`.
- **Recipe authors and sources:** `ON DELETE SET NULL` allows recipes to remain when their author or source is removed.
- **Postal codes:** stored as text because they are identifiers rather than quantities.
- **Instruction order:** each instruction number must be unique within its recipe.
- **Serving counts:** a check constraint requires non-null serving counts to be greater than zero.

Several tables use names as natural primary keys. This keeps the initial design straightforward, but means renaming these records requires updating their references. Surrogate keys are a possible future improvement.

## Stored Procedures

The database includes parameterised procedures for retrieving menu recipes and cooking instructions.

```sql
-- List the recipes in a menu.
CALL GetMenuRecipes('Italian Evening');

-- Retrieve a recipe's instructions in order.
CALL GetRecipeInstructions('Swedish Meatballs');
```

## Views

`recipe_and_author` lists recipes with their authors’ names, email addresses and serving counts. It uses a LEFT JOIN to include recipes without an author.

Example:
```sql
SELECT *
FROM recipe_and_author
ORDER BY authorfname, authorlname, recipename;

## ER Diagram

<img width="1625" height="786" alt="Skärmbild 2026-10-04 131629" src="https://github.com/user-attachments/assets/182d298e-a77c-416e-8f9f-ada5743ef837" />
