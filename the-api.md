# The API

**Outcome:** an API key created the
proper way, used to drive Grocy from
the command line, with a wrong key
refused — and a clear picture of why
the API, not the pages, is Grocy's
real interface.

## Why the API is the product

Grocy's own web frontend is built on
its REST API: the pages call the
same URLs this chapter calls. The
release ships the contract as an
OpenAPI document — in 4.7.1, **74
paths and 87 operations** — and every
installation serves a browsable,
try-it version at `/api` (title:
"REST API browser"). Whatever exists
in Grocy, some endpoint names it:
objects (`/api/objects/{entity}` for
products, locations, chores and
every other table), stock movements
(`/api/stock/...`), recipes, chores,
batteries, users, files, and system
information.

That shape — generic object access
plus specific action endpoints — is
the whole learning curve. Master
data is created and read through
`objects`; *events* (buy, consume,
execute, charge) have their own
verbs. Every example in this guide
so far has used exactly those two
families.

## Keys: what they are

An **API key** is a long random
token tied to a user. Programs send
it in a header:

```
GROCY-API-KEY: <the key>
```

Facts proven in the lab:

- A request with the key returns the
  data; the same request with a
  wrong key returns **401**.
- A key acts *as its user* — its
  permissions are the user's
  permissions (chapter 12).
- Keys are stored in the database
  **as plain text** — visible in the
  `api_keys` table to anyone who can
  read `grocy.db`. Treat the data
  directory as holding secrets,
  because it does (chapter 13).

## Creating a key, for real

Keys are created in the web pages:
**user menu → Manage API keys →
New API key**, with a description.
The lab did it the way the page
itself does, with a logged-in
browser session from the command
line:

```
POST /login            (admin, new password)
GET  /manageapikeys/new?description=Guide+lab+key
  -> 302 to /manageapikeys?key=2
```

and the new key — shown by the app —
then drove every API call in
chapters 5 to 10. No database
surgery, no back door: the key in
the transcripts was born in the real
user interface flow, which is the
honest way to make one and the only
way this guide teaches. (The key
itself is redacted in the
transcripts; it was the lab's, and
the lab is gone.)

Name keys after their job — "kitchen
scanner", "backup script" — and
delete them in the same page when
the job ends. A key that outlives
its purpose is a password you forgot
you made.

## Driving it from a script

Everything in chapters 5 to 10 was
`curl` against this API, which is
the recipe shape for any automation:
a home-automation system booking the
milk out, a scanner app on a phone,
a nightly script listing what
expires this week —

```
GET /api/stock/volatile
```

— and warning whoever wants warning.
Two habits from the lab, repeated
because they are the difference
between automation that works and
automation that lies:

1. **Read back after writing** —
   chapter 8's silently-ignored
   `amount` field is the standing
   example.
2. **Send whole objects to edits** —
   Grocy's `PUT` endpoints expect
   complete records; a partial one
   can be a 500 error, as chapter 17
   records.
