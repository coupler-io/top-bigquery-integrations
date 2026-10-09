---
title: Connect Facebook Ads to BigQuery with Coupler.io
slug: connect-facebook-ads-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md
source: Facebook Ads (Meta Ads)
destination: BigQuery
meta_title: Connect Facebook Ads to BigQuery with Coupler.io
meta_description: Load Facebook Ads insights, creatives, leads, activity logs, and Meta invoices into BigQuery on a schedule. No SQL scripts, no API quotas to babysit.
---

![Facebook Ads](https://static.coupler.io/importers/facebook.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Facebook Ads to BigQuery with Coupler.io

Meta's Ads Manager keeps roughly three years of history and locks it behind a UI that was never meant for analysis. Coupler.io moves your Facebook Ads data into BigQuery on a schedule you set, as clean tables you own forever — so campaign spend can finally sit next to revenue, CRM records, and every other channel in a single query.

## How to export Facebook Ads data to BigQuery

Sign in to Meta once, then walk the setup wizard. Nothing here needs SQL or a developer, and most flows are live in under five minutes. Here's every screen and what goes in it.

### Step 1. Basic settings

- **Report type** — the one decision that shapes everything else. Pick the data category you want in BigQuery and the wizard adapts around it. All nine are listed further down the page.
- **Ad accounts** — tick every account you want in this table. Pull several at once and Coupler.io stacks them together, labelled, so an agency roll-up is one flow instead of twelve.
- **Businesses** — for invoices, choose which Business Manager accounts to bill from.
- **Pages** — choose the Pages you want data for.

### Step 2. Report period

- **Start date** — where your history begins. Pick a relative option like *days ago*, *this month*, *last month*, *this year*, or *last year* and the window rolls forward on its own, so you set it once and never touch it again. Need something more specific, like a full trailing year? A macro such as `{{365daysago}}` handles it.
- **End date** — where the window closes. Keep it relative — *today* — and every run stays current without anyone editing the flow.
- **Split data by period** — how fine you want the time grain. Daily by default, which is what you want if you ever plan to chart a trend.

### Step 3. Date filters

- **Billing start date** — the first month of invoices to bring in. Leave it alone and you get the last six months.
- **Billing end date** — the last month to include. Empty means right up to today.
- **Issue start date** — prefer to work by the date Meta issued the invoice rather than the period it covers? Set this and it takes over.
- **Issue end date** — the cut-off for issue dates.

### Step 4. Metrics and dimensions

- **Metrics and dimensions** — build exactly the table you need. Start from the sensible defaults, add the conversion and cost metrics your team reports on, and skip the hundreds you don't.
- **Breakdowns** — the "why" behind a number. Split performance by placement, device, age, or geography to see which slice is carrying the campaign.

### Step 5. Advanced settings

- **Attribution settings** — inherit your ad account's attribution so BigQuery matches Ads Manager, or override it when you need a consistent standard across accounts.
- **Action attribution windows** — decide how long after a click or view a conversion still counts.
- **Filters** — leave test campaigns and retired accounts out of the warehouse instead of cleaning them up later.
- **Lead generation forms** — narrow to specific forms, or leave it empty to capture leads from all of them.
- **Ad creative columns** — choose which creative attributes travel with your ads. Blank brings everything; core properties always come along.
- **Updated since** — pull only what changed since a given date, so scheduled runs stay fast as the account grows.

### Step 6. Blend and transform data

- **Column management** — hide the fields nobody reads, rename cryptic API keys into labels a human understands, reorder them, and set the right data types.
- **Join** — stitch Facebook Ads to another source on a shared key, so campaign spend and CRM outcomes arrive in one table.
- **Append** — stack Google Ads, TikTok Ads, and LinkedIn Ads under your Meta rows for a single cross-channel spend table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — for everything else. Write the query and Coupler.io loads the result.

### Step 7. BigQuery destination

- **Data location** — the region a new dataset gets created in. Leave it empty for US, or set it to keep data where your compliance team needs it. Pointing at a dataset that already exists? It stays in its original region regardless.
- **Dataset name** — type an existing dataset or invent a new one; Coupler.io creates it for you.
- **Table name** — same deal. Name the table and it appears on first run.

Nothing else to fill in. Coupler.io defines the table schema from your data, so you don't map columns or pick types by hand.

### Step 8. Schedule

Pick a refresh interval — every 15 minutes at the fastest, or hourly, daily, weekly, monthly — and choose whether each run replaces the table or appends to it. From then on it runs itself. Meta's rate limits, async insights jobs, and retries are Coupler.io's problem, not yours: no Cloud Function to babysit, no 3 a.m. quota error.

### Bonus: or just ask the AI Agent to build it for you

Don't fancy filling in any of that? You don't have to. The Coupler.io AI Agent lives right inside the app — open it and describe the report you want in your own words:

> *"Pull daily Facebook Ads performance — spend, impressions, clicks, CPC, CTR, and purchases — broken down by campaign and placement for the last 12 months, and load it into BigQuery."*

It knows every field Facebook Ads exposes, so it picks the report type, selects the metrics and breakdowns, sets the date window, and sets up the destination. Where there's a real decision to make, it asks. Where there isn't, it just gets on with it.

You only handle four things yourself:

1. **Connect Facebook Ads** — signing in to Meta stays with you.
2. **Connect BigQuery** — your Google Cloud access does too.
3. **Pick the ad accounts** — you decide whose data is in scope.
4. **Name the dataset and table** — you say where in your warehouse it lands.

That's the whole manual part. If you're combining several sources into one table, you'll finish that bit in the wizard — everything else the Agent does for you.

## What data can you export from Facebook Ads to BigQuery with Coupler.io?

Nine data categories are available. One flow handles one category, so most teams run a few in parallel — a fact table for performance, dimension tables for structure. Coupler.io's built-in **Join** transformation stitches them together before they land, so BigQuery receives one ready-to-query table and nobody has to write the join — or load each category as its own raw table and model it in the warehouse instead.

- **Reports and Insights** — your numbers. Money out and results back: spend, reach, frequency, impressions, clicks, CPC, CPM, CTR, plus whatever conversion events you track. Slice it by day, week, or month, and cut it by audience age, gender, placement, device, or country. This is the table most dashboards are built on.
- **List of Campaigns** — the top of your account structure, with each campaign's objective, current state, budget ceiling, bidding approach, and the window it ran in. Gives every row of spend a name and a goal.
- **List of Ad sets** — how each ad set was actually set up: who it targeted, what it was optimizing toward, and how much budget it has left to spend. Useful for spotting which configurations quietly ate the budget.
- **List of Ads** — one row per ad, with its live state, what you're bidding, any policy or review problems Meta has flagged, and the ad set and campaign it belongs to.
- **List of Ad creatives** — your full asset library. Format, headline, body copy, call to action, destination links, and media files, including everything sitting unused.
- **List of Ads with ad creatives** — the same creative detail, but already attached to the ads that ran it. Saves you the join when you want to ask which headline or thumbnail actually performed.
- **List of Sponsored Leads** — every person who filled in a Meta lead form, with their answers, when they submitted, and which ad brought them in. Drops straight into your CRM matching logic.
- **List of Activities** — a timestamped record of every change in the account and who made it: budgets moved, ads paused, audiences edited, settings flipped.
- **List of Business invoices** — what Meta actually charged you, month by month and entity by entity, with the campaigns rolled into each bill. The number finance cares about.

On the BigQuery side you name the dataset and table and pick the region the dataset is created in — handy when data residency isn't optional. Coupler.io creates anything that doesn't exist yet and defines the table schema for you, reading the right types straight from your data.

## Why export Facebook Ads to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Put your Facebook Ads reporting on autopilot**

  The most common reason, and the least glamorous: you need yesterday's numbers in a dashboard this morning and nobody wants to export another CSV. Coupler.io refreshes spend, impressions, clicks, CPC, CTR, and conversions into BigQuery on whatever schedule you pick, and Looker Studio, Power BI, or Tableau read straight from there. Everyone opens the same dashboard and sees the same figures — including the people who would never log into Ads Manager.

- **See every paid channel in one place**

  Meta is never the only line item. Coupler.io's Append transformation stacks Google Ads, TikTok Ads, LinkedIn Ads, and Microsoft Ads underneath your Facebook rows with consistent column names, so a single BigQuery table holds total paid spend. That's the table that answers where the next budget increment should go, and it's the one nobody can rebuild by hand often enough to be useful.

- **Put Meta spend in the same query as actual revenue**

  Ads Manager can tell you what a campaign cost and what the pixel thinks it earned. It cannot tell you what those customers were worth 90 days later. Coupler.io blends Facebook Ads insights with your Stripe, Shopify, or HubSpot data on the way in, so blended CAC, payback period, and contribution margin by campaign arrive in BigQuery already calculated — no monthly spreadsheet reconciliation, and no waiting for an analyst to free up. Teams stop arguing about whose figure is right and start cutting the campaigns that only looked profitable in-platform.

- **Learn which creatives and audiences earn their budget**

  Coupler.io pulls your ads together with the creatives they actually ran, broken down by placement, device, age, or region, so comparing thumbnails, headlines, and calls to action across whole months takes a filter rather than a fresh export. Creative fatigue becomes something you catch as it starts, instead of after the CPA has already climbed. Works the same whether you're testing five creatives or five hundred.

- **Hand sponsored leads to sales while they're still warm**

  Lead forms fill up faster than anyone remembers to check them. Coupler.io moves sponsored lead submissions into BigQuery as often as every 15 minutes, ready to be matched against CRM records and routed however your sales team already works. It also gives you the honest answer to which campaigns produce leads that close, rather than leads that merely submit.

- **Keep the history Meta throws away**

  Meta's reporting retention is finite, and breakdown combinations get sparser the further back you look. A scheduled flow into BigQuery turns that into an append-only archive: five years of daily spend by campaign, creative, and placement, at full granularity, in storage that costs cents per gigabyte. Year-over-year seasonality, pre- and post-iOS-change comparisons, and "what did we do last Black Friday" all become answerable instead of anecdotal.

- **Audit who changed the account — and what it cost**

  The Activities log is the most underused dataset in Meta Ads. Have Coupler.io join it to your daily insights and the answer shows up in the table itself: CPA doubled the same afternoon someone widened a lookalike audience or paused a winning ad set. For agencies and in-house teams sharing one account, that turns a blame conversation into a timestamped record, and makes alerting on unapproved budget changes realistic rather than a someday project.

- **Close the books without exporting invoices by hand**

  Finance does not want screenshots of the billing tab. Coupler.io pulls Business invoices in next to your campaign-level spend, aggregated and split across client entities or cost centers the way your chart of accounts expects, then lands it in BigQuery for whatever reporting your controller already runs. Reconciling what Meta billed against what was budgeted stops being a two-hour end-of-month task and becomes something that has already happened by the time you look.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

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
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
