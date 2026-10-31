# Best-Before Dates

**Outcome:** you can read Grocy's
four stock warnings — due, overdue,
expired, missing — and know exactly
which list a product lands on and
why. This is the module that pays
for the booking-in habit: it is very
hard to waste food you are reminded
about in time.

## Dates are per bag, not per product

A best-before date belongs to a
**stock entry** — the actual bags
that arrived together — not to the
product. Two deliveries of the same
milk can carry different dates, and
Grocy consumes the earlier one first
and warns about the earlier one
first. When you book stock in
without a date, the product's
*default best-before days* (chapter
5) proposes one; a dated delivery
should always get its real printed
date.

## The four lists

`GET /api/stock/volatile` — and the
stock overview page, which is built
on it — sorts your stock into:

- **Due products** — in stock, with
  a best-before date inside the
  warning window (five days by
  default, a user setting).
- **Overdue products** — in stock,
  and the date has passed. Grocy
  still counts them as stock; food
  past a best-before date is a
  judgement call, not an auto-bin.
- **Expired products** — past the
  date *and* the kind of product
  Grocy treats as expired rather
  than overdue (the distinction is a
  per-product and settings matter —
  in practice, things that must not
  be used after their date).
- **Missing products** — stock has
  fallen below the product's minimum
  amount. These are the shopping
  list's raw material (chapter 8).

## Proven with milk and yoghurt

The lab stocked two dated products
against the lab's date of
2026-10-31:

```
Fresh milk,  best before 2026-11-03
  -> due_products:     [Fresh milk, 2]
Yoghurt pot, best before 2026-10-20
  -> overdue_products: [Yoghurt pot, 1]
expired_products: []
missing_products:   []
```

Milk three days out is *due*;
yoghurt eleven days past is
*overdue*. Nothing was *missing*
because the oats (5 in stock,
minimum 1) and everything else were
above their minimums — the lists are
arithmetic on the ledger, and the
lab's transcripts show the raw
answers.

## Using the lists like a household

- **The overview page is the patrol.**
  Due and overdue sit at the top of
  the stock page; a weekly glance is
  the whole routine.
- **The calendar** (chapter 10's
  neighbourhood) shows best-before
  dates alongside chores and meal
  plans, and Grocy can publish them
  as an iCal feed for a real calendar
  app — documented feature, not run
  in the lab.
- **Eat from the due list.** Meal
  planning (next chapter) gets easy
  when "what needs using" is a list
  rather than a rummage.
- **Book spoiled food out as
  spoiled** (chapter 6). The overdue
  list shrinks, and the waste figures
  — amounts and money — stay honest
  enough to change behaviour.

One caution about dates themselves:
Grocy believes the date you typed.
Book a delivery in with its real
printed date, every time; a month of
lazy "plus 365 days" defaults turns
the due list into fiction, and
fiction is the one thing a ledger
must never contain.
