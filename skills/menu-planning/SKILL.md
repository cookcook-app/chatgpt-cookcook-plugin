---
name: menu-planning
description: Plan a dinner, a week of meals or a multi-course menu from cookbooks, with each dish linked to its book and page. Use when the user asks what to cook this week, for a menu for guests, or for meals built around ingredients they have, and prefer the cookbooks they own.
---

# Planning a menu with CookCook

A menu here is a set of recipes from real cookbooks, each with its book and
page, so the user can open the book and cook. CookCook supplies the titles,
pages and ingredient names; the choice of dishes and how they go together is
your suggestion, and you say so.

## Before you search

Use what the user already said: how many meals or courses, how many people,
what they like or avoid, what is in the fridge. Ask one short question only
when you cannot start without the answer, and state the assumptions you made
for the rest.

## Where the dishes come from

- Start with the user's own shelf. `search_my_books` searches the recipes in
  the cookbooks they own; it asks them to connect their CookCook account the
  first time. `my_books` lists the shelf when they want to plan from one book.
- Use `search_recipes` across the whole catalogue when they are not signed in,
  when they ask for it, or when their own books have nothing that fits. Say
  which dishes come from a book they do not own.
- `my_saved_recipes` is worth a look first: recipes they saved are recipes
  they meant to cook.
- Call `get_recipe` for every dish you put on the menu, for its ingredient
  names and exact pages. Do not put a dish on the menu from a search hit
  alone.

## What to show

A compact table: the day or course, the recipe title linked to its CookCook
URL, the book linked to its CookCook URL, the page, and one line on why it is
there. Keep the pages exactly as returned; when a page is missing, say it is
not available. Aim for variety across the menu and for ingredients that carry
over from one dish to the next, and present both as your reasoning.

## What the index cannot tell you

CookCook has ingredient names, not the method, the quantities, the servings or
the cooking time. Never invent them. Ingredient names help you screen dishes
but do not prove a dish is safe for an allergy, vegan, or gluten-free: flag
what you can see, and tell the user to check the recipe in the book when a
restriction is strict. The exception is a recipe of their own: `get_my_recipe`
returns the method and amounts when they captured it or hold the book as an
ebook, and then you may use them.

## Afterwards, only when asked

- A shopping list: `add_to_grocery_list` with the ingredient names of the
  dishes they chose, passing each dish's `recipe_id`. Add only what they asked
  for, keep qualifiers such as "smoked" or "fresh", and tell them the amounts
  are in the books. `get_grocery_list` shows what is already on it.
- Keeping the menu: `create_collection`, then `add_to_collection` for each
  dish. `save_recipe` puts single dishes on "Want to cook".
- Do not save, list or collect anything on the strength of the plan alone.
