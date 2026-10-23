# Clarity v0.5.1

Personal cognitive and wellbeing tracker. Static, offline-capable web application; no build step or server required.

CLARITY — personal cognitive and wellbeing tracker

INSTALL ON IPHONE
1. Host the contents of this folder together on an HTTPS website (e.g. a private static host).
2. Open the HTTPS URL in Safari.
3. Share > Add to Home Screen > Add.
4. Launch from Home Screen. Use the same iPhone and browser profile consistently.

This ZIP is source code, not an already hosted website or signed native iPhone app.
Opening index.html directly from Files may prevent storage/service-worker features from working reliably.

PRIVACY AND BACKUPS
Data is kept in browser localStorage on the device, without app-side server transmission.
This is not encrypted-at-rest application storage, and not password-protected.
Anyone with access to the unlocked browser profile may see it.
Browser data deletion can erase entries. Export full JSON backups regularly and store them securely.

LIMITATIONS
Experimental, non-validated cognitive tasks; not diagnostic. Touchscreen latency and test-form difficulty limit precision. Repeated exposure can produce practice effects. Follow medical advice for all medication changes.

Version 0.2 adds daily menstrual tracking and a dashboard. Replacing hosted files preserves data only if the site origin remains unchanged; export a JSON backup first.


## Hosting
Publish the repository root using GitHub Pages (Settings > Pages > Deploy from a branch > main > / (root)). Open https://aquestiaux.github.io/Clarity/ in Safari and add to Home Screen.

## Structure
- index.html: interface and styles
- app.js: journal, cognitive tasks, dashboards, local import/export
- sw.js: offline asset cache
- manifest.webmanifest and icon.svg: installation metadata

## Maintenance
Edit this repository directly. Preserve the clarity_v1 localStorage key and the journal/tests record structure. Any future schema changes require backward-compatible migration and synthetic-data verification. Keep the production origin stable. Increment the service worker asset cache version when changing cached application files. Never commit personal journal entries, health records, test results, or exported backups.

## Calendar and cycle tracking (v0.4)
Choose any calendar day (today by default), then add separate check-ins or edit one saved check-in. Old entries remain available. Each check-in has a date, local time and creation timestamp. Future plans are excluded from summaries until their date.

Additive schema: `logs[date]` stores `{id, time, createdAt?, updatedAt?, values}` check-ins. Legacy `journal[date]` records are kept and imported as the first check-in only when that day is first saved. `journal` remains a daily summary: mood, fog and anxiety average available check-ins; sleep quality uses the latest non-missing nightly rating; sleep and other numeric measures use the latest non-missing value by check-in time. Existing `tests` and unknown record fields are retained. No records are changed merely by opening the app. New check-ins leave scores blank until supplied.

Period-start details appear only when Yes is selected. General symptoms remain available every day. Cycle estimates use the mean of the most recent six recorded intervals once three exist; otherwise they use a stated usual length or an explicitly provisional 28-day assumption. Predictions use actual recorded starts only and never roll an overdue cycle forward automatically. Phase estimates are hidden for irregular timing or when disabled. Dates cannot measure hormone levels or confirm ovulation; estimates are not contraception. General timing information: https://www.nhs.uk/conditions/periods/fertility-in-the-menstrual-cycle/ . Headache comparisons use logged calendar days in completed cycle halves, not measured hormonal phases.

JSON exports include all check-ins. CSV exports include daily summaries and individual check-ins separately. Run `node tests/calendar-regression.cjs` for synthetic-data regression coverage.

## Sleep (v0.4.1)
Sleep is recorded against the waking date, including lights-out and wake-up clock times and a quality rating. The displayed interval handles midnight crossing but is not treated as actual sleep. Actual hours asleep remain optional and feed sleep-duration charts. Legacy `lights`, `quality` and `hours` fields are unchanged; `wake` is additive. Latest non-empty sleep clock times and latest non-missing quality are retained in daily summaries. Sertraline is removed from the interface and charts; any historic values remain in local records and exports.

## Sliders and irritability (v0.4.2)
The actual-hours-asleep input and duration charts are removed; legacy `hours` values remain in records and exports. The sleep chart now uses sleep quality. Mood, fog, quality, anxiety, irritability and pre-test sleepiness use labelled 0–10 sliders with live scores. Unanswered journal ratings save null, not the physical slider midpoint. Clear removes an answer; pre-test sleepiness resets to its existing default of 3. Irritability is an additive numeric field, averaged across check-ins for daily summaries and available in Trends and dashboard stats. Previous rating values and unknown legacy fields remain supported.


## Interface and comparisons (v0.5.1)
The mood-first interface keeps full check-ins in expandable groups, with a compact optional calendar, mood-coloured entry cards, bottom navigation and a local Soft paper/Calm dark preference. Context shortcuts add or remove whole lines from the existing events field while preserving other text. All cognitive tests, cycle estimates, journal fields, CSV exports and JSON restore controls remain available.

New slider choices are integers from 0 to 10. Historical fractional ratings are preserved on load and when editing another field; moving that slider replaces its score with a whole number. The storage key remains `clarity_v1`.

Trends supports multiple selected measures, defaulting to mood plus sleep quality. Compatible 0–10 scores share their actual scale. Different units automatically use relative 0–10 scaling based on each series' observed min/max; constant series appear at 5. The legend gives original units and ranges. Tap a date to read actual values. Date spacing is proportional to calendar time; missing values remain missing. Multiple study sessions on one date are averaged; practice sessions and future dates are excluded.

Run `npm test` for the dependency-free data regression checks. For browser checks, install development dependencies and Chromium (`npm install`, then `npx playwright install chromium`), and run `npm run test:browser`. An existing browser binary can be selected with `CHROMIUM_EXECUTABLE_PATH`. Tests use synthetic fixtures, a local server and temporary screenshots only.
