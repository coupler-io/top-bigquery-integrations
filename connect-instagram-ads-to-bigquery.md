---
title: Connect Instagram Ads to BigQuery with Coupler.io
slug: connect-instagram-ads-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-ads-to-bigquery.md
source: Instagram Ads
destination: BigQuery
meta_title: Connect Instagram Ads to BigQuery with Coupler.io
meta_description: Load Instagram Ads insights, campaigns, ad sets, and ads into BigQuery on a schedule. Pick your metrics and breakdowns in a wizard, no API work required.
---

![Instagram Ads](https://static.coupler.io/importers/instagram-ads.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Instagram Ads to BigQuery with Coupler.io

Instagram's paid performance lives inside Meta Ads Manager, where the reporting view is tuned for optimising today and not for answering questions about last quarter. Breakdowns get thinner the further back you scroll, exports top out at a CSV, and the moment you want Instagram spend in the same table as revenue you are writing API code. Coupler.io takes that off your plate: choose the report you want in a wizard, and your Instagram Ads data lands in BigQuery as a clean table that refreshes on its own.

## How to export Instagram Ads data to BigQuery

The whole setup is a short wizard. You sign in to Meta, say what you want, and point it at a dataset. No script, no service account juggling, no developer in the loop. Here is each screen.

### Step 1. Basic settings

- **Ad accounts** — select whichever accounts belong in this table. Choose more than one and Coupler.io loads them side by side with the account labelled on every row, which is how a multi-brand or multi-client roll-up becomes a single flow.
- **Report type** — the first and biggest choice. It decides what shape the table takes and which of the later fields you even see. The four options are described further down.

### Step 2. Report period

- **Start date** — the beginning of your window. Relative options such as *days ago*, *this month*, *last month*, *this year*, and *last year* keep moving forward with the calendar, so the flow stays correct without anybody opening it again. For a specific stretch like a rolling twelve months, a macro such as `{{365daysago}}` does the job.
- **End date** — where the window stops. Leave it relative and each run picks up the newest day automatically.
- **Split data by period** — the time grain of your rows. Daily, weekly, monthly, or totals only. Go daily if there is any chance someone will want a trend line later, since you can always roll it up and never break it back down.

### Step 3. Metrics and dimensions

- **Metrics and dimensions** — assemble the exact column list you report on. The defaults cover spend, reach, frequency, impressions, clicks, CPC, CPM, and CTR; add your conversion and value metrics on top and ignore the rest of the catalogue.
- **Breakdowns** — the context behind a flat number. Cut Instagram performance by placement, device, age, gender, region, or carousel card to find out which slice is actually carrying the account.

### Step 4. Advanced settings

- **Attribution settings** — follow the ad account's own attribution so your BigQuery table reconciles with Ads Manager, or set it yourself when several accounts need to be measured on one standard.
- **Action attribution windows** — choose how long a click, view, or engaged view keeps earning credit for a conversion, right down to SKAN postbacks and incrementality windows.
- **Filters** — keep test campaigns, paused accounts, and anything else you do not want to see out of the warehouse, instead of filtering it in every dashboard afterwards.

### Step 5. Blend and transform data

- **Column management** — drop columns nobody reads, rename raw API keys into words a stakeholder recognises, put them in a sensible order, and lock in the right data types.
- **Join** — match Instagram Ads against another source on a shared key, so paid performance and downstream outcomes arrive already connected.
- **Append** — stack Facebook Ads, TikTok Ads, Pinterest Ads, or Snapchat Ads under your Instagram rows to get one social spend table.
- **Aggregate** — summarise daily rows into the weekly or monthly grain your reporting runs on.
- **Custom SQL** — the escape hatch. Write a query and Coupler.io loads whatever it returns.

### Step 6. BigQuery destination

- **Data location** — the region in which a new dataset is created. Leave it blank for US, or name a region when residency rules say so. If the dataset is already there, it keeps the region it was born in whatever you type here.
- **Dataset name** — an existing dataset or a brand new name; Coupler.io creates it if it does not exist.
- **Table name** — same thing for the table, which appears on the first successful run.

Those three fields are the entire destination setup. Coupler.io reads the table schema from the data itself, so there is no column mapping to do and no types to guess.

### Step 7. Schedule

Choose how often the flow runs: every 15 minutes at the quickest, or hourly, daily, weekly, or monthly. Then decide whether each run replaces the table or appends to it. After that it is hands-off. Meta's rate limits, long-running insights jobs, and retries are handled on the Coupler.io side, so there is nothing to monitor and nothing to restart.

### Bonus: or just ask the AI Agent to build it for you

Rather not fill in a single field? You do not have to. The Coupler.io AI Agent sits inside the app. Open it and say what you are after in plain language:

> *"Give me daily Instagram Ads performance by campaign and placement for the last 6 months, including spend, impressions, clicks, CTR, and purchases, and load it into BigQuery."*

It already knows the Instagram Ads field catalogue, so it chooses the report type, picks the metrics and breakdowns, sets the period and the grain, and configures the destination. If a real judgement call comes up it checks with you. If not, it just builds the thing.

Four things stay in your hands:

1. **Connect Instagram Ads** — the Meta sign-in is yours.
2. **Connect BigQuery** — so is your Google Cloud access.
3. **Pick the ad accounts** — you decide which accounts are in scope.
4. **Name the dataset and table** — you choose where it lands in the warehouse.

That is the manual part in full. Merging several sources into one table is the one thing you will finish off in the wizard yourself; the Agent handles everything else.

## What data can you export from Instagram Ads to BigQuery with Coupler.io?

There are four report types. A flow covers one of them, so teams usually run a small set together: the insights report as the fact table and the structural lists as dimensions around it. The **Join** transformation in Coupler.io connects them before anything reaches BigQuery, so your warehouse gets one query-ready table and nobody writes the join by hand. Prefer to model in the warehouse? Load each report as its own raw table and let dbt take it from there.

- **Reports and Insights** — the performance table. Spend against reach, frequency, impressions, clicks, CPC, CPM, CTR, and the conversion events you care about, at a daily, weekly, or monthly grain, with breakdowns by audience, placement, device, or geography. This is what almost every Instagram dashboard sits on top of.
- **List of Campaigns** — the outer layer of your account, with each campaign's objective, status, budget cap, bid approach, and schedule. It turns anonymous spend rows into campaigns with stated goals you can hold them to.
- **List of Ad sets** — how targeting and optimisation were really configured: audience definition, optimisation goal, bid, and remaining budget. The quickest way to see which setup quietly consumed the month.
- **List of Ads** — a row per ad, including its current status, bid, the review or policy flags Meta has raised, and the ad set and campaign above it.

On the BigQuery side you give the dataset and table a name and choose the region a new dataset is created in, which matters when data residency is a requirement rather than a preference. Coupler.io creates whatever is missing and works out the table schema from the incoming data.

## Why export Instagram Ads to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

Here is what teams actually do once the data is in place. Either route arrives at the same destination; the no-code one just arrives before the end of the day.

- **Keep the weekly Instagram Ads report updating by itself**

  The ordinary case, and the one that comes up most. Somebody needs yesterday's spend, impressions, clicks, CTR, and conversions in a dashboard before the morning stand-up, and nobody wants to be the person exporting another CSV. Coupler.io refreshes the table in BigQuery on your schedule, and Looker Studio, Power BI, or Tableau read it directly. One dashboard, one set of numbers, available to everyone including the people who have never logged into Ads Manager.

- **Separate Instagram's contribution from Facebook's**

  Running a Meta campaign across both apps makes the blended figure comfortable and useless. Pull Instagram Ads as its own table, stack it with Facebook Ads using **Append**, and BigQuery holds a clean side-by-side comparison of CPM, CTR, and cost per result by app. That is the table that tells you whether Instagram deserves a bigger share of the next budget cycle or is being flattered by the average.

- **Rank every paid channel on the same page**

  Instagram is one line in a media plan that also has search, video, and other social in it. **Append** puts Google Ads, TikTok Ads, LinkedIn Ads, and Microsoft Ads underneath your Instagram rows with matching column names, so total paid spend and blended efficiency sit in one BigQuery table. Nobody can keep a cross-channel spreadsheet current by hand, which is exactly why this gets rebuilt every quarter until it is automated.

- **Measure Instagram spend against money that actually arrived**

  In-platform conversion counts are a proxy, not a result. Coupler.io blends Instagram Ads insights with Stripe, Shopify, or HubSpot data on the way into BigQuery, so CAC, payback window, and margin by campaign come out already worked through. Finance and marketing stop reconciling two versions of the same month and start talking about which campaigns are genuinely profitable.

- **Find the creative and placement combinations worth repeating**

  Reels, Stories, and feed placements behave nothing alike, and a flat campaign average buries that. With breakdowns by placement, device, age, and region landing in BigQuery every day, comparing performance over whole months takes a filter instead of a fresh export. Fatigue shows up as a trend you spot early rather than a CPA jump you explain afterwards.

- **Hold on to the history Meta eventually drops**

  Meta's reporting retention has a limit, and detailed breakdown combinations get sparse well before that limit. A scheduled append-only flow turns BigQuery into your own archive: years of daily Instagram spend by campaign, ad set, and placement at full detail, in storage that costs cents per gigabyte. Year-over-year seasonality and "what did we run last Black Friday" become queries rather than guesses.

- **Report on every client account from one table**

  Agencies lose real hours to repeating the same export across a dozen accounts. Select them all in one flow, let Coupler.io label and stack the rows, and BigQuery holds a single table that can be sliced per client for their deck or read whole for an internal margin review. Onboarding a new account means ticking one more box, not cloning a pipeline.

- **Give the whole account structure a shared vocabulary**

  Performance numbers alone cannot tell you whether a campaign was meant to drive reach or purchases. Join the insights report to the campaign, ad set, and ad lists and every spend row in BigQuery carries its objective, targeting, and status with it. Naming-convention archaeology stops being a monthly ritual, and reporting by objective, funnel stage, or market becomes something you can actually model.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
- [Connect Facebook Page Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md)
- [Connect Google Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-ads-to-bigquery.md)
- [Connect Google Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-analytics-to-bigquery.md)
- [Connect Google Search Console to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-search-console-to-bigquery.md)
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
