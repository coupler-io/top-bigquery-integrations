---
title: Connect TikTok Organic to BigQuery with Coupler.io
slug: connect-tiktok-organic-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md
source: TikTok Organic
destination: BigQuery
meta_title: Connect TikTok Organic to BigQuery with Coupler.io
meta_description: Load TikTok profile insights, audience demographics, hourly activity, and video performance into BigQuery on a schedule, and keep the history past TikTok's 60-day window.
---

![TikTok Organic](https://static.coupler.io/importers/tiktok.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect TikTok Organic to BigQuery with Coupler.io

TikTok's creator analytics tab has one serious flaw for anyone doing real reporting: it forgets. The platform will not serve data older than 60 days, so last quarter's launch, last year's best-performing video, and any sense of how the account has grown are simply gone unless somebody was screenshotting. Coupler.io pulls profile and video analytics into BigQuery on a schedule, appending as it goes, which turns a rolling two-month window into a permanent record you own.

## How to export TikTok Organic data to BigQuery

Connect your TikTok account once and the wizard is short — there are only two screens of source settings. No code, no developer, and a flow is typically running within a few minutes. Here is every field and what to do with it.

### Step 1. Basic settings

- **Report type** — the only real decision on this screen, and it determines the shape of the table. Pick whether you want profile-level insights, audience breakdowns, or video performance. All six are described further down the page.

### Step 2. Report period

- **Start date** — where the window opens. Pick a relative option like *days ago*, *this month*, or *last month* and it keeps rolling forward on its own, so you set it once and leave it. Bear in mind TikTok will not return anything older than 60 days, which is exactly why the BigQuery archive below matters.
- **End date** — where the window closes. Keep it relative and every scheduled run comes back current without anyone editing the flow.

### Step 3. Blend and transform data

- **Column management** — drop the fields nobody looks at, rename raw API keys into labels a human reads without flinching, reorder them, and set the right data types.
- **Join** — match organic results against another source on a shared key, so content performance and business outcomes land together.
- **Append** — stack Instagram, YouTube, Facebook, or LinkedIn rows under your TikTok rows for one social performance table with consistent columns.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting runs on.
- **Custom SQL** — for whatever the options above do not cover. Write the query and Coupler.io loads the result.

### Step 4. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it empty for US, or set it when your compliance team has an opinion about where data sits. Point at a dataset that already exists and it keeps its original region whatever you enter here.
- **Dataset name** — type an existing dataset name or invent a new one and Coupler.io creates it.
- **Table name** — the same. Name the table and it appears on the first run.

Nothing else to configure. Coupler.io derives the table structure from your data, so there is no column mapping and no manual type selection.

### Step 5. Schedule

Pick a refresh interval — as frequent as every 15 minutes, or hourly, daily, weekly, monthly — and choose whether each run replaces the table or appends to it. For TikTok Organic the append option is the interesting one: it is what builds the history the platform will not keep. After that the flow runs itself, and TikTok's rate limits and token refreshes stop being anything you think about.

### Bonus: or just ask the AI Agent to build it for you

Not in the mood to fill any of that in? You do not have to. The Coupler.io AI Agent lives inside the app — open it and describe what you want in your own words:

> *"Track our TikTok profile performance daily — views, followers, and engagement — plus video-level insights, and load it into BigQuery."*

It knows what TikTok Organic exposes, so it selects the report type, sets the period, and configures the destination. Where there is a real decision it asks. Where there is not, it carries on.

You only deal with four things:

1. **Connect TikTok Organic** — authorizing TikTok stays with you.
2. **Connect BigQuery** — so does your Google Cloud access.
3. **Pick the profile** — you decide which account is in scope.
4. **Name the dataset and table** — you say where in the warehouse it lands.

That is the whole manual part. If you are blending several sources into one table you will finish that in the wizard; everything else the Agent takes care of.

## What data can you export from TikTok Organic to BigQuery with Coupler.io?

Six report types are available. One flow carries one report type, so most teams run a few at once: a profile trend table, an audience table, and a video table. Coupler.io's built-in **Join** transformation combines them before they land, so BigQuery receives one ready-to-query table and nobody writes the join — or load each one separately and model it warehouse-side if that suits your stack better.

- **Profile insights** — the health of the account over time: views, profile activity, follower movement, and engagement, day by day. This is the table your weekly social report is built on, and it is the one worth archiving from day one.
- **Profile audience genders** — the gender composition of your audience across the chosen period, which is the fastest way to check whether the people watching resemble the people you are trying to reach.
- **Profile audience countries** — where your audience actually lives. Useful when deciding which markets deserve localized content, and sobering when a market you have never invested in turns out to be half your reach.
- **Profile audience hourly activity** — when your followers are online, broken down by hour. Feed this into a scheduling decision and posting time stops being folklore.
- **Video list insights** — performance per video: how each one did rather than how the account did. The basis of a content library you can sort, filter, and learn from, instead of a feed you scroll.
- **Video list top countries** — the geographic split for your videos, which shows when a single clip has travelled somewhere unexpected and started building an audience you were not planning for.

On the BigQuery side you name the dataset and table and choose the region a new dataset is created in, which matters as soon as data residency is a requirement rather than a preference. Coupler.io creates anything that is not there yet and defines the table structure itself, taking the types from your data.

## Why export TikTok Organic to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Put your TikTok reporting on autopilot**

  The everyday reason, and the least glamorous: the social report is due, and nobody wants to copy numbers out of the app again. Coupler.io refreshes views, followers, and engagement into BigQuery on whatever schedule you set, and Looker Studio, Power BI, or Tableau read straight from there. One dashboard, one set of figures, available to the people who need the numbers but have no reason to ever open TikTok's analytics tab.

- **Keep the history TikTok deletes after 60 days**

  This is the reason most teams set the flow up in the first place. The platform serves a rolling two-month window and nothing behind it, which makes year-over-year comparison, campaign retrospectives, and growth narratives impossible to assemble after the fact. A scheduled flow appending into BigQuery turns that window into a permanent archive: every day of profile performance and every video, retained at full detail in storage that costs cents per gigabyte. Set it up today and in twelve months you will have something nobody else on the team can produce.

- **Judge organic and paid TikTok against each other**

  Organic reach and paid reach are usually reported by different people in different decks, which hides the obvious question: is the content that performs organically also the content worth putting money behind. Coupler.io can append TikTok Ads results alongside organic video insights in one BigQuery table with consistent column names, so the comparison is a filter rather than a project, and promoting a proven video stops being a hunch.

- **Post when your audience is actually there**

  Hourly activity data is interesting once and useful permanently, but only if somebody keeps it. Loading it into BigQuery and layering it over your own posting log shows the relationship between publish time and performance for your account, not for social media in general. Scheduling turns into a decision based on evidence, and it stays current as the audience shifts.

- **Check the audience against who you meant to reach**

  Country and gender breakdowns sitting next to your customer data in BigQuery answer a question the app cannot: whether the audience you have built resembles the audience that buys from you. When it does not, you find out while there is still time to change the content, rather than at the end of a quarter when the follower count looked fine and nothing converted.

- **Build a content library that outlives the feed**

  Video-level insights accumulated over months become a searchable record of what your account has published and how each piece did. Sort it, filter it by format or topic, and the recurring patterns show up on their own. New team members get an honest history of what works here instead of a verbal briefing, and nobody relaunches an idea that already failed twice.

- **Report across brands and platforms in one place**

  Agencies and multi-brand teams normally reconcile several TikTok profiles plus every other network by hand. Land each profile into a shared BigQuery dataset, append the other social sources beside it, and one dashboard template serves every account. Adding the next brand becomes a configuration change rather than another recurring task in somebody's calendar.

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
- [Connect TikTok Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-ads-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect Xero to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
