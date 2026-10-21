# Changelog

All code changes to the Bank Bonus tracker site. Data lives in Google Drive
(`bank-bonus-tracker.json`); this file tracks code only.

## 2026-10-21
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
