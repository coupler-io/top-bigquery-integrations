---
title: Connect Shopify to BigQuery with Coupler.io
slug: connect-shopify-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-shopify-to-bigquery.md
source: Shopify
destination: BigQuery
meta_title: Connect Shopify to BigQuery with Coupler.io
meta_description: Load Shopify orders, line items, customers, products, inventory, transactions, and refunds into BigQuery on a schedule. No apps to build, no API versions to chase.
---

![Shopify](https://static.coupler.io/importers/shopify.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Shopify to BigQuery with Coupler.io

Shopify's admin reports answer the questions Shopify decided you would ask. The moment you want unit economics by product across two stores, or a cohort that follows buyers past their first order, you are back in the export screen downloading another CSV that goes stale by lunchtime. Coupler.io lifts your store data into BigQuery as proper tables, refreshed on your schedule, where orders can finally sit beside ad spend, subscriptions, and the rest of the business.

## How to export Shopify data to BigQuery

Authorize your store once and the wizard takes it from there. There is no app to publish, no private credential to rotate, and no GraphQL to learn. Here is every screen and what belongs in it.

### Step 1. Basic settings

- **Data entity** — the single choice everything else follows from. Say whether this flow is about orders, customers, products, inventory, or one of the finer order breakdowns, and the rest of the wizard reshapes itself. The full catalogue is further down the page.

### Step 2. Columns

- **Columns** — the table you want, not the one the API hands over. Start from the defaults, bring in the money fields, address fields, customer journey attributes, or tags your reporting actually uses, and put the order index wherever it reads best. Leaving it untouched is fine too.

### Step 3. Date filters

- **Created after date** — where your history starts. A relative setting such as *days ago*, *this month*, *last month*, *this year*, or *last year* keeps the window moving on its own, so you configure it once and forget it. For a precise trailing span, a macro like `{{365daysago}}` does the job.
- **Created before date** — the upper bound on creation date. Leave it open and every run stays current.
- **Changed after date** — the efficient option for scheduled flows: fetch only the records that moved since your last load, so a store with half a million orders still refreshes quickly.
- **Changed before date** — closes the modification window when you are rebuilding one specific period rather than the whole archive.

### Step 4. Advanced filters

- **Filter after ID** — begin from a known record and walk forward. Handy for backfilling a large catalogue in deliberate chunks instead of one enormous first run.
- **Status** — restrict orders to the lifecycle state you report on, so cancelled and archived rows do not quietly distort averages.
- **Financial status** — separate paid from pending, partially refunded, and voided before the data lands, which is what finance will ask for anyway.
- **Fulfillment status** — isolate what has shipped, what is partly out the door, and what is still sitting in the warehouse.
- **Status** — for products, decide whether drafts and archived items belong in the table or whether you only want what customers can actually buy.

### Step 5. Blend and transform data

- **Column management** — drop the fields nobody opens, give API keys names a merchandiser recognises, order them sensibly, and fix the data types.
- **Join** — attach another source on a shared key, so orders arrive already carrying the campaign, subscription, or support record that explains them.
- **Append** — stack a second and third storefront under the first, with matching columns, for one combined table across every region you sell in.
- **Aggregate** — collapse order-level rows into the daily, weekly, or monthly grain your reporting model is built on.
- **Custom SQL** — when none of the above fits, write the query yourself and Coupler.io ships the result.

### Step 6. BigQuery destination

- **Data location** — the region a brand new dataset is created in. Leave it blank for US, or set it where your data residency rules point. If you aim at a dataset that already exists, it keeps the region it was born in.
- **Dataset name** — type an existing dataset or invent one; Coupler.io creates it if it is missing.
- **Table name** — same idea. Name it and it shows up on the first run.

That is the entire destination step. Coupler.io derives the table schema from your data, so no column mapping and no type juggling.

### Step 7. Schedule

Choose how often it should run — every 15 minutes at the quickest, or hourly, daily, weekly, monthly — and decide whether each run replaces the table or appends to it. After that it looks after itself. Pagination, bulk operation polling, throttling, and retries are Coupler.io's job, not something your team maintains.

### Bonus: or just ask the AI Agent to build it for you

Rather not fill any of that in? You do not have to. The Coupler.io AI Agent sits inside the app. Open it and describe the table you want in plain language:

> *"Give me Shopify orders with line items for the last two years — product, quantity, discounts, taxes, and net revenue per line — filtered to paid orders, and load it into BigQuery."*

It already knows what Shopify exposes, so it chooses the data entity, selects the columns, sets the date window, applies the status filters, and configures the destination. When a judgement call is genuinely yours, it asks. When it is not, it proceeds.

Four things stay in your hands:

1. **Connect Shopify** — authorising the store is yours to do.
2. **Connect BigQuery** — so is your Google Cloud access.
3. **Pick the store** — you choose which storefront is in scope.
4. **Name the dataset and table** — you decide where in the warehouse it lands.

That is the full manual portion. If several stores are being combined into one table, you will wrap that up in the wizard; the Agent handles the rest.

## What data can you export from Shopify to BigQuery with Coupler.io?

Twelve data entities are available. A flow carries one entity, so most stores run several side by side — a transactional table at the centre, dimension tables around it. Coupler.io's built-in **Join** transformation can weld them together before the load, so BigQuery receives one query-ready table and nobody writes the join by hand. Or land each entity raw and model it in the warehouse instead, if that is how your stack works.

- **Orders** — the spine of ecommerce reporting. One row per order with totals, discounts, taxes, shipping, currency, payment gateways, financial and fulfillment state, plus the customer attached to it. Most revenue dashboards start and end here.
- **Customers** — who buys from you, how much they have spent in total, how to reach them, and where they are. The base layer for segmentation and retention work.
- **Inventory** — what is physically available, by item and by location. The table that tells you whether a bestseller is about to embarrass you.
- **Products** — your catalogue as data: titles, handles, types, vendors, status, and the tags you merchandise by.
- **Products with variants** — the same catalogue opened up to SKU level, so size, colour, price point, and stock-keeping detail each get their own row. Essential when the interesting variation happens below the product.
- **Orders with line items** — every product line inside every order. This is the one that answers what actually sold, at what price, with which discount applied, and in what combination with everything else in the basket.
- **Orders with activities** — the agreements and changes recorded against an order over its life, so edits and amendments stop being invisible.
- **Orders transactions** — the payment side: captures, authorizations, and the gateway behind each one. What was charged, when, and through whom.
- **Orders refunds transactions** — money going back out, matched to the transactions it reverses. The honest view of net revenue rather than gross.
- **Orders with refunded items** — refunds broken down to the item that was sent back, which is how you find the product with a quiet returns problem.
- **Orders with shipping lines** — the shipping methods chosen and what was charged for them, so delivery can be measured as a margin line rather than a rounding error.
- **Order fulfillments with line items** — what left the warehouse, when, and which items were in each shipment. The basis for any serious operations or delivery-speed reporting.

On the BigQuery side you give the dataset and table a name and pick the region a new dataset is created in, which matters when residency is a requirement rather than a preference. Coupler.io creates whatever does not exist yet and works out the table schema from the data itself.

## Why export Shopify to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Run your daily store reporting without touching an export button**

  The ordinary reason, and by far the most common: somebody needs yesterday's orders, revenue, AOV, and refunds in a dashboard before the morning stand-up. Coupler.io refreshes those tables into BigQuery on whatever cadence you choose, and Looker Studio, Power BI, or Tableau read them directly. One dashboard, one set of numbers, available to people who have no reason to ever open the Shopify admin.

- **Work out what each product really earns you**

  Gross sales flatter everything. Pull orders with line items alongside refunds and shipping lines and the picture changes: discount depth, return rate, and delivery cost come off the top, and some of your bestsellers turn out to be your worst margins. Coupler.io assembles that view before the load, so the table in BigQuery is already the one you would have spent an afternoon building in a spreadsheet.

- **Put store revenue next to the ad spend that produced it**

  Shopify knows what sold. It does not know what you paid to make it sell. Coupler.io blends orders with Facebook Ads, Google Ads, and TikTok Ads spend on the way in, so blended ROAS, customer acquisition cost, and payback by channel arrive in BigQuery already worked out. Budget conversations get shorter when everyone is reading the same table.

- **Follow customer cohorts past the first order**

  Lifetime value is the number that decides how much you can afford to spend on acquisition, and it is invisible in a dashboard that resets every month. With customers and orders in BigQuery you can group buyers by the month, channel, or first product they arrived through, then watch repeat rate and spend develop over the following year. Coupler.io keeps the underlying tables fresh so the cohort grid stays accurate without anyone rebuilding it.

- **Catch stockouts before they cost you a weekend**

  Inventory on its own is a snapshot. Inventory refreshed every 15 minutes next to recent sales velocity is an early warning system. Teams land both in BigQuery, work out days of cover per SKU per location, and trigger a reorder alert while there is still time to act, rather than discovering the gap in Monday's revenue figure.

- **Report across every storefront as one business**

  Separate stores for separate markets is sensible operationally and miserable for reporting. Coupler.io's Append transformation stacks each store's orders with consistent column names, labelled by source, so a single BigQuery table covers the whole group. Regional comparison stops being a manual consolidation exercise and becomes a filter on a chart.

- **Give finance refunds and transactions they can reconcile**

  Transactions and refund transactions land in BigQuery in the shape a controller expects: what was captured, through which gateway, what came back, and against which order. Reconciling payouts against recorded revenue turns into a query rather than a week of cross-checking two exports, and the numbers in the board pack stop needing a footnote.

- **Keep the full order history your reporting tier will not**

  In-platform reporting gets thinner the further back you look, and admin exports are not an archive. A scheduled append-only flow turns BigQuery into a permanent record of every order, line item, and refund at full granularity, in storage that costs very little per gigabyte. Year-over-year comparisons, last Black Friday, and the long tail of a product's lifecycle all become answerable from the warehouse.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
- [Connect Facebook Page Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md)
- [Connect Google Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-ads-to-bigquery.md)
- [Connect Google Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-analytics-to-bigquery.md)
- [Connect Google Search Console to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-search-console-to-bigquery.md)
- [Connect Instagram Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-ads-to-bigquery.md)
- [Connect Instagram Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-insights-to-bigquery.md)
- [Connect LinkedIn Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-linkedin-ads-to-bigquery.md)
- [Connect LinkedIn Public Data to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-linkedin-public-data-to-bigquery.md)
- [Connect Microsoft Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-microsoft-ads-to-bigquery.md)
- [Connect QuickBooks to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-quickbooks-to-bigquery.md)
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
