# The Moving Parts

Grocy has fewer parts than most web
applications, and knowing which part
holds what makes the rest of this
guide — especially backups and
troubleshooting — mostly obvious.

## The program

Grocy is written in **PHP**, a
language made for web pages, and
built on a small framework (Slim,
with Blade templates for the pages).
The release download is the whole
program: unpack it and there is
nothing to compile and no installer
to run. A web server offers its
`public` folder to the world; every
request that is not a plain file is
handed to `public/index.php`, which
routes it to a controller.

Two consequences matter. First, the
program is **replaceable**: an update
is "unpack the new release over the
old one", because nothing of yours is
in the program folders. Second, PHP
itself is a real requirement with a
version attached — Grocy 4.7 insists
on PHP 8.5 and checks, as chapter 3
shows. A Grocy that will not start is
usually a PHP that is too old or an
extension that is missing, and Grocy
says which, in so many words.

## The data directory

Everything that is *yours* lives in
one directory, named by the
`GROCY_DATAPATH` setting (or simply
the `data` folder inside the program
if you don't set it):

- `config.php` — your copy of the
  settings, made from the shipped
  `config-dist.php`.
- `grocy.db` — the database.
- `viewcache/` — compiled page
  templates; safe to delete, it is
  rebuilt.
- Folders for uploaded pictures and
  files appear here as you use them.

In this guide's lab the data
directory held a 602 KB database and
a few cache files — 728 KB in total —
containing the whole household the
lab built: products, stock, recipes,
users, an API key. Copy that one
directory and you have copied Grocy,
which is the whole of chapter 13.

## The database

The database is **SQLite**: one file,
no database server to install or
administer. SQLite is a real SQL
database — Grocy's tables hold
products, stock ledger entries (the
`stock_log` of every booking),
recipes, chores and the rest — but it
travels like a document. That is why
the data directory trick works.

SQLite also sets a scale limit worth
knowing: it handles one writer at a
time. For a household — a handful of
people booking shopping in and out —
that is never the bottleneck. Grocy
is a household tool, and its database
choice says so.

## The API underneath

Grocy's web pages do not talk to the
database directly in the old style;
the frontend calls a **REST API** —
URLs like `/api/stock` and
`/api/objects/products` that answer
in **JSON** — for nearly everything.
The release ships the API's full
description as an OpenAPI document
(74 paths, 87 operations in 4.7.1)
and serves a browsable copy at
`/api` on every install.

This is not a bolt-on. It means the
program has two equal doors — the
pages and the API — and anything one
can do, the other can: companion apps,
barcode-scanner software and home
automation all use the API door.
Authentication differs by door: the
pages use a login session, the API
uses **API keys** (long random
tokens, created in the pages, sent in
a `GROCY-API-KEY` header). Chapter 11
creates one properly and drives the
whole stock flow with it.
