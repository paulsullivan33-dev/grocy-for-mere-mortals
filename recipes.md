# Recipes

Worked sequences that combine the
chapters. Each assumes the ones
before it only where it says so.

## 1. The first cupboard, in one sitting

*Chapters 4, 5 and 6.*

1. Log in and change the `admin`
   password.
2. Create the locations your kitchen
   actually has — cupboard, fridge,
   freezer — not the ten a magazine
   would suggest.
3. Pick **one shelf** and only that
   shelf. Create each product on it
   (name, location, Piece for both
   units to start), with its barcode
   if it has one.
4. Book the shelf's contents in as
   purchases, with the real
   best-before dates off the packets
   and the prices from the last
   receipt if you have it, or 0 if
   you don't — a true date matters
   more than a true price.
5. Live with one true shelf for a
   week. Book out of it as you use
   things. A household that can keep
   one shelf honest can keep ten;
   the reverse is not true.

## 2. Expiry patrol

*Chapters 6 and 7. Ten minutes,
weekly.*

1. Open the stock overview, or pull
   `GET /api/stock/volatile`.
2. Cook from the **due** list this
   week; move what you can't use to
   the freezer and transfer its stock
   (chapter 6) so the date story
   stays true.
3. Book out the **overdue** items
   you throw away with
   `"spoiled": true`. Read the waste
   in money once a month; it is the
   most persuasive number Grocy
   produces.
4. Anything on the overdue list you
   ate anyway, book out as consumed —
   the ledger should say what
   happened, not what should have.

## 3. Let the cupboard write the list

*Chapters 5, 7 and 8.*

1. Set an honest **minimum stock
   amount** on the staples — the
   things a household must never run
   out of (for most: milk, eggs,
   coffee, the pet's food).
2. Before a shop, run
   `POST /api/stock/shoppinglist/add-missing-products`
   or the same button on the list
   page.
3. Add the week's fresh things by
   hand, shop with the list on a
   phone, and tick items as they go
   in the trolley.
4. At home, book the ticked items
   into stock with their real dates.
   The loop — missing, listed,
   bought, booked — is the whole
   system in one motion.

## 4. A second person joins

*Chapters 11 and 12.*

1. Create the user (`POST
   /api/users`, or the user pages).
   Give them their own password to
   change, not the household's
   shared one.
2. Decide their permissions
   deliberately instead of accepting
   the `['ADMIN']` default if your
   Grocy is reachable beyond the
   household.
3. Have them make **their own API
   key** for their scanner app or
   phone, named after its job.
4. Show them the two bookings that
   matter — purchase and consume —
   on one real item. The rest of
   Grocy can be learned by appetite;
   those two cannot be skipped.

## 5. Expiry warnings in your calendar

*Chapters 7 and 10. Documented
route; the lab verified the lists
the feed is built from, not the
feed itself.*

1. In your user settings, find the
   calendar (iCal) feed address —
   it carries a personal secret;
   treat the URL like a password.
2. Subscribe your calendar app to
   it as a read-only calendar.
3. Best-before dates, chores and
   meal plans now sit beside the
   household's appointments, where
   "the milk goes off Thursday" is
   seen by the people who plan
   Thursday.
