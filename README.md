# Black Star Studio

A free-to-start editorial workspace for Universal Black Star. Original application code, no paid services, no scraping of LinkedIn and no subscription bypasses.

## Included

- Responsive dashboard with real workspace counts, draft list and research links.
- Month calendar with browser-local time display. Planned dates are reminders, not automatic publishing jobs.
- Research desk: FMCSA, Federal Register, eCFR, EIA, BTS, PHMSA and an official state DOT directory; research briefs for all contiguous 48 states.
- Live Federal Register document-title retrieval through Apps Script, with a one-hour cache. Other sources are curated reading links, not live scraped news feeds.
- Text editor, preview, source URLs, evidence notes, source review date, rate-context fields, approval and no-emoji checks.
- Local persistence, JSON backup/import, Google Sheets shared storage, revision conflicts and an audit log.
- LinkedIn organization OAuth and text-only Posts API publishing, gated by product eligibility and explicit per-post confirmation. Manual copy, open LinkedIn and publication recording work without API approval.
- Duplicate-attempt protection: an uncertain publication locks the post until a person checks LinkedIn and reconciles it.

Start with [the setup guide](docs/SETUP.md). See [architecture and security](docs/ARCHITECTURE.md) and [verification](docs/VERIFICATION.md).

## Run locally

No install or build step is needed. Serve this folder with any static HTTP server, such as `python3 -m http.server 8765`, then open `http://localhost:8765`. Modern browsers are required. No credentials are shipped. Local drafts remain on this device until exported or explicitly saved to a connected backend.

Tests: `node tests/backend.test.cjs` from this folder. Tests use simulated Apps Script services and never publish anything.

## Deployment status

Source prepared on the `black-star-studio` feature branch in the existing private Universal-Black-Star repository. No production deployment, Google authorization or live LinkedIn publication has been completed. The repository's existing project brief asks that it remain private. Free GitHub Pages requires a public source repository; use a separately approved, source-only public repository rather than exposing existing private history. The provided workflow is manual and uploads only the five frontend assets.

## Editorial limits

No invented national spot rates, guaranteed rates, copied paid data or generated current legal claims. Seed posts are unapproved editorial starting points. A checked box is a human review attestation, not automatic fact verification. State briefs do not contain a verified 48-state legal database. Research and rate data need human review before publication. The app reports only locally recorded publications, not LinkedIn reach or engagement analytics.

The application has no automatic posting trigger, media upload or paid AI model dependency. Apps Script and other providers impose quotas; no unlimited availability or API approval is promised.
