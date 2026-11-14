---
name: get-started
description: >-
  Find which cookbook has a recipe, look inside a cookbook's index, and work with the user's own CookCook library — the books they own, saved recipes, collections and grocery list. Also covers the two things that start from a photo: reading the spines on a picture of their bookshelf to add those books, and transcribing a photographed recipe page into their own captured recipe, including from a book CookCook does not have.
---

# CookCook

CookCook is a catalogue of cookbooks and the recipes in them: each book's index, with recipe titles, page numbers and ingredient names.

## Finding recipes and books

- "Which cookbook has a recipe for pumpkin soup?" → `search_recipes` with the dish or ingredient. Answer with each recipe's title, its book and page, and link to the recipe's CookCook URL.
- "What's in *Ottolenghi Comfort*?" → `search_books` to find the book, then `get_book` for its details and recipe index.
- `get_recipe` gives one recipe's ingredients and its book. For a signed-in user, and for many recipes, the ingredients are the full lines with quantities and the yield (`quantities: true`); otherwise their names.
- CookCook never gives a book's method: point the user to the book and the page. Never make up a quantity CookCook did not give.
- A signed-out user who asks for quantities: call `get_my_recipe`, which asks them to connect their CookCook account or create one.
- Ingredient names do not prove a dish is safe for an allergy, vegan or gluten-free. Say what the ingredient list shows and send the user to the book when a restriction is strict.
- Show a book's cover (`cover_image_url`) beside it when you can display images, and always link to the CookCook URL of the books and recipes you mention.

## The user's own library

These tools need the user to connect their CookCook account; ChatGPT asks them to sign in the first time one is used.

- "What can I cook with leeks from my books?" → `search_my_books` searches the recipes in the cookbooks they own. Prefer it over `search_recipes` when they ask what they can cook.
- `my_books`, `my_saved_recipes`, `my_collections` and `get_collection` show their library.
- `get_my_recipe` gives their own copy of a recipe, with the method when they captured it or hold the book as an ebook, and otherwise with the quantities when CookCook has them.
- `get_grocery_list` and `add_to_grocery_list` work on their grocery list. Add only what they asked for.
- `save_recipe`, `like_recipe`, `rate_recipe`, `mark_cooked`, `log_tasting`, `add_to_my_books`, `wishlist_book`, `like_book`, `rate_book`, `comment_on_book`, `create_collection`, `add_to_collection` and `remove_from_collection` change their library, as the buttons on CookCook do. Use them only when the user asks, and never make up ratings, notes or comments.

## A photo of a bookshelf

When the user shows a picture of their bookshelf, read the spines yourself —
the tools take text, never the image.

- Write down the title, and the author where the spine shows one, left to
  right. Read only what is legible: a spine that is turned away, cut off or
  blurred is not a title. Never guess or complete a title from what you know
  of cookbooks.
- `match_shelf_books` with the titles you read, one call for the shelf (up to
  40). It writes nothing.
- Show the user what was found, what matched nothing, and how many spines you
  could not read, and ask whether to add them. A match under 0.7 confidence is
  a question: put it to them with its `alternatives` instead of picking.
- Only then `add_books_to_library` with the books they confirmed. Never on the
  strength of the photo alone.
- A book that matched nothing is still on their shelf: they can photograph a
  recipe page from it and capture it anyway.

## A photo of a recipe page

When the user photographs a recipe from a book they own, transcribe it
yourself and it becomes their own captured recipe. Offer this whenever they
mention cooking from one of their books.

- Transcribe the title as printed, the page number from the page, the
  ingredient lines word for word with the book's own amounts and units, and
  the method step by step.
- Never invent a quantity, an ingredient, a step, a page number or an ISBN,
  and never save a placeholder like "[unreadable]" either: everything in
  `ingredients` and `instructions` is parsed and saved as the recipe, so a
  marker becomes a junk line on the user's own sheet and their grocery list.
  When a line will not come out of the photo, ask for another photo of that
  part or have the user read it out, and capture only once every line was read
  or supplied. If they would rather not, save nothing. Ask for the rest when
  the recipe runs onto another page.
- Ask which book the recipe is from rather than assuming. Look on their own
  shelf first (`my_books`, `search_my_books`), then `search_books`, and show
  the match to confirm. Pass `recipe_id` when CookCook's index already has the
  recipe — it attaches the capture to that entry — and `isbn` otherwise.
- If the book is not in CookCook at all, say so and capture the recipe anyway
  — `capture_recipe` works with no book attached. Then offer to add the book
  by its ISBN with `add_books_to_library` and attach the capture with
  `link_recipe_to_book`.
- Show the whole transcription back for checking, then `capture_recipe`. It
  becomes the user's own pending capture, not a public catalogue recipe.
