---
title: Connect LinkedIn Ads to BigQuery with Coupler.io
slug: connect-linkedin-ads-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-linkedin-ads-to-bigquery.md
source: LinkedIn Ads
destination: BigQuery
meta_title: Connect LinkedIn Ads to BigQuery with Coupler.io
meta_description: Load LinkedIn Ads analytics, sponsored leads, campaigns, creatives, video ads, and conversions into BigQuery on a schedule. No exports, no API work.
---

![LinkedIn Ads](https://static.coupler.io/importers/linkedin.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect LinkedIn Ads to BigQuery with Coupler.io

LinkedIn Campaign Manager shows you a slice of the truth at a time: one dimension per report, a hard ceiling of 18 metrics in a single pull, and leads sitting in a download queue nobody checks. For a channel where a single deal can justify the quarter's budget, that is a thin basis for decisions. Coupler.io loads LinkedIn Ads data into BigQuery on a schedule, as tables you keep, so campaign cost finally sits in one query with pipeline, revenue, and every other channel.

## How to export LinkedIn Ads data to BigQuery

Sign in to LinkedIn once, then walk the wizard. Nothing here calls for SQL or a developer, and most flows go live in a few minutes. Here is every screen and what belongs in it.

### Step 1. Basic settings

- **Ad accounts** — select every account you want in this table. Coupler.io pulls several at once and keeps each row labelled, so a multi-client or multi-region roll-up is one flow instead of one per account.
- **Report type** — the choice that reshapes the rest of the wizard. Pick the data category you want in BigQuery; all ten are described further down the page.

### Step 2. Report period

- **Start date** — where your history begins. Choose a relative option such as *days ago*, *this month*, *last month*, *this year*, or *last year* and the window moves forward by itself, so you set it once and never return. For a longer, more specific stretch, a macro like `{{365daysago}}` handles it.
- **End date** — where the window closes. Keep it relative and every run stays current with nobody touching the flow.
- **Split data by period** — the time grain of your rows: a totals line, or daily, monthly, or yearly. Daily is the one to pick if anyone might want a trend.

### Step 3. Metrics and dimensions

- **Metrics** — build exactly the table your team reports on, from impressions and reach through clicks, spend, conversions, engagement, video completions, and document reads. LinkedIn caps a single report at 18 metrics, so choose the ones that earn their place and build a second flow for the rest.
- **Dimension** — for single-dimension analytics, the one cut your report is grouped by: campaign, creative, company, member industry, job function, seniority, country, device, placement, and more.
- **Dimensions** — for multi-dimension analytics, several cuts at once, so campaign, objective, creative, and placement can live in the same rows instead of four separate reports.

### Step 4. Advanced settings

- **Advanced filters** — set property and value pairs to leave test campaigns and dormant accounts out of the warehouse rather than cleaning them up later.
- **Lead generation forms** — narrow to specific forms, or leave it empty to collect leads from every form in the account.

### Step 5. Blend and transform data

- **Column management** — hide fields nobody reads, rename raw API keys into labels a human understands, reorder them, and set the right data types.
- **Join** — match LinkedIn Ads to another source on a shared key, so ad spend and CRM outcomes arrive in one table.
- **Append** — stack Google Ads, Facebook Ads, and TikTok Ads under your LinkedIn rows for a single cross-channel spend table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — for anything else. Write the query and Coupler.io loads the result.

### Step 6. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it blank for US, or set it when data residency is part of the brief. If the dataset already exists, it keeps the region it was created in regardless of what you put here.
- **Dataset name** — type the name of a dataset you have, or a new one and Coupler.io creates it.
- **Table name** — the same. Name it and the table appears on the first run.

That is the whole destination step. Coupler.io works out the table structure from your data, so there is no column mapping and no type picking.

### Step 7. Schedule

Choose a refresh interval — every 15 minutes at the fastest, or hourly, daily, weekly, monthly — and decide whether each run replaces the table or appends to it. From there it runs itself. LinkedIn's rate limits, pagination, and retries are Coupler.io's responsibility, so there is no script to maintain and no quota error waiting for you on Monday.

### Bonus: or just ask the AI Agent to build it for you

Not keen on filling that in? You do not have to. The Coupler.io AI Agent sits inside the app — open it and describe the report you want in plain language:

> *"Load daily LinkedIn Ads performance into BigQuery — spend, impressions, clicks, CTR, and conversions by campaign and creative for the last year — plus all sponsored leads."*

It knows the report types and metrics LinkedIn exposes, so it picks the category, chooses the metrics and dimensions, sets the window, and configures the destination. Where there is a genuine decision it asks. Where there is not, it just gets on with it.

Four things remain yours:

1. **Connect LinkedIn Ads** — signing in to LinkedIn stays with you.
2. **Connect BigQuery** — so does your Google Cloud access.
3. **Pick the ad accounts** — you decide whose data is in scope.
4. **Name the dataset and table** — you say where in the warehouse it lands.

That is the entire manual part. Blending several sources into one table is the one thing you will finish in the wizard; the Agent handles everything else.

## What data can you export from LinkedIn Ads to BigQuery with Coupler.io?

Ten data categories are available. A flow carries one category, so most teams run several in parallel: a performance fact table plus the structural and lead tables around it. Coupler.io's built-in **Join** transformation stitches them together before load, so BigQuery receives one query-ready table and nobody has to write the join — or load each category as its own raw table and model it in the warehouse instead.

- **Ad analytics by single dimension** — your core numbers grouped by one cut: campaign, creative, company, country, industry, seniority, job function, device, or placement. The table most LinkedIn dashboards are built on, and the one that answers who is actually seeing the ads.
- **Ad analytics by multiple dimensions** — the same metrics with several cuts in the same rows, so campaign, objective, creative, and placement sit side by side instead of in four separate exports waiting to be reconciled.
- **Sponsored leads** — every submission from your lead generation forms, with the answers people gave and the campaign that produced them. Drops straight into CRM matching, and finally separates campaigns that generate leads from campaigns that generate customers.
- **Campaigns** — campaign setup as a reference table: objective, state, budget, schedule, and targeting approach. Gives every row of spend a name and an intent.
- **Campaign groups** — the layer above campaigns, which is where most budgets are really governed and where reporting rolls up for clients and business units.
- **Creatives** — the assets themselves, so you can compare formats, messaging, and calls to action over months instead of judging by whichever one you remember.
- **Video ads** — your video inventory, which is what makes view, completion, and full-screen figures interpretable rather than a column of numbers with no subject.
- **Shares (creative sponsored content)** — the sponsored posts behind your campaigns, linking paid delivery back to the content that carried it.
- **Conversions** — the conversion actions configured in your account, so you can see what LinkedIn is counting before you build anything on top of it.
- **Ad account users** — who has access to which account and at what level. Quietly valuable for agencies, audits, and anyone who has ever asked who approved this budget.

On the BigQuery side you name the dataset and table and choose the region a new dataset is created in, which matters as soon as data residency is a requirement. Coupler.io creates whatever does not exist and defines the table structure itself, reading the right types from your data.

## Why export LinkedIn Ads to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Put your LinkedIn Ads reporting on autopilot**

  The ordinary case, and the one that recurs every week: the report is due, and nobody wants to export another file from Campaign Manager. Coupler.io refreshes spend, impressions, clicks, CTR, and conversions into BigQuery on the schedule you choose, and Looker Studio, Power BI, or Tableau read from there. One dashboard, one version of the numbers, open to everyone who needs them and has no reason to ever log into the ad platform.

- **Get past the 18-metric ceiling without extra work**

  LinkedIn will not return more than 18 metrics in a single report, which is why so many B2B teams keep three spreadsheets and merge them by hand. Set up a flow per metric group, let Coupler.io's Join transformation combine them on campaign and date before load, and BigQuery receives the complete picture as one table. The platform limit stays where it is; it simply stops being your problem.

- **Connect ad spend to pipeline, not just to conversions**

  LinkedIn can tell you a form was submitted. It cannot tell you whether that lead became an opportunity, closed, or churned in four months. Coupler.io blends sponsored leads and ad analytics with your CRM data on the way into BigQuery, so cost per qualified lead, pipeline generated per campaign, and genuine return on ad spend arrive already calculated. For a channel with LinkedIn's cost per click, that is the difference between justifying the budget and defending it.

- **Route sponsored leads while they are still interested**

  Lead forms fill up faster than anyone remembers to check the download queue, and B2B response time is worth real money. Coupler.io moves sponsored lead submissions into BigQuery as often as every 15 minutes, matched against existing CRM records and ready for whatever routing your sales team already uses. The same table later tells you which forms and campaigns produce leads worth calling.

- **See LinkedIn next to the rest of your paid budget**

  LinkedIn is rarely the only invoice. Coupler.io's Append transformation stacks Google Ads, Facebook Ads, and TikTok Ads under your LinkedIn rows with consistent column names, so one BigQuery table holds total paid spend. That is the table that answers where the next increment should go, and it also shows where an expensive LinkedIn click still beats a cheap one somewhere else.

- **Report on accounts, not just on clicks**

  LinkedIn's company, industry, seniority, and job function dimensions are the closest any ad platform gets to account-based reporting. Loaded into BigQuery and matched against your target account list, they answer whether the companies you actually want are seeing and engaging with the campaigns, and which segments quietly absorb budget without ever appearing in pipeline. That is a conversation with sales, not just a marketing metric.

- **Work out which creatives and formats carry the campaign**

  Pulling creatives, video ads, and sponsored content together with their performance means comparing formats, headlines, and video completion rates across whole quarters takes a filter rather than another export. Creative fatigue shows up as a trend you can see starting, and the next round of production gets briefed from evidence instead of taste.

- **Keep a permanent record of what you spent and what it did**

  Ad platforms are not archives, and detail thins out the further back you look. A scheduled flow appending into BigQuery builds a long record of daily spend by campaign, creative, and audience segment, kept at full granularity in storage priced in cents per gigabyte. Year-over-year comparisons, retrospectives on last year's launch, and board questions about historical efficiency become queries rather than guesses.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
- [Connect Facebook Page Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md)
- [Connect Google Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-ads-to-bigquery.md)
- [Connect Google Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-analytics-to-bigquery.md)
- [Connect Google Search Console to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-search-console-to-bigquery.md)
- [Connect Instagram Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-ads-to-bigquery.md)
- [Connect Instagram Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-insights-to-bigquery.md)
- [Connect LinkedIn Public Data to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-linkedin-public-data-to-bigquery.md)
- [Connect Microsoft Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-microsoft-ads-to-bigquery.md)
- [Connect QuickBooks to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-quickbooks-to-bigquery.md)
- [Connect Shopify to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-shopify-to-bigquery.md)
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
