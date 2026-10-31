# Troubleshooting

Every entry here was either hit by
this guide's lab, word for word, or
names the check that would have
found it fastest.

## "PHP 8.5.0 is required, however you are running …"

Grocy's prerequisite checker refuses
to run on older PHP, and this exact
message is the refusal. It is the
most common first-install wall: a
perfectly current server OS can
ship PHP 8.3, as this lab's did.

- Check: `php --version` — on the
  PHP your **web server** uses,
  which may not be the one in your
  shell.
- Fix: chapter 3's routes — Grocy
  Desktop, the Docker image, or a
  maintained PHP repository. There
  is no config setting that relaxes
  the check, by design.

## A named extension is missing

The checker's other refusal names
the extension (`fileinfo`,
`pdo_sqlite`, `gd`, `ctype`, `intl`,
`zlib`, `mbstring`). Install the
matching PHP extension package for
your PHP version and restart the
web server. `php -m` lists what the
command-line PHP has; if the list
disagrees with the error, your web
server is running a different PHP
or a different `php.ini` than your
shell — find that file first.

## "config.php in data directory … not found"

You skipped the one config step:
copy `config-dist.php` to
`config.php` **inside the data
directory** — not the program root —
and make sure the web server is
looking at the data directory you
think it is (`GROCY_DATAPATH` is the
override; chapter 3). The same
family of error complains about a
data directory that is not writable:
the web server's user must be able
to create `grocy.db` and
`viewcache` in it.

## Login fails with the right password

Two usual causes. The password is
being sent wrong by a password
manager mangling the form — type it
once by hand to see. Or the account
really has a different password: on
a fresh install it is `admin` /
`admin` and — see chapter 4 — it
should not stay that way. (Grocy's
own habit to know: failed logins
redirect to `/login?invalid=true`,
which is how the lab proved the old
default was dead after the change.)

## The API says success, but nothing changed

The standing trap of this guide
(chapter 8): a field under the
wrong name — `amount` instead of
`product_amount` — is **silently
ignored**, the call returns success,
and the default applies. Defences:

1. Send field names from the
   shipped `grocy.openapi.json`,
   not from memory or old posts.
2. Read the object back after every
   write and compare what you meant.
3. When a field name produces a
   *loud* error — Grocy 4.7's
   `table products has no column
   named quantity_unit_id_purchase`
   — believe it: the field was
   renamed (now `qu_id_purchase`),
   and the error is the documentation.

## A 500 error editing a user

`PUT /api/users/{id}` wants the
**whole user object** — username,
first and last name, and the
password if changing it. Sending
only the field you care about can
end in a server error rather than a
tidy refusal (the lab's partial
object did exactly that). Fetch the
user, change the copy, send the
copy back.

## First page takes a minute; the rest are fast

Normal, once: the first request
creates the database, runs hundreds
of migrations, and compiles every
page template into `viewcache`. On
a small or busy machine the lab
measured about a minute. If *every*
page is slow, the viewcache or the
data directory is on slow storage,
or something else on the machine is
eating it — Grocy itself is a light
application.

## The stock count is wrong

It is not the database's fault; it
is an unbooked movement (chapter
6). Find it honestly: check the
stock log for the product, count the
shelf, and book an **inventory
correction** for the difference.
Do not edit the database file to
make the number pretty — the next
booking would resurrect the lie,
and corrections exist precisely so
the log can say "we counted, it was
this" in the open.

## The API key stopped working

Keys die by deletion (Manage API
keys page), by their user's
password or account changing, or by
being pasted with stray whitespace
by a helpful editor. A known-good
test that needs no secrets in
chat or tickets:

```
curl -H "GROCY-API-KEY: <key>" \
     http://your-grocy/api/system/info
```

Answers with versions: the key, the
server and the API are all alive.
401: the key is wrong or gone. And
if the key has leaked — it is a
password, in plain text in the
database (chapter 11) — delete it
and mint a fresh one; there is no
"change" for a key, only replace.
