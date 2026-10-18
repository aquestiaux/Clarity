# Clarity v0.2

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
