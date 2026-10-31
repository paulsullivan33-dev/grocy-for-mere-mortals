# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's pantry is invented and its brands are no one's.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 16 names what the tool
   cannot do: it knows only what it is told, one household per
   installation, no business machinery, a household-sized
   database, dates no smarter than the typing, a platform
   floor that keeps moving, and a one-maintainer project.
8. **The ethics rule is in the text, early.** Grocy raises no
   interception question, but it holds a household's private
   facts — what a family eats, spends and wastes. Chapter 4
   states the public default password and the duty to change
   it before Grocy holds anything real; chapter 11 states
   that the data directory holds secrets (API keys in plain
   text) and chapter 13 gives backups the same care as a
   password vault.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, ~8 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran (timings are therefore
ranges, and the guide says so). Grocy **4.7.1** (release date
2026-09-04) was downloaded as the official release zip
`grocy_4.7.1.zip` from the project's GitHub releases
(43,599,890 bytes). The project publishes no checksum for the
zip; the lab computed SHA-256
`c4a10e96ed76664baf465e9b27dc7a31aa08c0e3ad876afdfae4f1ff9ab2fc19`
for the artefact it ran, and chapter 3 prints it. All
transcripts live in `grocy-lab/transcripts/`.

**How PHP was obtained — disclosed in full.** Grocy 4.7.1
refuses to run below PHP 8.5: the release's own
`helpers/PrerequisiteChecker.php` states
`REQUIRED_PHP_VERSION = '8.5.0'` and required extensions
`fileinfo, pdo_sqlite, gd, ctype, intl, zlib, mbstring`
(plus core `filter, iconv, tokenizer, json`), and SQLite
3.40+. The sandbox image's system PHP was 8.3.6 — too old.
The lab therefore twice used the distribution-package
extraction route: `apt-get download` of the php8.3 packages
and `dpkg-deb -x` into a private tree (t01, t02), then —
having read the 8.5 requirement in the shipped checker — the
same route with PHP **8.5.12** packages for Ubuntu 24.04 from
the ondrej/php repository (t03, t04). Nothing was installed
system-wide either time; the extracted PHP ran with `PHPRC`
pointed at a lab `php.ini` naming the extracted extension
directory and an empty ini scan directory, so the module list
in t04 is exactly what Grocy's checker judged. SQLite was
3.45.1. This route is a testing rig, not an installation
recommendation; chapter 3 gives the real routes (Grocy
Desktop, Docker, a maintained PHP) and says so.

Grocy was served by PHP's built-in development server on
loopback port 18106, document root the release's `public`
folder, with a small router script performing the
serve-the-file-or-fall-through-to-`index.php` work that
Grocy's README prescribes for nginx (`try_files`). The data
directory was redirected out of the program tree with the
`GROCY_DATAPATH` environment variable the program itself
honours; `data/config.php` is the shipped `config-dist.php`
copied per the install instructions, with defaults.

Run for real, with transcripts as evidence:

- Download, size and computed SHA-256 of the release zip;
  `version.json` read from the unpacked artefact (t00).
- First run: `GET /` redirects to `/login`; `GET /login`
  returns 200 with title "Login | Grocy"; the first request
  created `data/grocy.db` and migrated it (system info
  reports db_version 257). First render took about a minute
  on the shared box; later pages a second or two (t05).
- Login with the shipped default `admin` / `admin` (the form
  sends `password_base64`), session cookie issued; the
  cookie alone authenticated `/api/system/info`, which
  answered Grocy 4.7.1, PHP 8.5.12, SQLite 3.45.1 (t05, t06).
- **API key created through the application's own web flow**:
  logged in, then `GET /manageapikeys/new?description=...`,
  the same request the Manage API keys page's button makes;
  the app created key id 2 and displayed it. No database
  insert was used at any point. The key value is redacted in
  the transcripts and died with the lab. The lab read the
  `api_keys` table with the sqlite3 CLI afterwards and
  confirmed keys are stored unhashed — chapter 11's warning
  rests on that read (t06).
- Admin password changed via `PUT /api/users/1`; afterwards
  the old default redirects to `/login?invalid=true` and the
  new password to the stock page (t06). A first, partial
  PUT (password field alone) returned HTTP 500 because
  EditUser requires the username — the verbose server error
  was not copied into the transcripts (it echoed a fragment
  of the fixture password); the behaviour is described in
  chapter 17 without it.
- Master data and stock flow (t07–t09): location created
  (`Pantry shelf`, id 3); first product attempt failed
  verbatim with "table products has no column named
  quantity_unit_id_purchase", and the guide's fields were
  then taken from the release's own `grocy.openapi.json`
  (`qu_id_purchase`, `qu_id_stock`); product `Rolled oats`
  created (id 1); invented barcode 4008400401232 (valid
  EAN-13 check digit) attached and resolved through
  `/stock/products/by-barcode/...`; purchase of 6 at 1.85,
  best before 2027-06-30 → stock amount 6, stock value
  11.10; consume 1 → amount 5, value 9.25; `GET /stock`
  overview agreed line for line.
- Shopping list (t09, t10): `add-product` with the field
  `amount` silently defaulted to 1; with the documented
  `product_amount` the row accumulated to 2; a re-add
  without a note cleared the note. All three behaviours
  read back from `/api/objects/shopping_list`.
- Expiry (t11): milk booked with best before 2026-11-03
  (three days after the lab date) appears in
  `due_products`; a yoghurt with 2026-10-20 appears in
  `overdue_products`; `expired_products` and
  `missing_products` empty — the raw volatile response is
  in the transcript.
- Recipes (t11): recipe created, ingredient row added,
  `GET /api/recipes/1/fulfillment` answered
  `need_fulfilled: 1`, `missing_products_count: 0`,
  `costs: 1.85`.
- Chores, batteries, users (t12): monthly chore created
  and executed (`chores_log` row written); battery
  registered; second user created and logged in through
  the real login form.
- API surface (t13): the shipped `grocy.openapi.json`
  counted — OpenAPI 3.1.0, 74 paths, 87 operations;
  `GET /api` returned 200, title "REST API browser";
  a wrong key returned 401. The whole data directory was
  728 KB, the database 602,112 bytes.
- Shutdown: the server was stopped and port 18106
  verified free afterwards; the lab directory (program,
  PHP trees, data, keys) was deleted. Only the redacted
  transcripts remain, in this repository.

Documented only — described from the project's
documentation, release files or README, not run, and the
text does not claim otherwise:

- Click-by-click use of the web pages beyond loading them,
  the login, and the API-keys flow above: the pages and the
  API are the same machinery, and the chapters say the API
  carried the proof.
- Barcode lookup against external services (Open Food
  Facts plugin and friends): an internet service, out of
  scope for the lab; the local barcode store-and-resolve
  half is live.
- Meal planning on the calendar, the iCal feed, and the
  one-call shopping-list adds (missing/overdue/expired):
  described from the docs and the shipped flag/endpoint
  names; the lists they consume were verified live.
- Charge cycles for batteries, the equipment module's
  use, tasks, recipe consumption and shopping-list
  generation from recipes: shapes read from the release;
  registration-level live evidence only, as chapter 10
  states module by module.
- Camera barcode scanning, companion/scanner apps, Grocy
  Desktop, the Docker image, thermal label printing,
  reverse-proxy and LDAP authentication: named and
  characterised, never run.
- Updates performed with `update.sh`: the procedure is
  quoted from the project's README; the lab installed
  one version and updated nothing.

Fixtures are this guide's own and deliberately generic: an
invented pantry (rolled oats, milk, a yoghurt pot, all
"invented lab brand"), an invented EAN-13 barcode computed
to a valid check digit, and a second user named as an
example. No real household, shop, organisation or person is
modelled anywhere in the guide or the lab.
