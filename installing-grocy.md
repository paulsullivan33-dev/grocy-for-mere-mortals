# Installing Grocy

**Outcome:** Grocy unpacked, its one
config file in place, and the login
page answering in a browser. Grocy's
own install notes fit in five bullet
points; this chapter is those points
with the checks that prove each one.

## Get the release

Grocy is distributed as a single zip
from its GitHub releases page (the
site's "latest release" link points
at the same place). This guide's lab
downloaded **Grocy 4.7.1** (released
2026-09-04), a 43.6 MB zip. The
project does not publish a checksum
for the zip, so the lab computed its
own SHA-256 —
`c4a10e96ed76664baf465e9b27dc7a31a`
`a08c0e3ad876afdfae4f1ff9ab2fc19` —
recorded here so the exact artefact
is named; if you verify downloads as
a habit, compare against a fresh
download of your own rather than
trusting this number blindly.

Unpack it anywhere. The top level
holds the program (`app.php`,
`controllers`, `views`, `public`) and
two files to know by name:
`config-dist.php`, the settings
template, and `version.json`, which
states the version in plain text.

## The PHP it insists on

Grocy is unusually strict about its
platform, and it checks before it
runs anything. The 4.7.1 release
requires:

- **PHP 8.5 or newer** — the shipped
  prerequisite checker states
  `REQUIRED_PHP_VERSION = '8.5.0'`
  and refuses older PHP outright.
- **SQLite 3.40 or newer.**
- **PHP extensions:** `fileinfo`,
  `pdo_sqlite`, `gd`, `ctype`, `intl`,
  `zlib`, `mbstring` (plus core ones
  it lists for completeness: `filter`,
  `iconv`, `tokenizer`, `json`).

That version demand is the first
wall most installers meet: a current
long-term-support Linux server often
ships PHP 8.3, which Grocy 4.7 will
not accept. The lab met exactly that
wall — the machine's PHP was 8.3.6 —
and chapter 17 tells the story.
Practical routes, in the order most
people take them:

1. **Grocy Desktop** — the project's
   own Windows wrapper; download,
   double-click, done. For a household
   that just wants Grocy on a PC, this
   is the intended easy path.
2. **Docker** — the community image
   maintained by linuxserver.io,
   named in Grocy's README, bundles
   the right PHP.
3. **A PHP from a maintained
   repository** — on Debian/Ubuntu
   the ondrej/php repository carries
   current PHP versions for current
   systems.
4. **A shared host or server** whose
   PHP is already new enough.

This guide's lab took a laboratory
variant of route 3: it downloaded
the distribution `.deb` packages for
PHP 8.3 — too old — and then for
**PHP 8.5.12** from the ondrej/php
repository, unpacked the packages
into a private folder with
`dpkg-deb -x` (nothing installed
system-wide), and ran that PHP with
its `php.ini` pointed at the
unpacked extensions. The route is
described in full in `criteria.md`;
it proves the version and extension
lists are exactly as stated, because
the checker passed against them.
Your install should use a properly
installed PHP; the lab's extraction
trick is a way to *test*, not a way
to *run a household*.

Check before you start, on any
route:

```
php --version
php -m
```

The first must say 8.5 or newer; the
second must list every extension
named above. Grocy will check again
itself and name anything missing —
but it can only do that once a web
server is already pointing at it.

## The one config file

Copy the template into the data
directory and edit the copy:

```
cp config-dist.php data/config.php
```

The data directory defaults to the
`data` folder in the program tree;
the environment variable
`GROCY_DATAPATH` can point it
elsewhere, which is what the lab did
so that program and data stayed
separate. The settings you are most
likely to touch, all documented in
the file itself:

- `CURRENCY` — three-letter code,
  `USD` by default.
- `DEFAULT_LOCALE` — language, `en`
  by default.
- Feature flags `FEATURE_FLAG_STOCK`,
  `FEATURE_FLAG_RECIPES`,
  `FEATURE_FLAG_CHORES`,
  `FEATURE_FLAG_BATTERIES`,
  `FEATURE_FLAG_TASKS` and more — set
  one to `false` and that module
  leaves the menus entirely.
- `DISABLE_URL_REWRITING` — for web
  servers that cannot do the
  rewriting described next.

## Point the web server at `public`

The web server's document root must
be Grocy's `public` folder — not the
top level. Requests for files that
exist (stylesheets, scripts, images)
are served directly; everything else
must fall through to `index.php`. On
nginx that is the line from Grocy's
README:

```
try_files $uri /index.php$is_args$query_string;
```

On Apache the shipped rewriting
rules do the same job. The lab used
PHP's own built-in development
server with a twelve-line router
script doing exactly this
serve-the-file-or-fall-through work,
on port 18106 — fine for a lab and
for trying Grocy out, not a server to
run a household on.

## First hit

Open the address in a browser. The
first request does real work: Grocy
creates `grocy.db` in the data
directory and runs its database
migrations into it (the lab's fresh
database reported schema version
257). On a slow or busy machine the
first page can take a while — the
lab's took about a minute; later
pages took a second or two. Then:

```
GET /        -> redirect to /login
GET /login   -> 200, title "Login | Grocy"
```

That login page is the install
check: if you can see it, PHP, the
extensions, the config and the
database are all working. Chapter 4
walks through the door — carefully,
because the password it ships with
is printed in the README of every
copy ever downloaded.
