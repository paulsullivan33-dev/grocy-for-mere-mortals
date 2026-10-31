# Grocy for Mere Mortals

**Grocy** calls itself "ERP beyond your
fridge". ERP is the software businesses
use to track stock — what came in, what
went out, what is about to run out.
Grocy is that idea, shrunk to fit a
household and run on a server in your
own home. It tracks the food in your
cupboards and fridge with its
best-before dates, the shopping list
that replaces it, the recipes that use
it, and — because a household is more
than food — recurring chores, the
batteries in your remotes, equipment
and tasks.

The honest catch is in the first
sentence: Grocy knows only what you
tell it. Every bag of oats has to be
booked in when it arrives and booked
out when it is eaten, by a person, a
barcode scan, or a program using its
API. Households that keep that habit
get a pantry that answers questions —
what do we have, what expires this
week, what must go on the list.
Households that don't get a database
that slowly drifts away from the real
cupboard. This guide shows the
machinery working for real, and
chapter 16 is plain about that bargain.

The lab behind this guide ran the real
thing: **Grocy 4.7.1**, the official
release download, on a small Linux
machine — unpacked, served, and driven
mostly through its own **REST API**,
the same API its web pages use. It
created a location, a product with a
barcode, bought six bags of oats into
stock, ate one, and watched the count
and the value change. It stocked milk
that expires in three days and a
yoghurt already past its date, and
read back Grocy's own lists of what is
due and what is overdue. It wrote a
recipe and asked whether the cupboard
can fulfil it (it could, and Grocy
priced it). It ran a chore, registered
a battery, made a second user, changed
the default administrator password,
and minted an API key through the real
web pages. Where the lab hit walls —
an old field name the API no longer
accepts, an amount field that is
silently ignored under the wrong name
— the walls are in the guide, word for
word.

What the lab could not do, it did not
fake. Nobody clicked through the web
interface for hours; the pages were
loaded and read, the login was real,
and everything else went through the
API the pages themselves use, which
proves the same machinery. Barcode
lookup against outside services,
meal planning on the calendar, and
the phone and scanner companions are
taught from the documentation and
labelled as such. `criteria.md` keeps
the full ledger of what was run and
what was only read.

## What this guide covers

- [**What Grocy is**](what-grocy-is.md) —
  the mental model: a stock ledger for
  a household, and the booking-in and
  booking-out habit it runs on
- [**The moving parts**](the-moving-parts.md) —
  the PHP program, the SQLite database,
  the data directory that holds
  everything, and the API underneath
  the web pages
- [**Installing Grocy**](installing-grocy.md) —
  the release download, the PHP version
  it insists on, the extensions it
  checks for, and the one config file
- [**First login and settings**](first-login-and-settings.md) —
  the default `admin` password and why
  it must change at once, currency,
  locations and quantity units
- [**Products and barcodes**](products-and-barcodes.md) —
  what a product is, where it lives,
  the units it is counted in, and the
  barcode stuck on it
- [**Stock in, stock out**](stock-in-and-out.md) —
  buying, consuming, opening and
  correcting, and the ledger every
  movement leaves behind
- [**Best-before dates**](best-before-dates.md) —
  due, overdue, expired and missing —
  the four lists Grocy keeps for you
- [**The shopping list**](the-shopping-list.md) —
  the list that stock writes for you,
  and the field-name trap in its API
- [**Recipes and meal planning**](recipes-and-meal-planning.md) —
  recipes that know what they cost and
  whether your cupboard can fulfil
  them, and the meal plan calendar
- [**Chores, batteries and tasks**](chores-batteries-and-tasks.md) —
  the household beyond the fridge:
  recurring jobs, charge cycles,
  equipment and tasks
- [**The API**](the-api.md) — API keys,
  the built-in API browser, and why
  the API is the product, not an extra
- [**Users and permissions**](users-and-permissions.md) —
  one account per person, what the
  permission switches actually gate,
  and the shared-household model
- [**Backups and updates**](backups-and-updates.md) —
  why backing up Grocy means copying
  one directory, and how updates
  replace everything else
- [**Recipes**](recipes.md) — stocktake
  day, expiry patrol, a second user,
  and driving Grocy from a script
- [**The neighbours**](the-neighbours.md) —
  paper lists, spreadsheets, hosted
  inventory apps and smart-home
  platforms: what each does that
  Grocy doesn't, and vice versa
- [**Limits**](limits.md) — the
  discipline it demands, one household
  per install, and what it never tries
  to be
- [**Troubleshooting**](troubleshooting.md) —
  the PHP version wall, the missing
  extension, the field name from an
  old tutorial, and the amount that
  silently stayed 1
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Running a business on it — Grocy is
built for one household, and chapter
16 says where that shows. Feeding it
automatically from a supermarket's
systems; no such feed exists, and
barcode lookup services are described,
not run. The Windows desktop wrapper
and the phone companions are pointed
at, not taught step by step.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing that everything lives in one
data directory makes backups,
upgrades and half of troubleshooting
obvious later. Chapters 3 and 4 get
you a running Grocy with a safe
password. Chapters 5 to 8 are the
daily loop — product, stock in, stock
out, list — and are worth doing once
with something real from your own
cupboard. The lab's transcripts live
in `grocy-lab/transcripts/`; the API
key and passwords used there were
the lab's own and are redacted.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [PHP for Mere Mortals](https://paulsullivan33-dev.github.io/php-for-mere-mortals/)
- [SQLite for Mere Mortals](https://paulsullivan33-dev.github.io/sqlite-for-mere-mortals/)
- [APIs for Mere Mortals](https://paulsullivan33-dev.github.io/api-for-mere-mortals/)
- [Home Assistant for Mere Mortals](https://paulsullivan33-dev.github.io/home-assistant-for-mere-mortals/)
- [NocoDB for Mere Mortals](https://paulsullivan33-dev.github.io/nocodb-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/grocy-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/grocy-for-mere-mortals/blob/main/LICENSE-CODE).
