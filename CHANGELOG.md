# Changelog

All code changes to the Bank Bonus tracker site. Data lives in Google Drive
(`bank-bonus-tracker.json`); this file tracks code only.

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
