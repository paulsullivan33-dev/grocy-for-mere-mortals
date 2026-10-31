# The Neighbours

Grocy is one answer to "what do we
have, and what do we need?" These
are the others, characterised fairly
— several are better answers for
particular households.

## Paper, and the fridge-door list

The zero-technology neighbour: a
notebook or a whiteboard. It never
needs booking discipline because it
holds so little — the list is the
point, the stock is in your head.
Paper wins on cost, on speed, and on
never being down. It loses on every
question Grocy exists for: it cannot
warn about dates, add up waste, or
be checked from the shop. Households
whose real problem is only "we
forget to buy things" should stay
here and skip this whole guide.

## The spreadsheet

One step up: a shared spreadsheet of
stock. It is free, everyone can edit
it from the shop, and it bends to
any shape. It is also a ledger
nobody maintains, in practice —
there is no booking, only editing,
so the count and the shelf drift
apart silently, and there are no
dates that warn, no barcodes, no
recipes that cost themselves. A
spreadsheet is where Grocy's author
started for ten years; his verdict
is the program this guide covers.

## Hosted inventory and pantry apps

Phone-first services — the barcode
pantry apps and subscription
inventory tools — sell exactly
Grocy's booking habit with the
server removed: scan in, scan out,
dates tracked, lists shared, for a
monthly fee and your data on their
machines. For a household that will
not run a server, they are the
honest competition, and their
scanning is usually smoother than a
self-hosted page. What you give up
is the API's openness, the data's
address, and the guarantee the
service still exists in ten years.

## HomeBox and the asset trackers

A close cousin worth naming:
**HomeBox** and its kin track *what
you own and where it is* — tools,
appliances, boxes in the attic,
with labels and locations — rather
than *what you consume*. If the
household problem is "which box is
the tent in", that is the shape to
look for; if it is "the milk expires
Thursday", it is Grocy's. (This
series does not have a HomeBox guide
yet; it is named here so the
distinction exists, not linked.)

## The smart-home platforms

**Home Assistant** and its peers are
not stock keepers, but they are
where Grocy often ends up *living*:
through the API, Grocy's due lists
and counts appear on the household
dashboard, and automations ("milk
below one — add to the list the
house already uses") close the loop.
Grocy also has a built-in neighbour
in the data sense: its database is
plain SQLite, so tools like
**NocoDB** or Datasette can be
pointed at a *copy* of `grocy.db` to
browse or chart it — read-only, on a
copy, because the ledger's writer
must stay Grocy itself.

## The difference, in one line

All of these track things. Grocy is
the one that insists on being a
*ledger* — dated movements, prices,
a log that can be undone but not
rewritten — and on running entirely
in your house. Whether that
insistence is a feature is chapter
16's question, and it is a household
question, not a technical one.
