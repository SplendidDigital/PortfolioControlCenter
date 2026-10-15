# Digital Splendid Portfolio Control Center

**A private WordPress workspace for preparing websites and other digital assets for sale.** Track sale-preparation evidence, maintain an internal due-diligence checklist, collect selected information from compatible WordPress plugins, and share a carefully selected buyer evidence report.

- **Current plugin version:** `0.18.1`
- **Requires WordPress:** `6.0` or newer
- **Requires PHP:** `7.4` or newer
- **License:** GPL-2.0-or-later
- **Author:** Digital Splendid

> **Important:** The sale-readiness score measures the completeness of preparation evidence. It is not a valuation, an earnings verification, a guarantee of traffic, or a prediction that an asset will sell.

## Overview

Digital Splendid Portfolio Control Center is designed to help an owner prepare a portfolio of websites, domains, and related digital assets for due diligence and a possible sale. It is a separate companion plugin: the source listing remains the source of truth for public listing fields, while the Control Center stores private preparation details and report settings.

Main capabilities include:

- Import and synchronize WordPress `listing` posts into a private portfolio dashboard.
- Maintain a 19-item weighted sale-preparation checklist, grouped by traffic/SEO evidence, financial evidence, technical documentation, domain/ownership records, content inventory, and transfer/handover readiness.
- Calculate a private checklist-completeness score and recommend prioritized preparation tasks.
- Keep private notes and structured financial evidence for internal use.
- Refresh selected evidence from compatible Domain Details, Site Inventory, and Flipnzee Analytics sources.
- Discover likely GitHub repository and domain-specific analytics-report links from explicit homepage links, then require administrator review where matching is ambiguous.
- Prepare a buyer-facing evidence report using only the fields deliberately selected by the owner.
- Generate a browser-based printable report that can be saved to PDF.
- Restrict reports to named, logged-in buyers following a successful listing inquiry or offer when buyer-specific access is enabled.
- Review, revoke, or restore buyer access from the relevant asset's admin screen.

## What the readiness score means

The score is based on weighted checklist completion, with checklist weights totaling 100 points. In Automatic mode, supported checklist items can use evidence retrieved from connected sources; an administrator can manually override checklist statuses.

The score reflects **preparation completeness only**. It does not measure market value, profitability, legal ownership, site quality, or the likelihood of a sale. Review the supporting evidence and any automated statuses rather than relying on the number alone.

## Requirements and integrations

### Core requirements

- WordPress `6.0+`.
- PHP `7.4+`.
- Administrator access to install the plugin and manage the portfolio.
- Published WordPress posts using the `listing` post type for automatic import.

### Optional companion plugins and evidence sources

| Integration | What the Control Center uses | Important limitation |
|---|---|---|
| **Flipnzee Analytics** | Listing metadata and available cached aggregate GA4/Search Console metrics; compatible inquiry/offer event for buyer access | The Control Center reads cached metrics; it does not itself call Google Analytics or Search Console APIs. Buyer access integration requires a compatible Flipnzee Analytics release that dispatches the documented event. |
| **Flipnzee Site Inventory** | Filtered website inventory summary, when the source site's inventory endpoint is available | Only selected summary fields are stored. Raw API response data, user counts, roles, and drafts are not copied. |
| **Domain Details Display** | Registrar, registration date, renewal date, and related saved domain details from the local WordPress installation | Values are administrator-entered records, not independently verified registrar data. The plugin does not log in to registrars or query their APIs. |
| **Public Analytics Reports** | A link to a published domain-specific report page using the matching domain | Generic multi-site report hubs are not automatically treated as verified domain-specific evidence. |
| **GitHub** | A repository URL found in listing metadata, content, excerpt, or high-confidence homepage links | A repository is not assumed to belong to an asset merely because it is associated with a site-wide GitHub analytics plugin. Ambiguous candidates require review. |

Companion integrations are optional. If a plugin is missing, data is unavailable, or the source endpoint fails, the Control Center should mark that evidence as unavailable or needing review instead of inventing values.

## Installation

1. Back up your WordPress files and database.
2. In WordPress, open **Plugins → Add New Plugin → Upload Plugin**.
3. Upload the `digital-splendid-portfolio-control-center` ZIP package.
4. Install and activate the plugin.
5. Open **Portfolio Control Center** in the WordPress administration menu.
6. Click **Sync listings now** to import or refresh source `listing` posts, if needed.
7. Open an asset's scorecard, then use **Refresh evidence now** to refresh compatible Site Inventory, Domain Details, analytics-cache, and link-discovery evidence.
8. Review the detected values, checklist statuses, recommendations, and disclosure settings for each asset.

The plugin performs its database-table creation and schema upgrades through its activation/upgrade code. Back up first and verify upgrades in staging before updating a production site. Do not delete its database tables manually as a routine troubleshooting step.

## Typical workflow

### 1. Synchronize listings

The plugin imports available public listing fields from local `listing` posts, including the title/name, domain, website URL, niche/category, public listing URL, sale status, asking price, and currency where present. Source-owned listing fields are read-only in the Control Center; edit those fields in the original Listings interface.

Private notes, checklist selections, evidence references, financial evidence, and buyer-report settings are managed in the Control Center and are not overwritten as ordinary source-profile fields during synchronization. Trashed/deleted source listings are hidden from the active portfolio while saved checklist information is retained.

### 2. Refresh evidence

Open an asset scorecard and choose **Refresh evidence now**. Depending on the integrations installed and the data available, the refresh can:

- Request the source site's `/wp-json/flipnzee/v1/inventory` endpoint and save a filtered inventory summary.
- Match the asset domain to a local Domain Details record.
- Read aggregate analytics values already cached by Flipnzee Analytics.
- Discover explicit GitHub and domain-specific analytics-report links.
- Recalculate automatic checklist statuses from the refreshed evidence.

The inventory request uses WordPress safe HTTP request handling, a short timeout, limited redirects, and response-size constraints. Review source status and snapshot dates. An unavailable source is not proof that the underlying site has no traffic or content.

### 3. Complete the private checklist

Review each item in the readiness checklist. Use Automatic mode for supported items when connected evidence is available, or override a status manually when you have other reliable supporting information. The scorecard also provides prioritized next actions for common preparation gaps, such as missing traffic evidence, backups, transfer documentation, licences, and handover planning.

### 4. Record financial evidence privately

The private financial evidence section supports reporting period, currency, revenue, expenses, breakdown notes, and evidence references. These are owner-entered records. They are not independently audited and are not included in buyer reports by default. Do not place passwords, API keys, payment credentials, or other secrets in notes or evidence URLs.

### 5. Prepare the buyer evidence report

Open the asset scorecard and review **Buyer Evidence Report** settings.

- Report sharing is disabled until the owner enables the report.
- The owner selects which fields are disclosed for each asset.
- Use **Select available evidence** as a convenience for populated evidence fields, then review the selection before sharing.
- The internal readiness score and detailed checklist are excluded by default. Selecting them deliberately can disclose internal preparation status and gaps.
- An **Evidence included in this report** section describes the source type of included information and identifies missing or omitted evidence.
- Private notes and private financial records are not rendered in the buyer report.
- The public report is intended as a due-diligence evidence brief, not a duplicate of the complete sale listing.

The report is rendered by WordPress and printed by the buyer's browser. The plugin does **not** create a server-side PDF file. Use **Print / Save as PDF** to make a PDF copy.

## Buyer-specific report access

In version `0.18.1`, buyer-specific access is enabled by default for newly configured reports. When the report is enabled and buyer-specific access is required, a visitor must have a WordPress account and a successful listing-specific inquiry or offer before the report is displayed.

### Buyer journey

1. A buyer opens the public listing.
2. The buyer logs in or registers, then uses the existing Flipnzee Analytics inquiry/offer form on that listing.
3. Flipnzee Analytics must successfully save the inquiry or offer.
4. For account-bound access, the form email must match the email address on the logged-in WordPress account.
5. The compatible Flipnzee Analytics integration dispatches the `dsp_pcc_listing_inquiry_submitted` action with the listing ID, user ID, and message.
6. The Control Center records access for that WordPress user and the corresponding asset. That buyer can then open the report URL.

Guest inquiries can still be submitted through the marketplace, but guest submissions cannot grant account-bound buyer-report access. A logged-in account without an approved access record is denied. A buyer's access to one asset does not automatically grant access to every asset.

### Access administration and revocation

The asset's Portfolio Control Center screen provides buyer-access controls to review or revoke/restore access. A revoked buyer cannot regain access merely by submitting the inquiry again. While buyer-specific mode is active, the protected report endpoint does not accept a legacy share-token URL as a bypass.

**Revocation limitation:** Revoking access prevents further viewing of the live report through the protected endpoint. It cannot remotely delete a PDF that a buyer has already downloaded or a screenshot they have already captured.

### Compatibility hook

Compatible listing inquiry integrations can notify the Control Center after an inquiry or offer is successfully saved by firing:

```php
do_action(
    'dsp_pcc_listing_inquiry_submitted',
    $listing_id,
    $user_id,
    $message
);
```

Arguments are the source listing post ID, the logged-in WordPress user ID, and the saved inquiry/offer message. Dispatch this action **only after** successful persistence, and only when the logged-in user's submitted email has been verified to match their WordPress account email. Do not use this action as a substitute for validating the inquiry, user identity, or listing ID.

For custom listing templates that do not render the usual content filter, the Control Center provides this shortcode to place the access-instructions panel explicitly on an individual listing page:

```text
[dsp_pcc_buyer_report_request]
```

The shortcode displays the report-access instructions; it does not create a separate inquiry form.

## Evidence handling and privacy

The plugin is designed to minimize what is copied into buyer-facing reports:

- Analytics data is read from the existing Flipnzee Analytics cache. The plugin does not make new GA4 or Search Console API requests itself.
- Analytics snapshots contain aggregate measures such as active users, sessions, changes versus the prior 30-day period, Search Console clicks/impressions, source-reported query-row count, average session duration, and pages per active user where available.
- Individual search queries, detailed country/city information, traffic-source/referral detail, raw API responses, user counts/roles, and draft counts are not copied into the buyer report.
- The source query-row count is not represented as the total number of ranking keywords.
- Domain registration and renewal dates come from local records and are not independently checked with a registrar.
- Inventory fields are allowlisted and summarized. Raw inventory JSON and raw endpoint URLs are not exposed as buyer-facing evidence links.
- Analytics/report URLs are checked for domain relevance; unverified URLs are omitted from the buyer-facing report.
- Buyer report responses use noindex/no-follow and no-referrer protections.

The site owner remains responsible for deciding which evidence is appropriate to disclose and for reviewing claims before giving a buyer access. The plugin does not independently verify ownership, traffic, revenue, costs, or legal rights.

## Data storage

The plugin maintains its own WordPress database tables, using the active WordPress table prefix:

- `{prefix}dsp_portfolio_assets` — portfolio asset profiles, private notes, checklist data, evidence snapshots, financial evidence, and report settings.
- `{prefix}dsp_pcc_report_access` — asset-specific buyer access records, including listing/user IDs, status, request/grant times, and last access.

WordPress's active database prefix may not be `wp_`. Do not copy or edit these tables directly unless you have a tested backup and understand the plugin's state and schema. Use the provided admin screens and upgrade paths for normal operation.

## Troubleshooting

### No listings appear

- Confirm the source listing post is published and uses the `listing` post type.
- Open **Portfolio Control Center** and select **Sync listings now**.
- Check whether the source listing was trashed or deleted.

### Domain details are missing

- Confirm Domain Details Display is active on the same WordPress installation.
- Confirm the domain in the source listing matches the local Domain Details record after normalization.
- Remember that the Control Center does not retrieve domain data from a separate WordPress installation or query the registrar.

### Inventory is unavailable

- Confirm Flipnzee Site Inventory is active on the target website and exposes `/wp-json/flipnzee/v1/inventory`.
- Confirm the website URL is correct and HTTPS/network access works.
- Run **Refresh evidence now** again and review the source status/message.

### Analytics metrics are missing

- Confirm the source listing has working Flipnzee Analytics data and that its cache has been refreshed.
- Return to the Control Center and run **Refresh evidence now** again.
- Missing cached values are shown as unavailable; the Control Center does not issue fresh Google API requests.

### A GitHub or analytics report link is omitted

- Add the explicit repository/report link to the relevant listing metadata, content, or excerpt, or review the private homepage-discovery suggestions.
- A multi-site analytics hub is not automatically accepted as a dedicated report for a single domain.
- Refresh evidence and review the resulting candidate before including it in the buyer report.

### A buyer cannot open the report

- Confirm the report is enabled for that asset.
- Confirm buyer-specific access is enabled, if that is the chosen policy.
- Confirm the buyer is logged in with the WordPress account that submitted the listing inquiry/offer.
- Confirm the inquiry/offer email matches that account's email address and the inquiry was successfully saved.
- Confirm Flipnzee Analytics is a compatible version and the access event is dispatched after successful saving.
- Check the asset's buyer-access administration panel for that user and verify that access has not been revoked.
- Clear page/server caches after plugin updates so an old report page or listing template is not served.

### Buyer report panel is not displayed on a listing

The automatic panel uses the listing content filter. Some themes/builders render listing layouts without the normal `the_content` path. Add `[dsp_pcc_buyer_report_request]` to the individual listing template/content where appropriate, and ensure the asset is synced, the report is enabled, and buyer-specific access is active.

### Print / Save as PDF does not open

- Allow pop-ups for the WordPress site.
- Try the browser's print dialog and select **Save as PDF**.
- Disable browser print headers and footers for a cleaner report.
- Remember that the plugin does not store or deliver a server-generated PDF.

## Security recommendations

- Keep WordPress, PHP, the Control Center, and companion plugins updated.
- Use HTTPS for account login, inquiry submission, and report access.
- Back up the database before any update and test on staging first.
- Do not expose the private dashboard to non-administrator users.
- Review the buyer-report field selection before each sharing decision.
- Keep API keys, passwords, recovery codes, payment credentials, and private financial documents out of public notes and URLs.
- Test report access with at least one guest, one logged-in account without access, and one buyer account with an approved inquiry.
- Remember that saved PDFs and screenshots cannot be remotely revoked.

## Testing status

The release package has been through PHP syntax and ZIP-integrity checks in the release workflow, with focused source-level checks of report privacy and access-control behavior. The complete range of external services, WordPress themes/builders, email delivery, remote inventory endpoints, and companion-plugin combinations must still be verified on the deployment where the plugin is installed.

Before production use, test at least:

- [ ] Plugin activation and database upgrade on a staging copy.
- [ ] Listing synchronization and preservation of private fields.
- [ ] Evidence refresh with each installed companion plugin and with missing/unavailable sources.
- [ ] Default exclusion of the internal score and detailed checklist from buyer reports.
- [ ] Guest report request denied.
- [ ] Logged-in buyer without an approved access record denied.
- [ ] Logged-in buyer with a successful matching-email inquiry granted access to that asset only.
- [ ] Revoked buyer denied even after another inquiry.
- [ ] A previously saved token URL cannot bypass buyer-specific mode.
- [ ] PDF output and visible/clickable selected evidence links.

## Version history

See [`CHANGELOG.md`](CHANGELOG.md) for the release history. The current documented release is `0.18.1`.

## License

GPL-2.0-or-later. See the `LICENSE` file that should be included in the repository root when you publish the project.
