# Architecture and security

## Data path

GitHub Pages serves five static frontend files. Disconnected mode uses browser localStorage for draft records and research titles. Connected mode sends an HTML form POST to an Apps Script `/exec` deployment in a hidden response iframe. This deliberately avoids relying on unsupported arbitrary CORS headers in Apps Script ContentService. HtmlService returns a sandboxed page which sends a JSON response to the configured Pages origin using `window.top.postMessage`.

Each request has a cryptographically random ID. The browser accepts matching pending responses only from Google script response origins. The backend requires an exact configured PAGE_ORIGIN and a high-entropy workspace key in the POST body. Origin checking is defense in depth, not authentication: the workspace key authorizes access. The integration must be verified against a real deployment and browser; mocked tests cannot prove Google's live framing behavior or account policy.

This is a shared single-workspace owner/admin key, not multi-user identity or role-based access. All key holders can read drafts, approve, delete drafts, start OAuth and publish approved posts. Rotate WORKSPACE_KEY in Script Properties when a person loses access. The browser discards it on reload/disconnection; the backend URL alone is stored locally. Scripts on the same hosting origin can potentially access local data, so use a trusted dedicated origin and do not host untrusted JavaScript alongside the app.

Google Sheets Posts rows contain ID, JSON record, revision, last update and status. JSON serialization avoids spreadsheet formula interpretation of post content. A script lock protects state changes. Revision checks reject stale edits; timestamps use UTC ISO strings and calendar display uses the browser timezone. Audit records contain timestamps, action names and post IDs, not OAuth secrets, tokens or full post text.

OAuth state is random, hashed for property indexing, valid for ten minutes and consumed once under a lock. Only server code exchanges authorization codes. Secrets and tokens stay in Script Properties, accessible to trusted script project editors. The callback does not return tokens to the browser. API error responses omit provider response bodies, which can contain sensitive data.

## Publishing state

`draft → approved/planned → publishing → published`

A failed ambiguous request becomes `publish_unknown`. A terminated execution may leave `publishing`. Neither can be republished directly. A reviewer must inspect LinkedIn and record the published URL, or explicitly attest that it did not publish after a two-minute wait. A definite non-201 client HTTP error returns the post to approved; always reload before retrying. Published records are immutable in the app. To revise, create a new draft.

The backend revalidates sources, review date, review attestation, post length, no emojis and rate-context fields before approval or publication. The browser mirrors these checks for immediate feedback. Edits in the browser clear the review checkbox. Validation is not a factual accuracy engine: a human must verify that sources support each claim.

## Research

The Apps Script research action calls one fixed Federal Register API URL, caches at most twelve title/link/date/type records for an hour and never fetches a user-provided URL. This prevents the research endpoint from becoming an arbitrary URL proxy. External reference URLs open in new tabs with opener access disabled. No paid content or LinkedIn pages are scraped.

Source summaries are editorial guidance; regulations and figures require current primary-source review. The state selector creates a checklist, not a verified legal answer. Brokerage/carrier market updates use public FMCSA/BTS sources; private company news must be sourced by the editor. There is no free live national lane-rate database bundled here.

## Operational limits

Browser drafts are not encrypted and persist until browser storage is cleared or a local backup is imported/replaced. Shared drafts are private under normal Drive sharing but accessible to workspace key holders. The key is not written to logs by this code. Do not enable request-body logging or record a browser session containing the key.

Apps Script quotas, Sheet size, script property limits and provider rate limits apply. The public endpoint can consume script capacity even with invalid keys; this is suited to a small trusted team, not a public multi-tenant service. Hosting and Google/LinkedIn consent remain owner-controlled. No automatic scheduler, retries of uncertain publications, metrics API, AI generator or media uploader is included.
