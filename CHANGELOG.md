## 2026-10-31
- Current Direct Deposits moved out of the Dashboard into its own "Current Direct Deposits" tab (after DD Guide in the tab bar); same panel, same data, just no longer crowding the Dashboard.
- Connect button moved to the top bar next to Logout and renamed "Connect Google Drive"; the idle-timeout helper text was removed entirely.
- Removed the duplicate Logout button from the Backup & Google Drive panel — the top-bar Logout is now the only one.
- New Settings tab: color theme and auto-clear after idle live there now; the Backup & Google Drive box is removed from the Dashboard entirely, and the Drive sync status moved up to the top bar next to Connect Google Drive.
- Current Direct Deposits totals fixed: the monthly total now counts the "gets the rest" row (net pay when known), shown as a per-paycheck equation (fixed + rest = total) with weekly/biweekly monthlies labeled as averages; history is sorted newest-first by period end date and shows only the latest 3 with a Show all toggle; unnamed "gets the rest" rows no longer vanish on save, and their amount box is disabled/cleared.
- Drive sync feedback moved off text entirely: the Connect Google Drive button itself changes color — green for saved/synced, amber while saving/connecting/loading, red on save/sync failure — with the full message kept as a hover tooltip.
- DD editor: the biweekly anchor date now uses the site's calendar picker (📅 button, same as bonus-form dates) instead of a bare date field.
- New "Current Direct Deposits" panel on the Dashboard: per-person (You / Spouse) pay schedule — Weekly / Biweekly / Semi-monthly / Monthly with the matching rule (weekday, biweekly anchor payday, two days of the month, day of the month) — computing the next payday plus the following four, with the payroll weekend rule (a Sat/Sun payday lands the Friday before). Each person gets unlimited paycheck split rows (bank, nickname, last 4, $ per paycheck, one "gets the rest" row), a net-pay-per-paycheck field, and live totals (fixed splits per paycheck and per month, remainder to the rest destination, over-split and unallocated warnings). Saving a setup archives the previous one into that person's dated History. Stored in the same Drive file as the bonuses (ddSettings key), so it syncs and survives the session timeout. Prefilled from his payroll setup: You semi-monthly 15th & 30th, Spouse biweekly with the anchor left blank to enter.

## 2026-10-28
- Session timeout is now bank-style: after no activity for your chosen timeout (Off / 20 min / 30 min / 1 hour / 4 hours, default 1 hour), the site logs you out and clears this browser's saved data — whether the tab is still open or you left and came back later, and whether Drive is connected or not. If Drive is connected at expiry it syncs first; otherwise your Drive copy is what comes back when you reconnect. Only your theme and timeout choice survive an expiry. This fixes the old auto-clear, which never fired at all: it referenced a theme key that lives in another script block (threw before logout ran), it only ran while the page stayed open (a closed tab or sleeping laptop had no timer and no record of last activity), and on a fresh visit Drive counted as disconnected so the timer just reset forever. Activity is now stamped to localStorage (throttled) and checked on boot and when the tab becomes visible again.

## 2026-10-22
- Auto-clear after inactivity: new "Auto-clear after idle" setting in the Backup & Google Drive panel (Off / 20 min / 30 min / 1 hour / 4 hours, default 1 hour). After no mouse/keyboard/touch activity for that long, the site signs out and clears this browser's saved data, exactly like the manual Logout button. Safety guard: it only runs when Drive is connected, and it force-syncs bonuses + history to Drive first — if Drive isn't connected, nothing is cleared, so unsynced edits can't be lost.
- Removed the green "banks eligible to rechurn now" callout panel — the rechurn action itself stays available from each bonus's Details footer (and the table/history rows), so nothing is lost, just the banner.
- Merged the two Upcoming panels into one: the bottom Reminders panel is deleted, and planned closes now appear as a third row type in the top Upcoming Repeats & Deadlines panel (deadlines, repeats, planned closes — days 8–90, soonest first, no duplicates with This Week). The per-bonus reminder lead-time fields remain saved on each record but no longer drive a separate panel.
- Removed the "Fix Account Types" button and its modal — his data check showed 0 type/name conflicts across all 96 records (the site's auto-inference had them all right), so the manual review tool was dead weight. The boot-time auto-inference for legacy records stays.
- Person tags everywhere they're needed: This Week's Actions and Reminders items now show whose account it is (You / Spouse tag next to the bank).
- This Week's Actions is split into labeled groups (Deadlines / Safe to close / Repeats ready / Planned closes) in its own container, with a second container beside it: Upcoming Repeats & Deadlines — DD deadlines on open accounts and repeat-eligible dates on closed accounts landing in the next 8–90 days, soonest first, each row clickable into Details. Items already in This Week or the rechurn callout don't repeat there. The two containers stack on narrow screens.
- Tax Export button is now a full-width primary button ("🧾 Tax Export — CSV by Year") on the Received This Year card — it was a small ghost button that looked hidden.

# Changelog

All code changes to the Bank Bonus tracker site. Data lives in Google Drive
(`bank-bonus-tracker.json`); this file tracks code only.

## 2026-10-21
- DD Progress Log: log each direct deposit that actually posted (date / amount / method) from the Details view; progress ("Posted 1 of 2 · $1,500 of $3,000") with a progress bar in the main table and Details, a green "DD met" tag when the count and total are both satisfied (a DD counts when it meets the required per-DD amount). Entries are stored in the bonus record, survive form edits, and every add/remove is recorded in the Change Log.
- Tax Export: new button on the "Received This Year" card opens a per-year export — Bank / Player / Date Posted / Amount, subtotal per player, year total, and a CSV download with the same four columns. A bonus is taxed in the year it posted.
- Repeat dates: a repeat-eligible date in the past is no longer shown on an account that has already been (re)opened (main table, Details card, and History rows where a newer account is active) — it was computed from the previous churn and is noise once you're in the new one. Future dates and closed accounts still show it.
- History diffs no longer show phantom "— → —" lines for fields that were missing (vs blank) in older records.
- XSS hardening (the deferred fix): all user-data text (bank, account
  name/type, requirements, notes-derived labels, person/status fallbacks)
  is now HTML-escaped at every rendering site — main table, Closed
  section, History tab, Keepers, rechurn list, week widget, reminders,
  Change Log, CSV import preview, and the Fix Account Types modal.
  Data-* id attributes are escaped too. Source URLs now pass through a
  scheme check (`safeUrl`): only http(s) links render as links, anything
  else (e.g. a `javascript:` URL in imported data) becomes `#`.
  No visual changes for normal data.
- Rebuilt View-only mode as a dedicated Details modal: summary cards
  (Opened / DD Deadline / Bonus Posted / Safe Close / Repeat Eligible),
  grouped Account / Requirements & Fees / Terms & Repeat sections, and
  empty fields hidden instead of shown blank. Replaces the locked
  edit-form view.
- Added per-bank History: a "🕐 History (n)" button next to View/Edit on
  the main table, Closed section, History tab, Keepers, and the rechurn
  list opens a timeline for that bank only, with scope toggles
  (This bonus / All bank — this person / All bank). Update events show
  real old → new value diffs (the Change Log tab only listed field
  names). Bonuses with no Change Log events get synthesized lifecycle
  lines (Opened / Bonus posted / Closed + repeat-eligible date), so
  bonuses from before the Change Log existed (2026-10-18) are not
  blank. Bank names in the Change Log tab now link into the same
  per-bank history, and the modal links back to the Change Log
  filtered to that bank.

## 2026-10-18
- Added Change Log: every bonus create, update, delete, and bulk change is now
  recorded like a history table — timestamp, action, before/after snapshot —
  and saved to `bank-bonus-tracker-history.json` in the same Drive folder
  (created automatically on the first logged change). New "Change Log" tab
  shows events newest-first with search.
- Fixed Closed section visibility: bonuses with a closed-type status
  (`closed`, `closed_repeat`, `cancelled`) but no close date were invisible in
  every view — hidden from the main table by status, excluded from the Closed
  section by the missing date. They now appear in the Closed section, sorted
  last, with "—" for the date.

## 2026-10-17
- Added read-only View mode: 👁 View button on all bonus lists; locks form
  fields and hides Save while viewing.

## 2026-10-16
- Added DD Guide tab: per-bank direct-deposit knowledge base with form
  autocomplete.
- Restored Add Bonus tooltip CSS.

## 2026-10-16
- Removed duplicated calendar picker, dead contact-form handler, and
  duplicate date-picker CSS.
