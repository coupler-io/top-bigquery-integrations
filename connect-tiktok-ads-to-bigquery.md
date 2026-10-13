---
title: Connect TikTok Ads to BigQuery with Coupler.io
slug: connect-tiktok-ads-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md
source: TikTok Ads
destination: BigQuery
meta_title: Connect TikTok Ads to BigQuery with Coupler.io
meta_description: Move TikTok Ads performance, audience, playable, DSA, and GMV Max reports into BigQuery on a schedule. No async report polling, no CSV exports.
---

![TikTok Ads](https://static.coupler.io/importers/tiktok.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect TikTok Ads to BigQuery with Coupler.io

TikTok Ads Manager is built for launching campaigns, not for answering questions about them. Every breakdown is a separate screen, every export is a fresh CSV, and nothing you pull yesterday is still current today. Coupler.io takes the same reports TikTok exposes and lands them in BigQuery on a schedule, as tables you keep — so TikTok spend can sit in one query next to Meta, Google, your store, and your CRM.

## How to export TikTok Ads data to BigQuery

Authorize TikTok once, then work through the wizard. There is no script to write and no developer to borrow, and a straightforward flow is usually live before your coffee goes cold. Here is each screen and what belongs in it.

### Step 1. Basic settings

- **Report type** — the first and biggest choice, because the rest of the wizard reshapes itself around it. Pick the data category you want sitting in BigQuery; all seventeen are described further down this page.
- **Ad accounts** — tick as many advertiser accounts as you want in one table. Coupler.io pulls them together and keeps each row labelled, which turns an agency roll-up into a single flow rather than one per client.
- **Stores** — for GMV Max reporting, choose which connected stores the campaign results should come from.

### Step 2. Report period

- **Start date** — where your window opens. Choose a relative option such as *days ago*, *this month*, *last month*, *this year*, or *last year* and the range keeps moving on its own, so you configure it once and forget it. For a longer, more specific stretch, a macro like `{{365daysago}}` does the job.
- **End date** — where the window closes. Keep it relative and every scheduled run brings back current numbers without anyone opening the flow.
- **Split data by period** — the time grain of your rows. Daily, hourly where the report supports it, or a single totals line. Go daily if there is any chance someone will want a trend line.

### Step 3. Metrics and dimensions

- **Metrics** — assemble the exact column set your report needs. Sensible defaults are preselected; add the cost, conversion, video, and engagement figures your team actually reviews and ignore the rest.
- **Audience dimensions** — split audience reports by gender, age, country, province, DMA region, language, platform, device brand, placement, behavior, or interest tier. This is where you find out which slice of the audience is quietly carrying the campaign.
- **DSA dimensions** — for Dynamic Showcase Ads, group results by catalog, product set, product, or SKU, so performance lands at the level your merchandising team thinks in.
- **Targeting dimension** — add a country or ad type cut to a basic report without building a second flow.
- **Asset dimension** — group by the creative asset behind the numbers: video material, image, component, page, post, room, event source, or custom event type.
- **Search dimension** — break performance out by search terms when you want to see what people typed before they arrived.

### Step 4. Advanced settings

- **Advanced filters** — set property and value pairs to keep paused tests, retired campaigns, and anything else that pollutes a dashboard out of the warehouse in the first place.

### Step 5. Blend and transform data

- **Column management** — hide columns nobody reads, rename raw API keys into words a human recognizes, put them in a sensible order, and set the right data types.
- **Join** — match TikTok Ads against another source on a shared key, so campaign cost and downstream outcomes arrive as one table.
- **Append** — stack Facebook Ads, Google Ads, and LinkedIn Ads rows under your TikTok rows to build a single cross-channel spend table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — for anything the options above do not cover. Write the query and Coupler.io loads the result.

### Step 6. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it blank for US, or set it when your compliance team cares where the bytes live. If you point at a dataset that already exists, it keeps the region it was created in no matter what you type here.
- **Dataset name** — type the name of a dataset you already have, or make one up and Coupler.io creates it.
- **Table name** — the same. Name the table and it shows up on the first run.

That is the entire destination setup. Coupler.io works the table structure out from your data, so there is no column mapping and no type picking to do.

### Step 7. Schedule

Choose how often the flow runs — every 15 minutes at the quickest, or hourly, daily, weekly, monthly — and decide whether each run replaces the table or appends to it. After that it looks after itself. TikTok's rate limits, asynchronous report jobs, and retries belong to Coupler.io now, so there is no Cloud Function to keep alive and no overnight quota failure waiting for you.

### Bonus: or just ask the AI Agent to build it for you

Would rather not fill any of that in? You do not have to. The Coupler.io AI Agent sits inside the app — open it and describe the report you want in plain language:

> *"Give me daily TikTok Ads performance by campaign and ad — cost, impressions, clicks, CTR, CPC, and conversions — for the last six months, loaded into BigQuery."*

It knows every report type and metric TikTok Ads offers, so it chooses the report, selects the metrics and dimensions, sets the period, and configures the destination. When there is a genuine decision to make it asks you. When there is not, it simply proceeds.

Four things stay in your hands:

1. **Connect TikTok Ads** — authorizing TikTok is yours.
2. **Connect BigQuery** — so is your Google Cloud access.
3. **Pick the ad accounts** — you decide whose data is in scope.
4. **Name the dataset and table** — you say where in the warehouse it lands.

That is the whole manual part. Combining several sources into one table is the one thing you will finish in the wizard; the Agent handles the rest.

## What data can you export from TikTok Ads to BigQuery with Coupler.io?

Seventeen report types are available. A flow carries one report type, so most teams run several side by side: a performance fact table plus the structural tables around it. Coupler.io's built-in **Join** transformation puts them together before they ever reach the warehouse, so BigQuery receives one query-ready table and nobody writes the join — or you load each one as its own raw table and model it warehouse-side instead.

- **Basic campaign report** — the workhorse. Cost, impressions, clicks, conversions, CTR, CPC, CPM, and cost per conversion at campaign level, by day or hour. Most TikTok dashboards start here.
- **Basic ad group report** — the same figures one level down, which is where bidding and targeting decisions actually live and where overspend usually hides.
- **Basic ad report** — performance for each individual ad, so you can tell a strong idea from a strong placement.
- **GMV Max campaign report** — the commerce view of automated campaigns: cost, net cost, SKU orders, cost per order, gross revenue, and ROI. The report finance will recognize.
- **Campaigns** — your campaign structure as a reference table, so every row of spend has a name, an objective, and a setup behind it.
- **Ads** — one row per ad with its attributes, which is what makes creative-level analysis readable instead of a wall of IDs.
- **Basic advertiser report** — account-level totals. Useful as the top line of an agency or multi-brand summary without rebuilding it from campaign rows.
- **Audience campaign report** — campaign results broken out by who saw them: age, gender, country, interest, device, placement, and more.
- **Audience ad group report** — the same demographic cut at ad group level, which is where your targeting assumptions get tested against reality.
- **Audience ad report** — demographics per ad. The table that tells you which creative lands with which cohort.
- **Audience advertiser report** — account-wide audience composition, handy for checking whether the people you reach resemble the people you meant to reach.
- **Playable Ad report** — engagement and cost for playable creatives, each with its name and current state, so interactive formats can be judged against standard ones.
- **Playable Ad report by country** — the same playable figures split by market, for when one region flatters the global average.
- **Dynamic Showcase Ads (DSA) campaign report** — catalog campaign performance, grouped the way your product feed is organized.
- **Dynamic Showcase Ads (DSA) report** — DSA results at ad level, down to product and SKU.
- **Dynamic Showcase Ads (DSA) ad group report** — catalog performance per ad group, which is the level most feed tuning happens at.
- **Dynamic Showcase Ads (DSA) advertiser report** — the account-wide catalog picture for a quick read on how commerce campaigns are doing overall.

On the BigQuery side you name the dataset and table and pick the region a new dataset is created in, which matters the moment data residency stops being optional. Coupler.io creates whatever does not exist yet and defines the table structure for you, reading the types straight from the data.

## Why export TikTok Ads to BigQuery — real life cases

Here is what teams actually do with the data once it lands in BigQuery.

- **Put your TikTok Ads reporting on autopilot**

  The ordinary reason, and the one that comes up every single morning: somebody wants yesterday's numbers in a dashboard and nobody wants to download another CSV. Coupler.io refreshes cost, impressions, clicks, CTR, CPC, and conversions into BigQuery on the schedule you choose, and Looker Studio, Power BI, or Tableau read from there. The whole team opens one dashboard and sees one set of figures, including everyone who has never logged into Ads Manager and never will.

- **See TikTok next to every other paid channel**

  TikTok is a line in the budget, not the budget. Coupler.io's Append transformation stacks Facebook Ads, Google Ads, LinkedIn Ads, and Microsoft Ads under your TikTok rows with matching column names, so one BigQuery table holds total paid spend across platforms. That is the table that settles where the next increment of budget should go, and the one that nobody can rebuild by hand often enough to stay useful.

- **Compare what TikTok reports against what actually sold**

  In-platform conversion counts and your order table rarely agree, and the gap grows with the purchase cycle. Coupler.io blends TikTok Ads results with Shopify, Stripe, or HubSpot data before load, so blended acquisition cost, payback window, and margin by campaign arrive in BigQuery already worked out. The monthly spreadsheet reconciliation disappears, and so does the argument about whose number is correct.

- **Find out which creatives earn their budget**

  Creative is where TikTok performance is won, and Ads Manager makes comparing creatives over time genuinely tedious. With ad-level reports in BigQuery, alongside asset dimensions and the playable reports, comparing videos, hooks, and interactive formats across whole months is a filter instead of another export. Fatigue becomes something you catch while it starts rather than something you diagnose after the cost per acquisition has already climbed.

- **Check who you are actually reaching**

  Targeting is a hypothesis until somebody measures it. The audience reports break results out by age, gender, country, interest, device, and placement, and once they sit in BigQuery you can hold them against your real customer profile from the CRM. Teams routinely discover a cohort that converts well and was never in the plan, and another that absorbed budget for weeks without buying anything.

- **Report on catalog and GMV Max commerce properly**

  DSA and GMV Max reports speak in catalogs, product sets, SKUs, orders, gross revenue, and ROI. Coupler.io joins them to your product, inventory, and margin data on the way into BigQuery, so profitability lands at SKU level instead of campaign level. That turns the question from which campaign performed into which products are worth advertising at all, which is a different and considerably more useful question.

- **Keep the history the platform does not hold for you**

  Advertising platforms are not built as archives, and the further back you look the thinner the breakdowns get. A scheduled flow appending into BigQuery turns that into a permanent record: years of daily spend by campaign, ad, audience, and creative, kept at full granularity in storage priced in cents per gigabyte. Seasonality, year-over-year comparisons, and questions about last year's launch become answerable with a query rather than from memory.

- **Give every client the same report without the same work**

  Agencies running several advertiser accounts normally pay for that in repetitive manual exports. One flow can pull multiple ad accounts at once with each row labelled, land them in a shared BigQuery dataset, and feed one dashboard template filtered per client. Adding the next account becomes a tick box rather than a new process.

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
- [Connect Shopify to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-shopify-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
