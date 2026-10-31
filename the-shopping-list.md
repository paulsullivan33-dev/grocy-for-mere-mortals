# The Shopping List

**Outcome:** products on the
shopping list — added by hand, and
by Grocy itself from what stock is
missing — with amounts that mean
what you think they mean.

## One list, or several

Grocy starts with a single default
shopping list (id 1). Multiple lists
can be switched on
(`FEATURE_FLAG_SHOPPINGLIST_MULTIPLE_LISTS`)
for households that shop at several
places — one list per shop is the
usual pattern. A list is a set of
rows: a product, an amount, a note,
and a *done* flag you tick in the
shop. Rows for things that are not
products yet can be typed as free
text and turned into products later.

## Adding by hand, for real

The lab added its oats:

```
POST /api/stock/shoppinglist/add-product
{"product_id": 1, "list_id": 1,
 "product_amount": 2,
 "note": "Running low by December"}

GET /api/objects/shopping_list
-> [{"product_id": 1, "amount": 2,
     "note": "Running low by December",
     "done": 0, ...}]
```

Adding a product that is already on
the list does not duplicate the row:
the amounts add up. (Send no note
the second time and the note is
cleared — the row is rewritten with
what you sent, so send the note
again if you want to keep it.)

## The trap this chapter exists for

The amount field is named
**`product_amount`**. In the lab's
first try it sent `amount` — a
reasonable guess — and the API
answered success and added the
product with amount **1**, the
default. Nothing complained; the
wrong field was simply ignored.
Only reading the list back showed
the truth. Grocy's shipped OpenAPI
document names every field, and the
rule generalises far beyond this
endpoint:

**After any API write, read the
thing back.** A 200 or 204 means
"accepted", not "did what you
meant". The transcripts
(`t09`/`t10`) keep both attempts so
the shape of the trap stays visible.

## Letting stock write the list

The list's real party trick is that
stock maintains it. Endpoints exist
to add, in one call each:

- **Missing products** — everything
  below its minimum stock amount
  (`.../shoppinglist/add-missing-products`).
- **Overdue products** and
  **expired products** — replacing
  what time took
  (`add-overdue-products`,
  `add-expired-products`).

Each takes the amount needed to
restore the minimum (or a fixed
amount, your setting) and skips
products already on the list. Run
"add missing" before a shop and the
list is the cupboard's own account
of what it needs — provided, as
ever, the bookings were kept up.
The lab verified the underlying
lists live (chapter 7's volatile
stock); the one-call adds are the
same arithmetic wearing a button,
and are labelled documented here
rather than run, because with
nothing missing they would have
added nothing and proved nothing.

## In the shop

The list page is made for a phone:
tick items as they go in the
trolley, and ticked items can be
booked straight into stock on
return — the loop back to chapter 6.
A thermal-printer option for paper
lists exists in the config for
households with the printer; the
lab did not have one and did not
pretend to.
