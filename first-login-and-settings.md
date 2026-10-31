# First Login and Settings

**Outcome:** you are logged in as the
administrator, the default password
is gone, and the handful of settings
every household touches first are
where you want them.

## The default password is public

Grocy ships with one user:

- **Username `admin`, password
  `admin`.**

This is printed in the project's
README, on its website, and in this
guide. It is not a secret; it is a
starting handle. **Change it before
Grocy holds anything real, and
certainly before the server can be
reached from outside your home
network.** An unchanged `admin` /
`admin` on an internet-reachable
Grocy is an unlocked door with a
sign on it, and scanners know the
address of every such door within
hours.

The lab logged in with the default
(a `POST` to `/login`; the form
sends the password base64-encoded in
a field named `password_base64`,
which is a transport detail, not
encryption) and changed the password
immediately — in its case through
the API, which makes the same change
the user menu's password page makes.
Two behaviours worth knowing from
that run:

- The old password stops working at
  once: after the change, logging in
  with `admin` returned the login
  page with `?invalid=true`; the new
  password went through to the stock
  page.
- Through the API, a user is edited
  as a **whole object**: sending only
  the new password field is an error
  (chapter 17). The web form does not
  have this trap.

## What a fresh Grocy already has

A first install is not quite empty.
The lab's fresh database contained:

- **Quantity units** `Piece` and
  `Pack` — the counters products are
  counted in (chapter 5).
- **A location** named `Fridge` —
  one of the places stock can live.
- **One shopping list** and the
  `admin` user.

That is all. No demo products, no
sample data — a clean ledger waiting
for its first entries. (The project's
public demo sites are full of sample
households, but those are theirs,
not yours.)

## Settings worth a first visit

Grocy's settings are mostly per-user
(the user menu, top right) plus the
`data/config.php` choices from
chapter 3. The ones a household
usually sets in the first sitting:

- **Currency** — a config setting
  (`CURRENCY`); prices and stock
  values display in it.
- **Language and locale** — config
  default, overridable per user.
  Dates in Grocy's own fields are
  always ISO format (`2027-06-30`),
  whatever the locale.
- **Night mode and the stock page
  options** — per user; the entry
  page setting (`ENTRY_PAGE` in
  config) decides what you land on.
- **Locations and quantity units** —
  these are data, not settings, and
  chapter 5 treats them properly:
  the rooms and shelves your stock
  lives in, and the units it is
  counted in, should match your real
  kitchen before the first product
  is created.

## The check

You are set up when: you can log in
with your new password and not with
`admin`; the stock overview page
loads empty without errors; and
`/api/system/info` — visit it in the
browser while logged in — answers
with the Grocy, PHP and SQLite
versions. The lab's answer:

```json
{"grocy_version": {"Version": "4.7.1",
 "ReleaseDate": "2026-09-04"},
 "php_version": "8.5.12",
 "sqlite_version": "3.45.1",
 "db_version": 257, ...}
```

That endpoint is also the fastest
health check Grocy has: if it
answers, the whole stack is alive.
