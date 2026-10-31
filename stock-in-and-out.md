# Stock In, Stock Out

**Outcome:** stock booked in with a
price and a date, some of it booked
out again, and the numbers — amount
and value — proven at each step.
This is Grocy's daily loop; every
other module feeds on what happens
here.

## The four movements

Every change to stock is a
**booking** — a dated ledger entry —
of one of these kinds:

- **Purchase** — stock arrives, with
  a price. (Also how gifts and
  home-grown produce arrive; the
  price can be 0.)
- **Consume** — stock is used up:
  eaten, drunk, cleaned with.
- **Inventory correction** — the
  shelf and the ledger disagree, and
  the shelf is right. Grocy sets the
  amount to what you counted and
  books the difference honestly as a
  correction, not a quiet edit.
- **Transfer** — stock moves between
  locations (cupboard to freezer)
  without being used.

There is also **open** — marking a
package opened, for products whose
life shortens once opened — and
every booking can be **undone** from
the stock log, which writes a
reversing entry rather than erasing
history. The ledger (`stock_log`, one
row per booking, visible in the
pages and at `/api/stock/...`) is
the system's memory: amounts are
always "the log, added up".

## Booking in, for real

The lab bought six bags of the oats
from chapter 5:

```
POST /api/stock/products/1/add
{"amount": 6,
 "best_before_date": "2027-06-30",
 "purchased_date": "2026-10-30",
 "price": 1.85,
 "location_id": 3}
```

Grocy answered with the stock entry
it created — amount 6, transaction
type `purchase`, its own stock id —
and the product's stock then read:

```
stock amount: 6 | value: 11.10
```

Six bags at 1.85: the value is the
ledger's prices, added up. Grocy
tracks what your stock is *worth* at
purchase prices, which is what makes
the waste reports in chapter 7 sting
pleasantly — spoiled food has a
number on it.

The same call exists by barcode —
`/api/stock/products/by-barcode/{barcode}/add` —
which is the shape scanner-driven
booking takes: beep, amount, done.

## Booking out, for real

Breakfast:

```
POST /api/stock/products/1/consume
{"amount": 1, "spoiled": false}

stock amount: 5 | value: 9.25
```

One bag left the ledger at the price
it arrived at, oldest stock first —
Grocy consumes in best-before order
unless you name a specific stock
entry. The `spoiled` flag matters:
consume-as-eaten and
consume-as-thrown-away are different
rows in the log, and only one of
them is a success story. Marking
spoiled stock honestly is how the
"what we waste" numbers stay true.

## Reading stock back

Three views, all live-checked:

- **One product** —
  `GET /api/stock/products/{id}`
  gives the product, its total
  amount, and its value (the numbers
  above).
- **Everything** — `GET /api/stock`
  is the stock overview: one row per
  product in stock with amount,
  best-before date of the next-expiring
  entry, and value. The lab's whole
  household at that moment was one
  line: `Rolled oats -> 5 pieces |
  best before 2027-06-30 | value 9.25`.
- **One location** —
  `GET /api/stock/locations/{id}/entries`
  answers "what is on this shelf",
  the question you ask standing in
  front of it.

## The habit, stated plainly

Booking takes seconds when it is
part of the routine — scan the
shopping as it goes in the cupboard,
book the bag out when it goes in the
bin — and it compounds painfully
when it is saved up. A week of
unbooked eating is a stocktake, not
a tidy-up. Chapter 14's first recipe
is the stocktake, because every real
household needs it sometimes;
chapter 16 is honest that needing it
*weekly* means the system, not the
household, is failing.
