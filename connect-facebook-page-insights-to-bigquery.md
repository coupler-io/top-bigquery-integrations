---
title: Connect Facebook Page Insights to BigQuery with Coupler.io
slug: connect-facebook-page-insights-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md
source: Facebook Page Insights
destination: BigQuery
meta_title: Connect Facebook Page Insights to BigQuery with Coupler.io
meta_description: Load organic Facebook Page overview, performance, follower snapshot, post, reel, and video reports into BigQuery on a schedule, with daily snapshots kept as history.
---

![Facebook Page Insights](https://static.coupler.io/importers/facebook.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Facebook Page Insights to BigQuery with Coupler.io

Organic Page performance is the part of Meta reporting nobody has automated. Business Suite gives you a handful of fixed date ranges, a follower chart that resets, and lifetime post numbers with no way to see how they accumulated. If you manage more than one Page it gets worse, because there is no view that covers them together. Coupler.io exports Facebook Page Insights into BigQuery on a schedule, with the option to keep a dated snapshot of every run, so unpaid reach and engagement finally have a history as solid as your ad reporting.

## How to export Facebook Page Insights data to BigQuery

It is a single wizard. Sign in to Facebook, choose the report you want, and say which dataset it belongs in. The fields on each screen change depending on the report type, so you only ever see what applies.

### Step 1. Basic settings

- **Pages** — choose the Pages this flow covers. Select several and Coupler.io loads them into one table with the Page named on every row, which turns a portfolio review into a single flow rather than one per brand.
- **Report type** — the decision that shapes the rest of the wizard. It sets whether you get Page-level trends, follower snapshots, posts, reels, or video performance, and which later fields appear. All six are described below.

### Step 2. Report period

- **Start date** — the opening of your window for the Page-level reports. Relative options such as *days ago*, *this month*, *last month*, *this year*, and *last year* follow the calendar on their own, so you set this once and leave it. For a longer fixed run-up, a macro like `{{365daysago}}` covers it.
- **End date** — the closing of the window. Leave it relative and every run brings in the newest day available.
- **Split data by period** — the grain of the Page reports. Day-level rows if a trend chart is anywhere in your future, or a coarser split when you only report monthly.

### Step 3. Metrics

- **Metrics** — pick the figures the report carries. For post reporting the defaults already cover views, unique viewers, clicks, likes, comments, shares, reactions, and video views, grouped so you can see what is performance, what is engagement, and what is video.

### Step 4. Metrics and dimensions

- **Metrics and dimensions** — for the reports where a metric comes paired with a breakdown, choose the combination you want. Some report types take a list of pairs, others a single pair, and the wizard shows whichever applies to your selection.

### Step 5. Advanced filters

- **Created after** — for video performance, the earliest video publish date to pull. Keeps a back catalogue of old uploads out of a report about recent work.
- **Created before** — the upper bound of that publish window.

### Step 6. Advanced settings

- **Post columns** — decide which post attributes travel with the metrics, such as the message text and attachments. Leave it empty and everything comes through; the post identifier and update time always do.
- **Data behavior** — the setting worth thinking about. Keep only current values for a snapshot of today, or switch to tracking changes over time and Coupler.io writes a dated record on each run. That is how lifetime post figures and follower counts turn into a genuine time series instead of a number that silently overwrites itself. Records are stored one per day: if the importer runs more than once on the same date, the latest run replaces the earlier one (run at 9 AM and again at 8 PM, and the 8 PM figures win), and only runs on different days add new records.

### Step 7. Blend and transform data

- **Column management** — drop the columns nobody opens, rename raw API fields into plain labels, reorder them, and set the data types correctly.
- **Join** — link Page Insights to another source on a shared key, so organic results arrive attached to whatever you judge them by.
- **Append** — stack Instagram Insights, LinkedIn, TikTok, or YouTube rows beneath your Facebook Page data for one organic social table.
- **Aggregate** — summarise daily rows into the weekly or monthly grain your reporting uses.
- **Custom SQL** — write your own query and Coupler.io loads what it returns.

### Step 8. BigQuery destination

- **Data location** — the region in which a new dataset is created. Blank gives you US; name a region when compliance dictates it. An existing dataset keeps the region it was created in regardless of what goes here.
- **Dataset name** — an existing dataset or a new name, which Coupler.io creates for you.
- **Table name** — the same for the table, which appears on the first run.

Three fields and you are done. Coupler.io determines the table schema from the data, so there is nothing to map and no types to set.

### Step 9. Schedule

Choose the interval: as often as every 15 minutes, or hourly, daily, weekly, or monthly. Then decide whether each run replaces the table or appends to it. Append is what you want when the point is to build history. Meta's rate limits and retries are handled on the Coupler.io side, so there is no job of yours to watch.

### Bonus: or just ask the AI Agent to build it for you

None of that appeals? You can skip it. The Coupler.io AI Agent is part of the app. Open it and describe the report in your own words:

> *"Pull daily organic reach, views, and engagement for my Facebook Page for the last 12 months, plus lifetime performance for every post, and load it into BigQuery."*

It knows what Facebook Page Insights exposes, so it chooses the report type, sets the metrics, picks the period and the split, decides whether snapshots are needed, and configures the destination. Where there is a real decision it asks. Where there is not, it finishes the job.

Four things remain yours:

1. **Connect Facebook Page Insights** — the Facebook sign-in stays with you.
2. **Connect BigQuery** — and so does your Google Cloud access.
3. **Pick the Pages** — you decide what is in scope.
4. **Name the dataset and table** — you choose where it lands in the warehouse.

That is the manual part in its entirety. If you are blending several sources into one table you will finish that step in the wizard yourself.

## What data can you export from Facebook Page Insights to BigQuery with Coupler.io?

Six report types are available, split between Page-level reporting and content-level reporting. A flow handles one report, so most teams run several together and let Coupler.io's **Join** transformation combine them before load, so BigQuery receives one analysis-ready table. If you would rather model in the warehouse, load each report raw and build on it with dbt.

- **Page: general overview** — the current standing of the Page. Core properties and headline figures, which makes it the natural dimension table for everything else in your social reporting.
- **Page: performance insights** — the trend report. Organic reach, views, and engagement over whatever window you define, at the grain you choose. This is what a Page-level dashboard is built on.
- **Page: followers daily snapshot** — the follower count captured day by day. Scheduled with append, it becomes the long-run audience curve Business Suite never keeps, and it lines growth up against the weeks you actually published.
- **Post: posts lifetime performance** — one row per post with its accumulated results: views, unique viewers, clicks, likes, comments, shares, reactions, and video views, with message text and attachments alongside if you want them. The table that tells you which posts earned their slot. This report can also collect historical data: turn on tracking changes over time in Data behavior and each run adds a dated row per post, so you see how the numbers grew instead of only the latest lifetime values.
- **Post: reels lifetime performance** — the same treatment for reels, which behave nothing like standard posts and deserve to be judged on their own terms before anyone decides how much of the calendar they get. Like the posts report, it can also collect historical data: with tracking changes over time turned on, each run adds a dated row per reel instead of overwriting the lifetime values.
- **Post: video daily performance** — video results day by day rather than as a lifetime total, so you can see how long a video keeps working after publication instead of only what it ended up with.

On the BigQuery side you name the dataset and the table and choose the region a new dataset is created in, which matters when data residency is a rule rather than a preference. Coupler.io creates whatever is not there yet and reads the table schema straight from your data.

## Why export Facebook Page Insights to BigQuery — real life cases

Here is what teams actually do with the data once it lands in BigQuery.

- **Stop assembling the organic social report by hand**

  The plain, frequent case. Every month somebody needs organic reach, engagement, follower change, and a top-posts list, and the current process is screenshots from Business Suite pasted into a deck. Coupler.io refreshes the Page and post reports into BigQuery on your schedule, and Looker Studio, Power BI, or Tableau read them live. The report becomes a link that is already up to date, and the numbers stop shifting between the draft and the meeting.

- **Keep a follower history that does not reset**

  Facebook shows recent follower movement and throws the rest away. Schedule the daily snapshot report with append and BigQuery quietly accumulates years of audience data. When somebody asks whether the rebrand cost you followers or the partnership gained you any, the answer is a query against dated rows rather than a hunt for an old screenshot.

- **Judge reels and posts on comparable terms**

  Reels get pushed hard by the algorithm and look wonderful next to a link post, which makes the average misleading. Pulling reels lifetime performance and posts lifetime performance into BigQuery as separate tables and comparing them properly shows what each format contributes to reach and to actual engagement. That is how a content mix gets decided instead of argued about.

- **See how long a video keeps earning**

  Lifetime video totals hide the shape of the curve. Daily video performance in BigQuery shows whether a video does its work in the first 48 hours or keeps accumulating views for weeks, which changes how you schedule, how long you leave something promoted, and which back-catalogue uploads are worth resurfacing.

- **Put organic and paid Facebook results in one query**

  Page performance and Ads Manager live in different tools and are usually reported by different people, so the two never quite reconcile. Coupler.io can load Page Insights and Facebook Ads data into the same BigQuery dataset and join them on the way in, giving you a launch view that covers what the audience found by itself and what you paid to put in front of them. Useful when somebody senior asks what the organic effort is actually worth.

- **Report on every Page you manage from one table**

  Agencies and multi-market teams repeat the same export per Page, every month, forever. Select all the Pages in one flow, let Coupler.io label and stack the rows, and BigQuery holds a single table you can filter per client or read whole for an internal benchmark. Standardising KPI definitions across business units stops being a document and becomes a column.

- **Connect publishing activity to commercial outcomes**

  Reach is not a result. Join Page Insights to your CRM, ecommerce, or web analytics data and BigQuery can line up publishing days and post-level clicks against sessions, signups, and orders. It will not produce tidy attribution, but it does produce a defensible view of what the Page contributes, which is the question sitting behind every organic social budget review.

- **Archive the detail cheaply and model it later**

  Meta's organic reporting thins out the further back you look, and the detailed post-level figures are the first to go. A scheduled append-only flow turns BigQuery into a full-granularity archive in storage that costs cents per gigabyte, which is exactly where dbt models and scheduled queries belong. Year-over-year seasonality and multi-year content experiments become answerable without anyone having planned for the question in advance.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
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
