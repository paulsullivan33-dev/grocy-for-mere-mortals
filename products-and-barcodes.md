# Products and Barcodes

**Outcome:** a product exists the way
Grocy thinks of one — a named thing,
living in a location, counted in a
unit, carrying a barcode — ready for
stock to arrive in chapter 6.

## A product is a kind, not an item

The distinction the whole system
stands on: a **product** is the kind
of thing — "rolled oats" — while
**stock** is the actual bags, each
with its own date and price. You
create a product once and book bags
against it forever. Products live
under Master data in the web pages,
and under `/api/objects/products` in
the API.

The fields that matter at creation:

- **Name** — what your household
  calls it. You will search by this,
  not by the manufacturer's name.
- **Location** — the product's home:
  where it normally lives (the
  lab created a location named
  `Pantry shelf` first, the same way:
  a name, an optional "is freezer"
  flag, nothing more).
- **Quantity units** — up to three,
  and the difference is real:
  `qu_id_stock` is the unit stock is
  counted in; `qu_id_purchase` the
  unit you buy it in; Grocy converts
  between units you define
  conversions for. A bag of oats
  uses `Piece` for both, which is
  why beginners should start with
  Piece-for-everything products.
- **Minimum stock amount** — below
  this, the product counts as
  *missing* (chapter 7).
- **Default best-before days** — when
  you book stock in without a date,
  Grocy proposes purchase date plus
  this many days.

Field names change between versions,
and old tutorials bite: Grocy 4.x
calls the unit fields `qu_id_stock`
and `qu_id_purchase`. Sending the
older `quantity_unit_id_purchase`
name to the 4.7 API answers, word
for word:

```
table products has no column named
quantity_unit_id_purchase
```

The lab's first attempt is kept in
`grocy-lab/transcripts/` as evidence
— and as the reason this guide names
fields from the release's own
OpenAPI document, not from memory.

## Creating one, for real

Through the API (chapter 11 explains
the key in the header):

```
POST /api/objects/products
{"name": "Rolled oats",
 "description": "1 kg bag",
 "location_id": 3,
 "qu_id_purchase": 2,
 "qu_id_stock": 2,
 "min_stock_amount": 1,
 "default_best_before_days": 365}

-> {"created_object_id": "1"}
```

The web pages do the same thing
through a form with pickers for the
location and units — the API answer
is shown here because it can be
checked character by character.

## Barcodes

A product can carry one or more
**barcodes** — the numbers under the
stripes on the packaging. Barcodes
live in their own table
(`product_barcodes`), one row per
barcode, because one product can
wear several (sizes, multipacks) and
because a barcode can carry its own
amount and last price.

The lab attached an invented barcode
`4008400401232` (a made-up number
with a valid EAN-13 check digit —
the last digit computed from the
others, the way real till systems
expect) and then proved the point of
the exercise:

```
GET /api/stock/products/by-barcode/4008400401232

-> {"product": {"id": 1,
     "name": "Rolled oats", ...}}
```

Scanning is just typing very fast:
anywhere Grocy accepts a product, a
scanned barcode resolves through
exactly this lookup. That is why the
author recommends a USB laser
scanner by the fridge — it turns
booking in from data entry into a
beep — and why the phone camera
scanning built into the pages
matters for households without one.

## Looking a barcode up outside

Typing name, unit and dates for
every new product is the chore that
kills household stock-keeping, so
Grocy can ask **external barcode
lookup services** what a barcode is.
A plugin for **Open Food Facts** —
the open food database — ships with
Grocy and is the default
(`STOCK_BARCODE_LOOKUP_PLUGIN` in
the config); other plugins can be
added. When you enter an unknown
barcode, Grocy offers the lookup,
and fills the product form with what
comes back.

Honest label: the lookup talks to
services on the internet and this
guide's lab did not run it — it used
an invented barcode precisely so no
outside database was involved. What
the lab verified is the local half:
barcodes stored, resolved and used.
What the lookup returns also
deserves a check before you save —
crowd-sourced food data is good and
improving, and occasionally wrong
about weights and units.
