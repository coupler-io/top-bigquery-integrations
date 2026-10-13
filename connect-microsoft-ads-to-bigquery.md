---
title: Connect Microsoft Ads to BigQuery with Coupler.io
slug: connect-microsoft-ads-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-microsoft-ads-to-bigquery.md
source: Microsoft Ads (Bing Ads)
destination: BigQuery
meta_title: Connect Microsoft Ads to BigQuery with Coupler.io
meta_description: Load Microsoft Ads campaign, keyword, search query, shopping, audience, and billing reports into BigQuery on a schedule. Over forty report types, no API code.
---

![Microsoft Ads](https://static.coupler.io/importers/microsoft.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Microsoft Ads to BigQuery with Coupler.io

Microsoft Advertising has a genuinely deep reporting catalogue and a genuinely tedious way of getting at it. The UI builder hands you a download, the Reporting API hands you an asynchronous job to poll, and either way Microsoft ends up as the channel nobody refreshes because the process is too fiddly to repeat weekly. Coupler.io turns the whole catalogue into a wizard: pick a report, pick your columns, pick a dataset, and BigQuery stays current on its own.

## How to export Microsoft Ads data to BigQuery

Sign in to Microsoft once and work through the wizard. There is no code, no polling loop, and no developer required, and the fields adapt to the report you choose.

### Step 1. Basic settings

- **Ad accounts** — tick the accounts this flow covers. Select several and Coupler.io stacks them into one table with the account named on each row, which is how a managed-accounts roll-up becomes a single flow instead of a dozen.
- **Report type** — the choice everything else hangs off, with more than forty reports grouped by what they are for. It determines which columns, filters, and periods are available further on. The groups are described below.

### Step 2. Report period

- **Report period** — the quick way to set a window. Pick a named range such as today, yesterday, last 7 days, this month, last month, last 3 months, last 6 months, this year, or last year, and it keeps rolling without anyone touching it. Choose the custom option when you want to specify the dates yourself.
- **Start date** — with a custom period, where your history begins. Relative options like *days ago*, *this month*, *last month*, *this year*, and *last year* keep the window moving forward on their own, and a macro such as `{{365daysago}}` covers a specific trailing stretch.
- **End date** — where the custom window closes. Set it beyond today and the report simply stops at the latest data available.
- **Split data by period** — the grain of your rows. Summary for a single total, or hourly, daily, weekly from Sunday, weekly from Monday, monthly, or yearly. Daily is the safe choice if a trend chart is likely later.

### Step 3. Metrics and dimensions

- **Metrics** — build the measure list yourself for a custom report. Everything Microsoft exposes is here, from impressions, clicks, spend, and conversions through to impression share lost to budget, quality score components, and the video view milestones.
- **Dimensions** — the attributes those measures are cut by in a custom report: account, campaign, ad group, ad, keyword, asset, audience, device, location, search query, and the rest.

### Step 4. Dimensions

- **Dimensions** — for the prebuilt reports, choose which dimensions the report is grouped by. Every available metric for that report comes along automatically, so you only have to decide on the grain rather than tick through a column list.

### Step 5. Advanced settings

- **Advanced filters** — narrow the report before it is ever generated. Keep paused campaigns, irrelevant account types, or low-volume rows out of the warehouse instead of excluding them in every dashboard downstream.

### Step 6. Blend and transform data

- **Column management** — hide columns nobody reads, rename Microsoft's API names into labels a stakeholder recognises, reorder them, and fix the data types.
- **Join** — connect Microsoft Ads to another source on a shared key, so search spend and the outcomes it produced land together.
- **Append** — stack Google Ads, Facebook Ads, LinkedIn Ads, and Amazon Ads rows under your Microsoft data for one cross-channel spend table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — write the query and Coupler.io loads the result.

### Step 7. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it blank for US, or name a region when residency rules apply. If the dataset already exists it keeps its original region whatever you put here.
- **Dataset name** — an existing dataset or a new name, created for you on the first run.
- **Table name** — the same for the table.

That is the whole destination setup. Coupler.io works out the table schema from the data, so there is no column mapping and nothing to type by hand.

### Step 8. Schedule

Set the cadence: every 15 minutes at the quickest, then hourly, daily, weekly, or monthly. Choose whether each run replaces the table or appends to it, and that is the last time you think about it. Microsoft's report queueing, rate limits, and retries are handled on the Coupler.io side, so there is no asynchronous job of yours to poll and no failure to notice on Monday.

### Bonus: or just ask the AI Agent to build it for you

Forty-odd report types is a lot to read through, and you do not have to. The Coupler.io AI Agent is built into the app. Tell it what you want in plain language:

> *"Get me daily Microsoft Ads performance by campaign and keyword for the last 6 months, with spend, impressions, clicks, conversions, cost per conversion, and impression share, and load it into BigQuery."*

It knows the Microsoft Ads report catalogue and the columns each report supports, so it picks the report type, sets the dimensions and metrics, chooses the period and the split, and configures the destination. Where a real decision is needed it asks you. Where it is not, it just builds it.

Four things stay with you:

1. **Connect Microsoft Ads** — the Microsoft sign-in is yours.
2. **Connect BigQuery** — as is your Google Cloud access.
3. **Pick the ad accounts** — you decide which accounts are in scope.
4. **Name the dataset and table** — you choose where it lands.

That is all of the manual work. Advanced filters and multi-source blending are finished off in the wizard; everything else the Agent handles.

## What data can you export from Microsoft Ads to BigQuery with Coupler.io?

More than forty report types are available, organised into groups in the wizard. A flow produces one report, so most teams run a few in parallel: a performance fact table plus the structural and query-level reports around it. Coupler.io's **Join** transformation combines them before they reach BigQuery, so your warehouse gets one ready-to-query table and nobody writes the join. The alternative is equally sound: load each report as its own raw table and model it in BigQuery with dbt.

- **Frequently used** — Campaign performance report, Ad group performance report, Ad performance report, and Keyword performance report. The four tables that cover most day-to-day search reporting, from budget pacing down to individual keyword efficiency.
- **Performance** — the deeper analytical set: Account performance report, Ad dynamic text performance report, Asset performance report, Asset group performance report, Audience performance report, Conversion performance report, Destination URL performance report, Goals and funnels report, Publisher usage performance report, Search query performance report, Share of voice report, and the share-performance variants of the account, ad group, and campaign reports. This is where impression share, competitive position, and funnel completion live.
- **Targeting** — Age gender audience report, Geographic performance report, Professional demographics audience report, User location performance report, and Negative keyword conflict report. Who you reached, where they were, and where your own negatives are blocking traffic you meant to buy.
- **Product ads** — Product dimension performance report, Product partition performance report, Product partition unit performance report, Product search query performance report, Product match count report, and the share-performance variants. Everything a retail account needs to work out which feed items earn their place.
- **Dynamic search ads** — DSA auto target performance report, DSA category performance report, and DSA search query performance report. The visibility that automated targeting otherwise lacks, including what it decided to bid on.
- **Ad extensions** — Ad extension by ad report, Ad extension by keyword report, Ad extension detail report, and Call detail report. Extensions quietly carry a lot of clicks, and call details tie phone enquiries back to the ads behind them.
- **Hotel ads** — Hotel group performance report, Hotel dimension performance report, and their share-performance variants, for accounts running Microsoft's hotel inventory.
- **Billing and budget** — Budget summary report. What was actually spent against what was allocated, in the form finance wants to see it.
- **Change history** — Search campaign change history report. A timestamped record of who changed what in the account, which is what you join against performance when something moved overnight.
- **Custom** — build a report from scratch by choosing your own metrics and dimensions when none of the prebuilt shapes is quite right.

On the BigQuery side you name the dataset and the table and choose the region a new dataset is created in, which matters when data residency is mandated rather than chosen. Coupler.io creates anything that is missing and derives the table schema from the data.

## Why export Microsoft Ads to BigQuery — real life cases

Here is what teams actually do with the data once it lands in BigQuery.

- **Keep the search performance dashboard current without touching it**

  The ordinary reason, and the most common. Somebody needs yesterday's spend, impressions, clicks, conversions, and cost per conversion in a dashboard this morning, and the current process is a download from the reporting tab. Coupler.io refreshes the campaign and keyword reports into BigQuery on your schedule, and Looker Studio, Power BI, or Tableau read them straight from the table. One dashboard, one number, available to people who will never log into Microsoft Advertising.

- **Stop treating Microsoft as the channel nobody reports on**

  Microsoft Ads is usually smaller than Google, so it is the one that falls out of the weekly review and then quietly underperforms for a quarter. **Append** stacks Microsoft rows under Google Ads, Facebook Ads, and LinkedIn Ads with matching column names, so one BigQuery table holds total paid spend. Microsoft stops being a footnote and starts competing for budget on the same evidence as everything else.

- **Compare Microsoft and Google search on like-for-like terms**

  The two platforms share a vocabulary but not a definition, and the differences in conversion counting and match-type behaviour are exactly where bad conclusions come from. Load both into BigQuery, align the columns with **Column management** on the way in, and the comparison becomes honest. For a lot of accounts that is when somebody notices Microsoft's cost per conversion has been the better one all along.

- **Bid on the queries, not just the keywords**

  Search query reports are where the money leaks, and they are too big and too frequent to review by download. Scheduled into BigQuery, they give you months of query data you can filter, group, and aggregate at will. Negatives get added because a pattern showed up in the data, not because someone happened to scroll past it, and the Negative keyword conflict report tells you where your existing list is blocking traffic you intended to buy.

- **Work out where impression share is actually going**

  The share-performance reports tell you whether you lost an impression to budget or to rank, and that distinction leads to two completely different decisions. In BigQuery, with daily history behind it, you can see which campaigns consistently run out of budget before the day is over and which are losing on rank regardless of spend. That is a reallocation argument with numbers attached.

- **Tie search spend to revenue that actually landed**

  Platform conversion counts are a measurement, not an outcome. Coupler.io blends Microsoft Ads performance with your CRM, Stripe, or ecommerce data before load, so CAC, payback, and margin by campaign arrive in BigQuery already calculated. The channels that only look efficient in-platform get found out, and nobody has to rebuild a reconciliation spreadsheet at the end of the month.

- **Reconcile budget against what was actually spent**

  The Budget summary report in BigQuery, next to campaign-level spend and split the way your chart of accounts expects, answers the question finance keeps asking without anybody opening the billing tab. Month-end reconciliation turns into a query that has already run by the time it is needed.

- **Keep the detailed history and model it later**

  Microsoft's reporting interface is not built for multi-year questions, and granular query and product data is bulky enough that nobody keeps it locally. A scheduled append-only flow makes BigQuery the archive, in storage that costs cents per gigabyte, which is exactly where dbt models and scheduled queries belong. Year-over-year seasonality, pre- and post-restructure comparisons, and long-run keyword decay all become answerable instead of anecdotal.

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
- [Connect QuickBooks to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-quickbooks-to-bigquery.md)
- [Connect Shopify to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-shopify-to-bigquery.md)
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
