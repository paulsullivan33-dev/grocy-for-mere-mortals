# Chores, Batteries and Tasks

**Outcome:** the non-food modules in
their real shapes — a chore with a
schedule and a log, a battery with a
charge life, equipment and tasks —
each switchable off if your
household doesn't want it.

## Chores

A **chore** is a recurring job: wipe
the shelves monthly, descale the
kettle, change the water filter.
Creating one names it and gives it a
**period** — how the next due date
is worked out:

- **Manually** — no schedule; you
  execute it when it's done.
- **Daily / weekly / monthly /
  yearly** — fixed intervals
  (`period_days` for the counted
  kinds).
- **Interval** and **adaptive** —
  counted from the last execution,
  so a job that slips slips its
  schedule instead of piling up.

Chores can be **assigned** — rotated
among users, given to whoever did
it least, and so on (the
`FEATURE_FLAG_CHORES_ASSIGNMENTS`
flag) — and every execution is a
row in the chore log with who and
when.

The lab created a monthly chore and
executed it, for real:

```
POST /api/objects/chores
{"name": "Wipe the pantry shelves",
 "period_type": "monthly",
 "period_days": 30,
 "track_date_only": true}
-> {"created_object_id": "1"}

POST /api/chores/1/execute
{"done_by": 1}            -> 200
chores_log: 1 done at 2026-10-31
```

`track_date_only` is the humane
setting for housework: the log
records the day, not the minute
nobody cleaned at 14:03.

## Batteries

The **batteries** module tracks
rechargeable batteries — which ones
exist, what they are used in, and
their **charge cycles**: book a
charge (`POST /api/batteries/{id}/charge`)
and Grocy counts cycles and, with a
charge interval set, warns when a
battery is due. For households with
a drawer of AAs and no idea which
are charged, this is the module
that sounds like a joke and turns
out to be the one people keep.

The lab registered one
(`{"name": "AA cells, TV remote"}` —
created, id 1) and did not run
charge cycles against it; the cycle
machinery is documented here, the
registration is live in the
transcripts.

## Equipment

**Equipment** is the registry of
household machines — the washing
machine, the boiler, the mower —
with their details and a place for
manuals and notes (files live in
the data directory, chapter 2). It
has no schedule machinery of its
own; it pairs with chores ("service
the boiler" is a chore; the boiler
is equipment) and with tasks. The
lab describes it from the release's
own structure; it created none, and
says so.

## Tasks

**Tasks** are the plain to-do list:
a name, a due date, a category, a
done flag. No periods, no
assignments engine — deliberately
the simplest module, for the jobs
that are neither stock nor chores.
Like the others it is a feature
flag away from disappearing from
the menus entirely.

## The calendar ties them together

Grocy's **calendar** view overlays
chores due, batteries due,
best-before dates (chapter 7) and
the meal plan (chapter 9) on one
grid, and the whole thing can be
published as an **iCal feed**
(`/api/calendar/ical`, with a
per-user secret in the address) so
a real calendar app shows "the
milk, the shelves, the filter"
alongside the household's
appointments. The feed is documented
here, not run — the lab's
verification of its *contents* stops
at the lists the feed is built
from, each proven in its own
chapter.
