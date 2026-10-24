# Deployment and account authorization

## 1. Review the source and choose a public source-only destination

The existing repository is private and its PROJECT_BRIEF.md says to keep it private and request review before merging/deployment. The implementation lives on a feature branch for review. Do not make the existing repository public just to enable hosting.

For the requested no-paid-services deployment, create or authorize a separate public GitHub repository containing only these application sources. GitHub Free provides Pages for public repositories. Do not transfer private repository history, drafts, Google Sheet exports, tokens or workspace keys. No custom domain is needed.

In the public source repository, open Settings > Pages and choose GitHub Actions. Run the included “Deploy Black Star Studio” workflow using Actions > Run workflow. It stages only index.html, style.css, app.js, core.js and seed.js. Follow the actual deployment URL returned by the workflow; no live URL is claimed in this handoff.

Review [GitHub Pages usage limits](https://docs.github.com/en/pages/getting-started-with-github-pages/github-pages-limits), including its restrictions on online business/commercial SaaS hosting and sensitive transactions. This app is a single-workspace editorial tool, not a commercial multi-tenant SaaS. That description is not a guarantee of GitHub approval; if its hosting terms do not permit your intended usage, stop before deployment and choose another approved free static host. The frontend does not depend on Pages-specific APIs.

## 2. Create the Google backend

1. Sign in to the Google account that should own the data and open [Apps Script](https://script.google.com/home/start).
2. Create a project named Black Star Studio. Replace Code.gs with backend/Code.gs and add a second script file named Core containing backend/Core.gs.
3. In Project Settings enable “Show appsscript.json manifest file in editor.” Replace its contents with backend/appsscript.json. The two scopes allow Sheets access and external HTTP requests. Review those permissions before granting access.
4. Run `setup` from the editor and authorize Google. It creates a new private Sheet with Posts and Audit tabs. Existing sheets are not replaced. Project Settings > Script Properties now contains SPREADSHEET_ID and WORKSPACE_KEY. Open the generated Sheet from your Drive to verify it exists.
5. Set PAGE_ORIGIN to the exact origin of the actual hosted frontend, for example `https://your-account.github.io` (no path or trailing slash). For localhost testing use the exact localhost origin and port instead; change it back for production.
6. Deploy > New deployment > Web app. Execute as **Me**; access **Anyone**. This makes the endpoint publicly reachable, while private operations require the generated workspace key. Granting deployment/Google permissions is an owner authorization step. Workspace policies may disallow anonymous web apps; if so, this bridge cannot be deployed under that policy and must not be represented as connected.
7. Copy the deployed `/exec` URL. In the hosted frontend open Connections & backup, export any local drafts, enter the URL and workspace key, then Connect & load shared posts. The key is sent in the HTTPS POST body and held only in tab memory. The URL alone grants no private data access.
8. Create a test draft, save, reconnect and verify it appears both in Studio and in the Sheet. The first shared workspace starts empty. Seed drafts are local examples and are not automatically uploaded.

The repository tools available during implementation can edit files but do not deploy Apps Script projects or grant your Google account consent. These Google steps remain owner actions. Use a normal supported browser if an embedded browser blocks Google authentication or framed responses.

When changing backend source, update the existing deployment to a **new version** through Manage deployments. Keep the same deployment URL so the LinkedIn callback remains valid.

## 3. Use manual publishing immediately

1. Open a draft. Check the original sources and record the review date and evidence notes.
2. Confirm the editorial checklist and approve it. Optional planned date/time is a calendar reminder only.
3. Copy the post, open LinkedIn, select Universal Black Star as the Page author, paste and review, then publish yourself.
4. Copy the live post URL and use Record manual publication. This records your attestation; it does not independently verify the post through LinkedIn.

No Page URL or organization ID has been invented. The Open LinkedIn button opens the company area; navigate to the actual Universal Black Star Page.

## 4. Enable the approved LinkedIn integration, if eligible

For organization posting, you need an application with approved Community Management product access, the `w_organization_social` scope, and an authorized member with an eligible role on the target organization. Personal `w_member_social` access does not substitute for Page access. LinkedIn operates Development and Standard tiers with different access limits/review requirements; approval is not automatic, and a single-business internal tool may not qualify for the use cases LinkedIn accepts. Confirm eligibility in the developer portal; use manual publishing if unavailable.

1. Create/select an application in the [LinkedIn developer portal](https://www.linkedin.com/developers/apps). Associate and verify the correct company Page and complete LinkedIn's required application review. The app may need public privacy and product information; complete truthful owner contact and policy details before submitting. No application has been submitted here.
2. After approval, confirm the scope under Auth. Register the deployed Apps Script `/exec` URL as the exact HTTPS redirect URL. No frontend URL, URL fragment or query parameters are needed for the callback.
3. Add the following **only to Apps Script Script Properties**:

| Property | Value |
| --- | --- |
| LINKEDIN_CLIENT_ID | Developer application's ID |
| LINKEDIN_CLIENT_SECRET | Private developer application secret |
| LINKEDIN_REDIRECT_URI | Exact registered Apps Script `/exec` URL |
| LINKEDIN_ORGANIZATION_ID | Actual numeric Universal Black Star organization ID |
| LINKEDIN_VERSION | A currently supported YYYYMM Posts API version from LinkedIn's official docs |
| LINKEDIN_APPROVED | `true` only after the product and scope are granted |

4. In Studio connect the Google backend, then select Authorize LinkedIn and follow the generated authorization link. An eligible Page admin grants consent on linkedin.com. The callback exchanges the authorization code server-side and stores the token and expiration in Script Properties. Return to Studio and Check connection.
5. Approve a real post, verify the configured organization ID, and use Publish approved post. The final dialog shows the saved text. Publishing is immediate even if the calendar date is later. LinkedIn's API is the final authority on Page permissions and supported API versions.

Do not put the secret, token or workspace key in GitHub, a deployment variable exposed to browser code, Google Sheet cells, URLs, browser localStorage or chat. No refresh token is requested; authorize again when the token expires. Remove the stored token through Studio and revoke the application in LinkedIn settings when ending access. Restrict Apps Script project editor access: editors can read Script Properties.

If a request times out, reconnect to reload the server state and inspect the Page. Record its actual URL if it published. If it did not publish, wait at least two minutes and use “Confirm attempt did not publish” on the locked post. This human reconciliation is necessary because LinkedIn does not provide an app-level idempotency guarantee here.

## Official references

- [LinkedIn Community Management access and tiers](https://learn.microsoft.com/en-us/linkedin/marketing/community-management/community-management-overview)
- [Posts API permissions and version headers](https://learn.microsoft.com/en-us/linkedin/marketing/community-management/shares/posts-api)
- [LinkedIn authorization code flow](https://learn.microsoft.com/en-us/linkedin/shared/authentication/authorization-code-flow)
- [Google Apps Script web app permissions](https://developers.google.com/apps-script/guides/web)
- [Google Apps Script quotas](https://developers.google.com/apps-script/guides/services/quotas)

Free-to-start means no required paid subscription. Providers may change quotas, eligibility and terms; this project does not enable billing or buy any service.
