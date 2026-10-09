---
title: Connect LinkedIn Public Data to BigQuery with Coupler.io
slug: connect-linkedin-public-data-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-linkedin-public-data-to-bigquery.md
source: LinkedIn Company Pages
destination: BigQuery
meta_title: Connect LinkedIn Public Data to BigQuery with Coupler.io
meta_description: Load LinkedIn company page insights, follower and audience demographics, and post performance into BigQuery on a schedule. No manual exports, no lost history.
---

![LinkedIn Company Pages](https://static.coupler.io/importers/linkedin.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect LinkedIn Public Data to BigQuery with Coupler.io

LinkedIn's page admin view gives you a handful of charts, a short export window, and no way to put organic performance next to anything else your company measures. For a channel most B2B teams now treat as their main audience, that is a surprisingly small window. Coupler.io pulls company page analytics — page performance, follower and audience demographics, and post-level results — into BigQuery on a schedule, as tables you keep for as long as you want them.

## How to export LinkedIn Company Pages data to BigQuery

Authorize LinkedIn once, then work through four short source screens. No SQL, no developer, and most flows are live within minutes. Here is each screen and what goes in it.

### Step 1. Basic settings

- **Pages** — choose the company pages you want in this table. Select several and Coupler.io pulls them together with each row labelled, so a multi-brand or agency roll-up is one flow rather than one per page.
- **Report type** — the decision that reshapes the rest of the wizard. Pick whether you want page-level insights, audience breakdowns, or post performance. All nine current types are described further down the page.

### Step 2. Report period

- **Start date** — where the window opens. Choose a relative option such as *days ago*, *this month*, *last month*, *this year*, or *last year* and the range rolls forward on its own, so you set it once and leave it alone. For a longer, more specific stretch, a macro like `{{365daysago}}` covers it.
- **End date** — where the window closes. Keep it relative and every scheduled run returns current data without anyone opening the flow.
- **Split data by period** — the grain of your rows: daily, weekly, monthly, or a single totals line, depending on the report. Daily is the safe choice whenever a trend line might be asked for.

### Step 3. Dimensions

- **Dimension** — the cut your demographics report is grouped by: country, region, industry, company size, job function, seniority, or association type. This is where you find out who your followers and visitors actually are.
- **Breakdown dimension** — the equivalent control on the legacy time-based and lifetime report types, kept for flows built before the current reports existed.

### Step 4. Metrics

- **Metrics** — for video post analytics, choose which figures travel with each post: views, viewers, time viewed, and time viewed for views. Enough to tell a video people started from a video people finished.

### Step 5. Blend and transform data

- **Column management** — hide the fields nobody reads, rename raw API keys into labels a human recognizes, reorder them, and set the right data types.
- **Join** — match page data to another source on a shared key, so organic reach and business outcomes land in one table.
- **Append** — stack LinkedIn Ads, Facebook, Instagram, or TikTok rows under your page rows for a single social performance table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — for anything the options above miss. Write the query and Coupler.io loads the result.

### Step 6. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it empty for US, or set it when your compliance team needs the data somewhere specific. A dataset that already exists keeps its original region no matter what you enter here.
- **Dataset name** — type an existing dataset name or a new one, and Coupler.io creates it.
- **Table name** — the same. Name the table and it appears on the first run.

That is all there is to the destination. Coupler.io derives the table structure from your data, so there is no column mapping and no type selection by hand.

### Step 7. Schedule

Pick a refresh interval — every 15 minutes at the quickest, or hourly, daily, weekly, monthly — and choose whether each run replaces the table or appends to it. Append is the one that quietly builds you a long history of page performance. After that the flow maintains itself, and LinkedIn's rate limits and token refreshes stop being anybody's job.

### Bonus: or just ask the AI Agent to build it for you

Rather not fill any of that in? You do not have to. The Coupler.io AI Agent lives in the app — open it and describe what you need in plain language:

> *"Load our LinkedIn company page performance into BigQuery daily — impressions, engagement, and follower gains — along with follower demographics by industry and seniority."*

It knows what LinkedIn company pages expose, so it chooses the report type, sets the dimension and period, and configures the destination. Where there is a real decision to make it asks you. Where there is not, it simply continues.

Four things stay with you:

1. **Connect LinkedIn** — authorizing the account is yours.
2. **Connect BigQuery** — so is your Google Cloud access.
3. **Pick the pages** — you decide which company pages are in scope, and this one has to be done in the wizard.
4. **Name the dataset and table** — you say where in the warehouse it lands.

That is the whole manual part. Blending several sources into one table is the other thing you will finish in the wizard; the Agent does the rest.

## What data can you export from LinkedIn Company Pages to BigQuery with Coupler.io?

Nine current report types are available, split across page, audience, and post data. A flow carries one report type, so most teams run several at once: a page trend table, a demographics table, and a post table. Coupler.io's built-in **Join** transformation combines them before they land, so BigQuery gets one ready-to-query table and nobody writes the join — or load each one as its own raw table and model it warehouse-side instead.

- **Page: performance insights** — how the page itself performed over the period: impressions, engagement, clicks, and visitor activity, split daily, weekly, or monthly. The backbone of any LinkedIn organic dashboard.
- **Page: lifetime overview** — the all-time position of the page in one row, which is what you want when the question is where we stand rather than what happened last week.
- **Page: cumulative posts performance** — the combined results of everything the page has published across the chosen window, useful for judging output as a body of work rather than post by post.
- **Page: follower gains trend** — follower movement over time, broken out so growth spurts line up with the content and campaigns that caused them. The honest alternative to a follower count screenshot.
- **Page: follower demographics insights** — who your followers are, grouped by country, geographic market, industry, job function, seniority, company size, or association type. The closest thing to seeing whether your audience matches your target market.
- **Page: audience demographics insights** — the same style of breakdown for the wider audience reaching your page, so you can tell the people who follow you from the people who merely find you.
- **Post: individual posts lifetime performance** — results for each post across its whole life, which matters on LinkedIn where good posts keep collecting impressions long after the week they went out.
- **Post: video performance** — views, viewers, and time viewed per video post, split by period, so you can separate a thumbnail that attracts clicks from a video that holds attention.
- **Post: social reactions & comments count** — reactions and comments per post, the engagement signal LinkedIn's distribution actually responds to.

A handful of older report types are still listed in the wizard and marked as deprecated, including the earlier time-based, lifetime, shares, and UGC post reports. Existing flows keep working, but anything new is better built on the nine above.

On the BigQuery side you name the dataset and table and pick the region a new dataset is created in, which matters the moment data residency becomes a requirement. Coupler.io creates whatever is not there yet and defines the table structure for you, reading the types directly from your data.

## Why export LinkedIn Company Pages to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Put your LinkedIn page reporting on autopilot**

  The everyday reason: the weekly marketing report needs organic numbers and nobody wants to open page analytics and copy them across again. Coupler.io refreshes impressions, engagement, clicks, and follower movement into BigQuery on whatever schedule you set, and Looker Studio, Power BI, or Tableau read straight from there. Everyone reads the same dashboard, including the people who need the figures and will never be page admins.

- **Report organic and paid LinkedIn as one channel**

  Page performance and Campaign Manager results usually live in separate decks, which makes the obvious question hard to ask: does the content that works organically also work when you put money behind it. Coupler.io can append LinkedIn Ads results alongside page and post data in one BigQuery table with consistent columns, so promoting a proven post becomes a decision supported by evidence rather than a feeling about which one did well.

- **Check your followers against your ideal customer profile**

  Follower and audience demographics broken out by industry, seniority, job function, company size, and geography are only half the story until they sit next to your actual customer list. In BigQuery they do, and the comparison is direct: whether the audience you have built resembles the buyers you want, and which segments you are reaching in volume but never converting. That is a conversation worth having with sales, not just a chart for the marketing review.

- **Keep a content library instead of a feed**

  Post-level lifetime performance accumulated over months becomes a searchable record of everything the page has published and how each piece did. Sort by format, topic, or author and the patterns show up without anyone having to remember them. On LinkedIn, where posts keep earning impressions for weeks, lifetime figures also correct the usual mistake of judging a post by its first 48 hours.

- **Tell a video people started from a video people watched**

  View counts flatter everything. Pairing views with viewers and time viewed in BigQuery shows which videos actually held attention and which collected an autoplay and nothing more. Over a quarter that is enough to change how the next batch gets briefed, and it is the kind of comparison the native view makes awkward on purpose.

- **Keep the history LinkedIn does not hand back**

  Page analytics are built for a recent window, not for a multi-year record, and detail fades the further back you ask. A scheduled flow appending into BigQuery turns every refresh into another layer of archive: years of daily page performance, follower composition, and post results at full granularity, in storage that costs cents per gigabyte. Year-over-year growth, the effect of last year's rebrand, and board questions about long-term reach all become queries instead of recollections.

- **Run every brand and client from one dataset**

  Agencies and multi-brand teams normally repeat the same export for each page and reconcile the results by hand. One flow can carry several pages at once, labelled, into a shared BigQuery dataset feeding one dashboard template filtered per client. Taking on the next brand becomes a tick box rather than another recurring task.

- **Put social reach in the same table as the rest of marketing**

  LinkedIn is one surface among several, and leadership tends to ask about all of them together. Append Facebook, Instagram, and TikTok rows beneath your LinkedIn page data with matching column names and BigQuery holds a single organic performance table. Share of attention, consistency of output, and which network is genuinely earning the team's time stop being questions you answer by opening four tabs.

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
- [Connect Microsoft Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-microsoft-ads-to-bigquery.md)
- [Connect QuickBooks to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-quickbooks-to-bigquery.md)
- [Connect Shopify to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-shopify-to-bigquery.md)
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
