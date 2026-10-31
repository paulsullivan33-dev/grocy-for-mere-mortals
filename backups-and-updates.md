# Backups and Updates

**Outcome:** a backup you have
actually made and know the shape of,
and an update procedure that cannot
eat your household.

## The backup is one directory

Chapter 2's payoff: everything that
is yours is in the **data
directory** — `config.php`,
`grocy.db`, uploaded pictures and
files, and caches that don't matter.
Backing up Grocy is copying that
directory:

```
cp -r /path/to/grocy/data \
      /path/to/backups/grocy-2026-10-30
```

The lab's whole household — five
products, a dozen bookings, a
recipe, a chore, a battery, two
users, an API key — was 728 KB of
data directory, of which the
database file itself was 602 KB.
Copy it while Grocy is quiet (or
stop the web server for the seconds
it takes) and the copy is consistent:
SQLite keeps the ledger in that one
file.

Restoring is the same in reverse:
unpack a program, point it at the
backed-up data directory, log in.
Try the restore once — onto a spare
port, a laptop, anywhere — before
you need it. A backup you have never
restored is a hope, not a backup.

**The data directory is also the
secret store.** It holds the
database, and the database holds API
keys in plain text and password
hashes (chapter 11). Backups of
Grocy deserve the same care as
backups of a password vault: keep
them where the household's other
private things are kept.

## Updates replace the program only

Grocy's own update instructions are
short because the split of chapter 2
makes them safe:

1. Back up the data directory
   (above — always first).
2. Overwrite the program files with
   the new release, **keeping the
   data directory**.
3. Check `config-dist.php` in the
   new release for new settings and
   add any you want to your
   `data/config.php`.
4. Open Grocy; the first request
   runs any database migrations into
   the new schema version.

On Linux the release ships
`update.sh`, which does the overwrite
and additionally archives the current
installation into `data/backups`
(archives older than 60 days are
pruned). Docker and Grocy Desktop
users update by updating their image
or app, with the data directory —
a volume, in Docker's case — left
untouched by design.

Two habits make updates boring,
which is the goal. Never edit files
in the program tree: anything you
changed there is overwritten, and
Grocy gives you the data directory
and its `settingoverrides` mechanism
precisely so you don't have to.
And read the new release's changelog
for the fields-and-flags changes —
chapter 5's renamed quantity-unit
fields arrived in the world exactly
that way, announced to anyone
reading, a surprise to everyone
automating blindly.
