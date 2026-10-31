# Recipes and Meal Planning

**Outcome:** a recipe that knows its
ingredients as *products*, can say
whether your stock can fulfil it,
and what it costs — and an honest
map of the meal plan that sits on
top.

## A recipe is a stock document

In Grocy a **recipe** is not just
instructions. Its ingredient list
(`recipes_pos`, one row per
ingredient) names **products** and
amounts, which wires the recipe into
the ledger. That buys three things
a recipe on paper cannot do:

- **Fulfilment** — can the cupboard
  make this, right now?
- **Cost** — what the ingredients in
  stock were bought for.
- **Consumption** — cooking the
  recipe books its ingredients out
  in one action (`POST
  /api/recipes/{id}/consume`), and
  whatever is missing can be put on
  the shopping list in one action
  too (`add-not-fulfilled-products-to-shoppinglist`).

Recipes also nest — a sauce recipe
can be an ingredient of a pasta
recipe — and carry the usual fields
(name, description, servings, a
picture) for actually cooking from.

## Proven with overnight oats

The lab's recipe was deliberately
small — one ingredient, the oats of
chapters 5 and 6:

```
POST /api/objects/recipes
{"name": "Overnight oats", ...}
POST /api/objects/recipes_pos
{"recipe_id": 1, "product_id": 1,
 "amount": 1}

GET /api/recipes/1/fulfillment
-> {"need_fulfilled": 1,
    "missing_products_count": 0,
    "costs": 1.85,
    "costs_per_serving": 1.85,
    "product_names_comma_separated":
      "Rolled oats"}
```

`need_fulfilled: 1` — five bags in
stock, one needed. The cost, 1.85,
is the purchase price of a bag from
the stock ledger: recipe costs in
Grocy are your real prices, not
estimates. Had the oats been
missing, the same call reports the
count of missing products, and the
shopping-list endpoint above exists
to close exactly that gap.

Amounts in recipes deserve one
warning: an ingredient amount is in
the product's *stock* unit unless
you say otherwise, and "1 bag" of
oats as one serving is the lab's
simplification, not a serving
suggestion. Real recipes want the
quantity-unit conversions of
chapter 5 set up with some care —
grams against bags — or the
fulfilment maths gets silly.

## The meal plan

On top of recipes sits the **meal
plan**: a calendar week where each
day gets a recipe (or a note).
Grocy then knows what the week will
consume, which is the input for its
best trick — generating the week's
shopping list from the plan minus
what stock already holds. The meal
plan has its own feature flag
(`FEATURE_FLAG_RECIPES_MEALPLAN`)
and its own first-day-of-week
setting.

Honest label: the lab built and
proved the recipe and fulfilment
half. The meal plan calendar itself
— placing recipes on days in the web
pages and generating the week's
list — is taught here from the
project's documentation and was not
run; it stands on the same
fulfilment arithmetic the lab did
prove, but the clicking is
unverified, and `criteria.md` says
so in those words.

## Recipes from outside

Grocy's recipe editor is manual —
you type your household's recipes
once, in its form. There is no
built-in importer that reads the
web's recipe sites in this release;
households typically enter their
rotation (most homes cook a few
dozen dishes, not a few hundred)
and let the list grow. Enter the
ten meals you actually repeat first:
fulfilment and costing only help for
recipes that exist.
