# What Grocy Is

**Grocy** is a stock-keeping program
for a household. Businesses call this
kind of software **ERP** — enterprise
resource planning — and use it to know
exactly what is in the warehouse.
Grocy's author built the same idea for
a home, after years of running his
household on spreadsheets: a web
application you host yourself that
tracks groceries and household goods,
what they cost, and when they go out
of date — plus the chores, batteries,
equipment and tasks that keep a home
running.

The core of it is a **ledger**. A
ledger is a list of movements, not
just a number: six bags of oats
arrived on this date, at this price,
with that best-before date; one bag
was used on that date. The number in
stock — five bags — is what the
ledger adds up to. Because movements
are recorded, Grocy can answer
questions a shelf count cannot: what
did this cost us over time, which bag
expires first, what did we throw away
spoiled.

## The deal Grocy makes with you

Grocy is **free software**, free to
download and run. In return it asks
for a habit. Nothing in your kitchen
talks to it. Every arrival must be
booked in and every use booked out —
by tapping in the web page, scanning
a barcode, or sending a request to
its API. Skip the bookings and the
ledger drifts from the real cupboard,
and a drifted ledger is worse than
none, because it answers confidently
and wrong.

People make the habit stick in
different ways: a cheap USB barcode
scanner by the fridge (the author's
own recommendation), the phone
camera, a tablet on the wall, or
booking the whole shopping trip in at
once when the bags come home. What
matters is that the booking is part
of the routine, like putting the food
away itself. Chapter 16 is honest
about households where that never
takes: for them Grocy is the wrong
tool, and chapter 15 names lighter
ones.

## What it covers in a home

Grocy is organised as modules, each
of which can be switched off in its
config file if your household doesn't
want it:

- **Stock** — products, their
  locations, amounts, prices and
  best-before dates.
- **Shopping list** — one or more
  lists, fed by hand or automatically
  from what stock is missing.
- **Recipes** — ingredient lists that
  know what a meal costs and whether
  your stock can fulfil them, and a
  meal plan calendar.
- **Chores** — recurring household
  jobs with a schedule and a log of
  who did them, when.
- **Batteries** — rechargeable
  batteries and their charge cycles.
- **Equipment** — the household's
  machines and where their manuals
  and details live.
- **Tasks** — a plain to-do list.
- **Calendar** — a combined view of
  due dates, chores and meal plans,
  exportable as an iCal feed.

Underneath all of it sits a full
**REST API** — the web pages
themselves are built on it — so
anything the pages can do, a script
or another program can do too.
Chapter 11 takes that seriously; in
this guide's lab, most of the proof
ran through the API for exactly that
reason.

## What it is not

It is not a shopping service: it
never orders anything, holds no
account with any shop, and knows
prices only because you typed them.
It is not a family organiser in the
cloud sense — there is no Grocy
company and no account to create;
the software runs on your server and
your data stays in one folder on it.
And it is not automatic: if nobody
books the milk out, Grocy will go on
believing you have milk. That
limitation is the price of the
ledger's accuracy, and this guide
does not hide it.
