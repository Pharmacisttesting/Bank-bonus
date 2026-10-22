## 2026-10-22
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
