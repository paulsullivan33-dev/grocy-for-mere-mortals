# Limits

What Grocy cannot do, and what it
will not do for you. Read this
chapter before the household
commits; most of these are the
price of the things chapter 1
praised.

## It knows only what it is told

The standing limit, said once more
without decoration: Grocy has no
sensors in your kitchen. Every
accuracy it has is borrowed from
your booking habit, and every lapse
is a small lie the ledger will
repeat confidently — amounts,
values, due lists, recipe costs, all
of it. There is no mode where Grocy
notices the shelf itself. If the
household will not book, do not
deploy Grocy at them; a lighter
tool from chapter 15 will serve
them better and be forgiven more.

## One household per installation

Users share everything (chapter
12): there are no private stocks,
no per-person prices, no separated
sub-households under one roof of
data. Two families, a house-share
that keeps food apart, a small
business beside a home — each needs
its own installation. They are
cheap to run (one small PHP app,
one data directory), but the
boundary is real.

## It is not a business system

Despite the "ERP" in its slogan:
no suppliers, no purchase orders,
no invoices, no tax, no multi-site
stock, no audit trail a regulator
would accept. The ledger's undo is
a reversing entry for household
mistakes, not a compliance feature.
A café or shop that starts here
will outgrow it the week it needs
any of those words.

## The database is a household size

SQLite, one file, one writer at a
time (chapter 2). A household will
never feel it. A community fridge
with fifty volunteers booking at
once is beyond the design, and so
is treating Grocy as the data store
for other software to write to —
the API is the only door; the file
is Grocy's own.

## Dates are only as good as typing

Best-before warnings (chapter 7)
run on dates a person typed.
Printed dates vary in meaning —
best before, use by, display until —
and Grocy does not know food safety:
"overdue" is arithmetic, not advice
about whether something is safe to
eat. That judgement stays with the
human holding the yoghurt, where it
belongs.

## Platform demands are real

Grocy tracks the current PHP
closely (8.5 for the 4.7 line) and
will refuse older platforms at the
door (chapter 3). Hosts that upgrade
slowly — cheap shared hosting,
an old NAS's built-in PHP — may
simply never be able to run current
Grocy, and every major release can
move the floor again. Budget a
little platform-keeping into owning
it, or take the Desktop/Docker
routes where the PHP travels with
the app.

## Smaller print

- One currency per installation;
  prices are records, not accounts —
  no conversion, no budgets beyond
  what the numbers let you see.
- Barcode lookup depends on
  outside, crowd-sourced data —
  good, and sometimes wrong
  (chapter 5).
- The interface is a web page: no
  offline mode worth the name on a
  phone in a dead-spot supermarket;
  the list works, live booking may
  have to wait for signal.
- Development is one maintainer's
  hobby project, by its own README's
  cheerful admission — releases are
  regular and the community active,
  but there is no company, no
  support desk, and no promise.
  Your backup (chapter 13) is the
  continuity plan.
