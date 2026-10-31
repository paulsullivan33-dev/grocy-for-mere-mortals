# Users and Permissions

**Outcome:** one account per person,
a working sense of what the
permission switches gate, and the
shared-cupboard model Grocy assumes.

## One household, shared shelves

Grocy's model is a household sharing
one set of shelves. Every user sees
the same stock, the same lists, the
same chores; there are no private
pantries inside one installation.
Accounts exist so the *log* can say
who booked what and who did the
chore — and so each person gets
their own settings, language and
API keys — not to hide the oats from
each other. Households that need
true separation run two Grocys;
chapter 16 is honest about that.

## Creating a user, for real

```
POST /api/users
{"username": "sam",
 "first_name": "Sam",
 "last_name": "Example",
 "password": "a real password"}

GET /api/users
-> user: 1 admin
   user: 2 sam
```

The lab then logged in **as** the
new user through the real login
form — 302 to the stock page — so
"created" means "can walk in", not
just "a row exists". New users get
the configured default permissions
(`DEFAULT_PERMISSIONS` in
`config.php`, `['ADMIN']` out of the
box — see the next section before
you relax about that).

Editing a user follows the
whole-object rule from chapter 11:
a `PUT /api/users/{id}` carries
username, names and (only when
changing it) password, together.
The lab's password change for
`admin` in chapter 4 used exactly
this call.

## Permissions: switches, not roles

Grocy does not do job titles. Each
user carries a set of **permission
flags** — named switches such as
the master `ADMIN`, and per-area
ones under it (stock, recipes,
chores, master data, user
management, and so on) — edited on
the user pages and visible at
`/api/users/{id}/permissions`. Grant
`ADMIN` and the area switches stop
mattering; withhold it and you can
build, say, a shopping-only account
for a kitchen tablet that can book
stock but cannot delete products or
manage users.

Two honest observations. First, the
default for new users is
**`['ADMIN']`** — the shipped config
trusts the household; if your Grocy
can be reached beyond it, set
`DEFAULT_PERMISSIONS` to something
smaller and grant deliberately.
Second, permissions gate *features*,
not *data*: there is no switch that
lets Sam see the stock but not the
prices. That is the shared-household
model again, not an oversight to
work around.

## Other ways in

The config names alternatives this
guide does not run: authentication
can be handed to a reverse proxy
(`REVERSE_PROXY_AUTH_*` settings —
the proxy logs you in and Grocy
trusts its header) or to an **LDAP**
directory (`LDAP_*` settings,
chapter 3's config file lists them),
and auth can be disabled entirely
(`DISABLE_AUTH`) — a setting that
makes sense exactly when something
else is firmly in front of Grocy,
and is a hole in the wall when
nothing is. The default middleware
— Grocy's own logins and sessions —
is what the lab ran and what most
homes should keep.
