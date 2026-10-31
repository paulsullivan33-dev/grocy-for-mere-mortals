# Glossary

**API key** — A long random token
that lets a program use Grocy's API
as a particular user. Created in the
web pages, sent in the
`GROCY-API-KEY` header, stored in
the database as plain text.

**Barcode** — The number under the
stripes on a package. Grocy stores
barcodes per product so scanning can
find the product instantly.

**Best-before date** — The date on
one stock entry after which quality
is no longer promised. Grocy keeps
due, overdue and expired lists from
these dates.

**Booking** — One dated movement in
the stock ledger: a purchase, a
consumption, a correction or a
transfer. Stock amounts are bookings
added up.

**Consume** — To book stock out as
used. The `spoiled` flag separates
"eaten" from "thrown away".

**Data directory** — The one folder
holding everything that is yours:
`config.php`, the database, uploads.
Named by `GROCY_DATAPATH`.

**Due / overdue / missing** — Stock
states from chapter 7: a date inside
the warning window, a date passed,
or an amount below the product's
minimum.

**ERP** — Enterprise resource
planning; business stock-and-process
software. Grocy is the idea, shrunk
to a household.

**Fulfilment** — A recipe's answer
to "can the cupboard make this now,
and what is missing?"

**Grocycode** — Grocy's own
printable codes for products, stock
entries and chores, so a scan can
name an exact item or batch.

**Ledger** — The list of movements
itself (`stock_log`). Amounts are
derived from it; it is corrected by
new entries, never rewritten.

**Location** — A place stock lives:
Fridge, Pantry shelf, Freezer. Every
product has a home location; every
stock entry is somewhere.

**Master data** — The standing
records — products, locations,
quantity units — as opposed to the
movements booked against them.

**Open Food Facts** — The open,
crowd-sourced food database Grocy's
default barcode-lookup plugin asks
about unknown barcodes.

**Product** — A kind of thing
("rolled oats"), as opposed to the
stock entries that are the actual
bags.

**Quantity unit (QU)** — The counter
a product is measured in — piece,
pack, gram. Products can have
different units for buying and for
stock, with conversions between.

**Stock entry** — One arrival of a
product: an amount with its own
date, price and location.

**Volatile stock** — The API's name
(`/api/stock/volatile`) for the
due/overdue/expired/missing
overview.
