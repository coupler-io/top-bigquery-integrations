# top-bigquery-integrations

Export data from marketing, sales, and finance apps to BigQuery with no code. Explore Coupler.io's top integrations: what data you can move, why teams do it, and how to set it up in minutes.

Each page in this repo covers one source and answers the same three questions: how do I set the export up, what data can I actually get, and why would I bother. Everything is plain markdown, so the pages are equally readable on GitHub, in an editor, or by an AI assistant.

## Where to find what

| If you want to know | Go to |
| --- | --- |
| Which screens and fields the setup wizard has | **How to export** — one subsection per wizard step, with the real field names |
| How to let AI build the flow instead | **How to export → Bonus**, at the end of that section |
| Which reports, objects, or entities are available | **What data can you export** — one bullet per data category |
| Whether a specific metric or breakdown exists | **How to export → Metrics and dimensions** step |
| How often data can refresh | **How to export → Schedule** step |
| Where the data lands and how to control the region | **How to export → BigQuery destination** step |
| What other teams use this for | **Why export** — the use cases, ordered from most common to most specialised |
| Whether to transform in Coupler.io or in the warehouse | **Why export** — the two-route comparison table at the top |
| Another source that lands in BigQuery | **Other BigQuery integrations**, at the bottom of any page |

## How each page is structured

Every page follows the same order, so once you have read one you can skim the rest.

1. **Frontmatter** — machine-readable metadata (see below).
2. **Source and destination logos**, then the **H1**.
3. **Intro paragraph** — the limitation in the source's own interface, and what Coupler.io does about it.
4. **How to export `{source}` data to BigQuery** — the setup walkthrough. One `###` subsection per wizard step, in the order you meet them. Step names and field names are taken verbatim from the product, so what you read here matches what you see on screen. The final subsection is a bonus: describe the report in plain language and the Coupler.io AI Agent configures the flow for you.
5. **What data can you export from `{source}` to BigQuery with Coupler.io?** — every data category the connector exposes, one bullet each, with what it is useful for. Where a source offers dozens of report types, closely related ones are grouped but still named individually.
6. **Why export `{source}` to BigQuery — real life cases** — opens with a table comparing the no-code route against the warehouse-led route, then six to eight concrete use cases. The first is always the everyday reporting case; later ones get more specialised.
7. **Other BigQuery integrations** — links to every other page in this repo.

### Frontmatter fields

| Field | Meaning |
| --- | --- |
| `title` | The page's H1 |
| `slug` | URL-safe identifier, always matches the filename |
| `canonical` | Self-referencing canonical URL for this page |
| `source` | The connector's exact name in the Coupler.io app, which can differ slightly from the title |
| `destination` | Always BigQuery in this repo |
| `meta_title` | Same as `title` |
| `meta_description` | Search and social summary |

The `source` field is worth a look before you go hunting in the app: the QuickBooks connector is listed as *QuickBooks Reports*, LinkedIn Public Data as *LinkedIn Company Pages*, YouTube Analytics as *YouTube*, and Google Analytics as *Google Analytics 4 (GA4)*.

## Conventions

- **Markdown only.** No HTML, so headings and lists stay machine-readable and the pages chunk cleanly for retrieval.
- **Logos** are referenced from `static.coupler.io`, the same asset host the Coupler.io app uses, rather than committed as files.
- **Cross-page links** are absolute GitHub URLs so they still resolve if a page is quoted or scraped out of context.
- **Field and step names** are verbatim from the product. Only the explanatory copy around them is written for this repo.

## Integrations

### Ads and paid media

- [Connect Facebook Ads to BigQuery](./connect-facebook-ads-to-bigquery.md)
- [Connect Google Ads to BigQuery](./connect-google-ads-to-bigquery.md)
- [Connect Instagram Ads to BigQuery](./connect-instagram-ads-to-bigquery.md)
- [Connect LinkedIn Ads to BigQuery](./connect-linkedin-ads-to-bigquery.md)
- [Connect Microsoft Ads to BigQuery](./connect-microsoft-ads-to-bigquery.md)
- [Connect TikTok Ads to BigQuery](./connect-tiktok-ads-to-bigquery.md)

### Organic social

- [Connect Facebook Page Insights to BigQuery](./connect-facebook-page-insights-to-bigquery.md)
- [Connect Instagram Insights to BigQuery](./connect-instagram-insights-to-bigquery.md)
- [Connect LinkedIn Public Data to BigQuery](./connect-linkedin-public-data-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](./connect-tiktok-organic-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](./connect-youtube-analytics-to-bigquery.md)

### Web and search analytics

- [Connect Google Analytics to BigQuery](./connect-google-analytics-to-bigquery.md)
- [Connect Google Search Console to BigQuery](./connect-google-search-console-to-bigquery.md)

### Ecommerce

- [Connect Shopify to BigQuery](./connect-shopify-to-bigquery.md)
- [Connect WooCommerce to BigQuery](./connect-woocommerce-to-bigquery.md)

### Finance and accounting

- [Connect QuickBooks to BigQuery](./connect-quickbooks-to-bigquery.md)
- [Connect Xero to BigQuery](./connect-xero-to-bigquery.md)

<!-- Adding a page: create connect-{source}-to-bigquery.md following the structure above,
     add it to the matching category here, and add it to the "Other BigQuery integrations"
     list on every existing page. -->
