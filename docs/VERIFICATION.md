# Verification — October 24, 2026

## Passed

- JavaScript syntax checks for frontend, seed data and shared validation.
- Mocked Apps Script tests: editorial validation; emoji, length, source and review checks; workspace authorization and origin rejection; response HTML escaping; revision conflicts; forged publication status rejection; approval gates; duplicate prevention; uncertain network outcomes; reconciliation wait; OAuth state rejection/replay protection; token removal.
- Local browser smoke test: dashboard, opening a seed draft, local review/approval/save, calendar navigation/post list, research search filtering to EIA for “diesel,” and the selector for all 48 contiguous states.
- Responsive layout inspected at mobile and desktop sizes. Desktop document measurements showed no horizontal overflow. The mobile calendar intentionally scrolls horizontally.
- No application console errors during the smoke test.
- FMCSA, eCFR cargo securement, EIA diesel, PHMSA and FHWA directory reference pages opened through web research. This does not verify claims in future posts.

## Not yet verified live

- Pages deployment: available repository is private and its existing brief asks to keep it private. A separate public source-only destination and deployment review remain necessary for the no-paid-services plan.
- Apps Script deployment, Google consent, Sheet creation and production form/iframe transport. These need owner authorization and a deployed `/exec` URL. Browser/provider policies can affect the iframe response.
- Federal Register live API retrieval: the API URL could not be read by the web research tool. Failure handling is implemented; validate retrieval from deployed Apps Script.
- LinkedIn eligibility, granted scopes, actual organization ID, admin consent, callback, current API version and live publication. No social post was sent.
- Load, full accessibility and provider quota testing.

## Owner acceptance checks

1. Verify the actual static deployment URL.
2. Authorize/setup Apps Script; inspect the private Sheet; configure PAGE_ORIGIN; deploy and connect Studio.
3. Save/reload a test draft and confirm the Sheet row. Use two tabs to verify stale edits fail.
4. Refresh federal notices and check a full document's title, date and link.
5. Export/import a local backup; confirm it contains no secrets.
6. Test manual publishing to the real Page and record the actual published URL.
7. After LinkedIn approval and consent, test OAuth and one explicitly approved API post. Inspect the Page and verify a repeated request cannot duplicate it.
