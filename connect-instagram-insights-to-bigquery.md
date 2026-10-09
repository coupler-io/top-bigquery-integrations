---
title: Connect Instagram Insights to BigQuery with Coupler.io
slug: connect-instagram-insights-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-insights-to-bigquery.md
source: Instagram Insights
destination: BigQuery
meta_title: Connect Instagram Insights to BigQuery with Coupler.io
meta_description: Load organic Instagram profile, post, story, and follower demography reports into BigQuery on a schedule, and keep the metrics the app only shows you for a few days.
---

![Instagram Insights](https://static.coupler.io/importers/instagram-ads.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Instagram Insights to BigQuery with Coupler.io

Organic Instagram reporting has a shelf life. Story metrics are gone within a day, the in-app graphs stop at a handful of preset windows, and nothing in the professional dashboard lets you ask how this month's reach compares with the same month last year. Coupler.io pulls your Instagram Insights into BigQuery on a schedule, one report type per flow, and keeps every snapshot, so the numbers the app forgets become history you own.

## How to export Instagram Insights data to BigQuery

Setup is one short wizard: sign in to Instagram, choose a report, point it at a dataset. Nothing here needs code or a developer, and the fields you see adapt to the report you pick.

### Step 1. Basic settings

- **Profiles** — pick the professional accounts you want covered. Select several and Coupler.io brings them into the same table with the profile named on each row, which is what you want when one team looks after a family of brand accounts.
- **Report type** — the choice everything else follows from. It decides whether you get profile-level trends, individual posts, stories, or audience demography, and which fields appear in the steps below. All sixteen are described further down.

### Step 2. Report period

- **Start date** — the first day of the window for the profile trend reports. Relative options such as *days ago*, *this month*, *last month*, *this year*, and *last year* roll along with the calendar, so you configure it once. For a longer fixed stretch, a macro like `{{365daysago}}` works too.
- **End date** — the last day. Keep it relative and every run reaches the newest data Instagram has published.
- **Timeframe** — for the new audience demography report, choose whether the breakdown covers this week or this month, which is as far back as Instagram's own aggregation goes.
- **Published after** — for post-level reports, the earliest publish date you care about. Handy for isolating a campaign window or a single content push.
- **Published before** — the other end of that publishing window.
- **Split by** — how finely the profile reports are cut up. Totals only for one summary row, daily when you need a trend line, or monthly for a tidy board-level series.

### Step 3. Metrics and dimensions

- **Metrics** — for demography reporting, decide whether you want the make-up of the people you reached, the people who engaged, or both.
- **Dimension** — the attribute the audience is grouped by. Age, gender, both together, city, or country.

### Step 4. Advanced settings

- **Show promoted posts ad ids in separate rows** — keep the ad IDs behind boosted posts on their own rows, so organic and promoted performance stay distinguishable instead of silently merged.
- **Data behavior** — the setting that quietly matters most. Leave it on latest values for a current-state table, or switch to tracking changes over time and Coupler.io writes a dated snapshot on every run, which is the only way to build real trend history for metrics Instagram only ever reports as a total. Snapshots are stored one per day: if the importer runs more than once on the same date, the latest run replaces the earlier one (run at 9 AM and again at 8 PM, and the 8 PM figures win), and only runs on different days add new records.

### Step 5. Blend and transform data

- **Column management** — hide fields nobody reads, give cryptic API names labels a human understands, reorder them, and set correct data types.
- **Join** — connect Instagram Insights to another source on a shared key, so organic performance arrives next to whatever you measure it against.
- **Append** — stack Facebook Page Insights, TikTok, LinkedIn, or YouTube rows under your Instagram data for one organic social table.
- **Aggregate** — roll daily rows into the weekly or monthly grain your reporting cadence uses.
- **Custom SQL** — write the query yourself and Coupler.io loads the result.

### Step 6. BigQuery destination

- **Data location** — the region a new dataset is created in. Blank means US; name a region when your policy requires it. A dataset that already exists stays in its original region no matter what you enter here.
- **Dataset name** — type an existing dataset or invent one, and Coupler.io creates it.
- **Table name** — the same for the table, which shows up on the first run.

That is all three fields. Coupler.io works out the table schema from your data, so no column mapping and no manual types.

### Step 7. Schedule

Set the cadence: every 15 minutes at the fastest, or hourly, daily, weekly, or monthly. Then choose whether each run replaces the table or appends to it. Append is the one to use for story reports, since Instagram only exposes those metrics for 24 hours and an appending flow is what turns them into a permanent record. Rate limits and retries are handled for you.

### Bonus: or just ask the AI Agent to build it for you

If filling in forms is not how you want to spend the afternoon, skip it. The Coupler.io AI Agent is built into the app. Describe the report you need in ordinary words:

> *"Track my Instagram profile reach, follower growth, and post engagement daily for the past year, keep a snapshot of each run, and load it into BigQuery."*

It knows the full Instagram Insights catalogue, so it selects the report type, sets the period and the grain, chooses the right metrics and dimension, decides whether snapshots are needed, and sets up the destination. Where a genuine decision is required it asks you. Where it is not, it carries on.

You handle four things:

1. **Connect Instagram Insights** — the Instagram sign-in stays with you.
2. **Connect BigQuery** — as does your Google Cloud access.
3. **Pick the profiles** — you decide which accounts are in scope.
4. **Name the dataset and table** — you choose where it lands.

That is the whole manual part. Combining several sources into one table is finished off in the wizard; everything else the Agent does.

## What data can you export from Instagram Insights to BigQuery with Coupler.io?

Sixteen report types are available, grouped around profiles, posts, and stories. One flow produces one report, so most teams run a handful in parallel and let Coupler.io's **Join** transformation stitch them together before load, so BigQuery receives a single ready-to-query table. The warehouse-led alternative is just as valid: land each report as its own raw table and model them with dbt.

- **Post: performance totals** — lifetime numbers for every published post, so you can rank formats, captions, and publishing times on evidence instead of instinct.
- **Profile: account overview** — the current state of the account. Follower count, profile details, and headline figures, ideal as the dimension table the rest of your social reporting hangs off.
- **Profile: performance insights** — the core trend report. Reach, views, interactions, and profile activity over the window you choose, as totals, daily rows, or monthly rows.
- **Profile: followers daily breakdown (last 30 days)** — day-by-day follower movement for the last month. Scheduled with append, it quietly becomes the multi-year growth curve Instagram never gives you.
- **Profile: followers demography insights** — who your existing followers actually are, grouped by age, gender, city, or country. The table to reach for when someone questions whether the audience matches the target market.
- **Post: comment details review** — the comments themselves, attached to the posts that attracted them. Useful for sentiment work, moderation tracking, and spotting the questions your content keeps provoking.
- **Post: profile interactions breakdown** — what people did after a post landed: profile visits, follows, and other follow-up actions. This is the report that separates content that gets liked from content that wins audience.
- **Story: performance totals (last 24 hours)** — the headline figures for stories while they are still live. Instagram discards them shortly after, which makes a scheduled export the only way to keep them.
- **Story: navigation by action type (last 24 hours)** — taps forward, taps back, exits, and replies. The clearest signal available of exactly which frame people lose patience with.
- **Story: profile interactions breakdown (last 24 hours)** — the profile activity a story triggered, so ephemeral content gets measured on outcomes rather than impressions.
- **Profile: daily reach** — one reach figure per day, clean enough to chart directly and to correlate against publishing activity or paid bursts.
- **Profile: performance by media type** — results split across the formats you publish, which answers the perennial reels-versus-carousels-versus-static argument with your own data.
- **Profile: performance by follow type** — performance divided between followers and non-followers, the single best measure of whether your content is reaching anyone new.
- **Profile: links taps by contact button type** — how often people used the contact and link buttons on your profile. The nearest thing Instagram has to a conversion event for the account itself.
- **Profile: reach by media product type and follow type** — format and audience type crossed together, so you can see which format actually breaks out beyond your existing followers.
- **Profile: new audience demography insights** — the make-up of the people you reached or engaged recently, by age, gender, city, or country. The report that tells you whether your audience is drifting.

On the BigQuery side you name the dataset and table and set the region a new dataset is created in, which matters when data residency is prescribed rather than chosen. Anything missing gets created, and Coupler.io derives the table schema from the data itself.

## Why export Instagram Insights to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

What follows is what social teams genuinely use this for. Both routes work; the no-code one is live this afternoon.

- **Have the monthly social report waiting for you**

  The everyday reason. Somebody has to produce reach, follower change, and top-post engagement every month, and screenshotting the app is nobody's idea of a career. Coupler.io refreshes the profile and post reports into BigQuery on your schedule, Looker Studio or Power BI reads them straight from there, and the deck becomes a link that is already correct. The content team gets its numbers without asking, and the figures stop changing between the draft and the review.

- **Keep story metrics that Instagram deletes**

  Stories are measured for 24 hours and then effectively gone. A flow on an hourly or daily schedule, set to append, captures performance totals, navigation by action type, and profile interactions before the window closes. After a few weeks BigQuery can tell you which story structures hold attention to the last frame, which is a question the app itself can never answer retrospectively.

- **Build the follower growth curve the app will not show you**

  Instagram shows recent follower movement and little else. Turn on change tracking, schedule it daily, and the snapshots accumulate into years of follower history in BigQuery. Growth inflections line up against the weeks you published differently, and the honest answer to whether a campaign actually grew the audience stops being a matter of memory.

- **Settle the format argument with your own data**

  Opinions about reels are plentiful and evidence is not. The performance by media type and reach by follow type reports put format, reach, and audience type in the same BigQuery table, so you can see which formats travel beyond your existing followers rather than recirculating among them. That is a content calendar decision made on data instead of on whatever the industry is currently claiming.

- **Compare organic and paid Instagram side by side**

  Organic reach and ad reach are usually reported by different people in different tools. Coupler.io can load Instagram Insights next to your Instagram Ads data and join them before they reach BigQuery, with boosted post ad IDs kept on their own rows so the two are never confused. You get the full picture of a launch: what the audience found on its own, and what you had to pay to put in front of them.

- **Report on a whole portfolio of accounts at once**

  Agencies and multi-brand teams repeat the same export across every account they manage. Select all the profiles in one flow, let Coupler.io label and stack them, and BigQuery holds one table that filters down per client or reads whole for an internal benchmark. Adding an account is one more checkbox rather than another pipeline to maintain.

- **Connect content effort to commercial results**

  Nobody buys because of an impression count. Join Instagram Insights to your CRM, ecommerce, or web analytics data and BigQuery can line up publishing activity, profile link taps, and profile visits against sessions, signups, and orders. It will not hand you clean attribution, but it does give you a defensible view of what organic social contributes, which is usually the question behind the budget conversation.

- **Keep an audit trail of what the audience said**

  Comment data is treated as something to reply to and then forget. Load it into BigQuery on a schedule and it becomes a searchable record: recurring complaints, repeated product questions, the specific posts that set off a spike. Support and product get a feedback source they can query, and the community team can prove a pattern rather than describe one.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
- [Connect Facebook Page Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md)
- [Connect Google Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-ads-to-bigquery.md)
- [Connect Google Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-analytics-to-bigquery.md)
- [Connect Google Search Console to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-search-console-to-bigquery.md)
- [Connect Instagram Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-instagram-ads-to-bigquery.md)
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
