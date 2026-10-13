---
title: Connect Google Analytics to BigQuery with Coupler.io
slug: connect-google-analytics-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-analytics-to-bigquery.md
source: Google Analytics 4 (GA4)
destination: BigQuery
meta_title: Connect Google Analytics to BigQuery with Coupler.io
meta_description: Load GA4 sessions, events, ecommerce, acquisition, and audience reports into BigQuery on a schedule. Pick your own metrics and dimensions, no native export setup needed.
---

![Google Analytics](https://static.coupler.io/importers/google-analytics.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Google Analytics to BigQuery with Coupler.io

GA4's explorations sample, its reports cap how much you can see at once, and comparing more than a couple of properties means opening more than a couple of tabs. The native BigQuery link exists, but it gives you raw event tables that need modelling before anyone can read them, and it only starts from the day you switch it on. Coupler.io takes the GA4 reports you already understand — your metrics, your dimensions, your date range — and lands them in BigQuery as flat tables on a schedule, ready to query the moment they arrive.

## How to export Google Analytics data to BigQuery

Sign in to Google once, choose what you want in the table, and point it at a dataset. No tagging changes, no export configuration in the GA4 admin, no waiting a day to see whether it worked.

### Step 1. Basic settings

- **Properties** — choose the GA4 properties this flow covers. Select several and Coupler.io brings them into one table, labelled, so a multi-brand or multi-region view takes one flow rather than one per site.

### Step 2. Report period

- **Start date** — where your history begins. Use a relative option such as *days ago*, *this month*, *last month*, *this year*, or *last year* and the window rolls forward on its own. If you want something more precise, like a full trailing year, a macro such as `{{365daysago}}` covers it.
- **End date** — where the window closes. Keep it relative and every scheduled run stays current with nobody editing anything.

### Step 3. Metrics and dimensions

- **Metrics** — the numbers in your table. Nearly ninety of them, from sessions, views, and active users through engagement rate and average session duration to purchase revenue, ROAS, and cost per key event. Take the handful your team reports on and leave the rest.
- **Dimensions** — how those numbers get cut: up to nine per report, from a list of several hundred covering date and hour, source and medium, channel group, campaign, landing page, event name, device, geography, demographics, item attributes, and the linked Google Ads, SA360, DV360, and CM360 fields. The combination you choose is the table you get, so you can build the report GA4 will not let you build in its own interface.

### Step 4. Blend and transform data

- **Column management** — hide the fields nobody uses, rename GA4's API names into labels a human recognises, reorder them, and set the right types.
- **Join** — match GA4 to another source on a shared key, so sessions and the revenue they produced land in one row.
- **Append** — stack several properties or several reporting periods into one continuous table.
- **Aggregate** — roll daily rows up to the weekly or monthly grain your reporting model expects.
- **Custom SQL** — for whatever the transformations above do not reach. Write the query and Coupler.io loads the result.

### Step 5. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it empty for US, or set it where residency rules apply. If the dataset already exists, it stays in the region it was created in regardless of what you enter.
- **Dataset name** — an existing dataset, or type a new name and Coupler.io creates it.
- **Table name** — same again. Name it and the table appears on the first run.

Nothing more to configure. Coupler.io derives the table schema from your data, so there are no columns to map and no types to declare.

### Step 6. Schedule

Pick the refresh interval — every 15 minutes at the fastest, or hourly, daily, weekly, monthly — and decide whether each run replaces the table or appends to it. From then on it runs itself, and GA4's API quotas and retries are Coupler.io's problem rather than something on your roadmap.

### Bonus: or just ask the AI Agent to build it for you

If choosing from several hundred dimensions sounds like a long afternoon, hand it over. The Coupler.io AI Agent is built into the app: open it and describe the report you want.

> *"Build me a daily GA4 traffic and conversion report for the last two years — sessions, engaged sessions, total users, key events, and purchase revenue by default channel group, landing page, and device — and load it into BigQuery."*

It knows every metric and dimension GA4 exposes and which of them can legally sit in the same report, so it assembles the field list, sets the period, and configures the destination. Where there is a real choice it asks you. Where there is not, it just decides.

You handle four things:

1. **Connect Google Analytics** — signing in to Google stays with you.
2. **Connect BigQuery** — so does your Google Cloud access.
3. **Pick the properties** — you decide which sites and apps are in scope.
4. **Name the dataset and table** — you choose where in the warehouse it lands.

That is the whole manual part. If you are blending several sources into one table you will finish that in the wizard, and the Agent takes care of the rest.

## What data can you export from Google Analytics to BigQuery with Coupler.io?

GA4 does not hand you a list of fixed report types. Instead you compose the report yourself from roughly ninety metrics and several hundred dimensions, which means the data categories below are the ones you choose to build rather than ones you pick from a menu. Each is a real combination of fields in the connector, and one flow carries one combination, so most teams run a few in parallel. Coupler.io's built-in **Join** transformation stitches them together before they land, so BigQuery receives one ready-to-query table — or load each report separately and model it in the warehouse instead.

- **Traffic acquisition** — where visitors come from, using *Session source / medium*, *Session campaign*, *Session default channel group*, and *Session primary channel group* against sessions, engaged sessions, and engagement rate. The report every channel conversation starts with.
- **User acquisition** — the first-touch view, built from the *First user* family of dimensions, so you can tell what originally introduced someone from what happened to bring them back.
- **Engagement and sessions** — sessions, sessions per user, average session duration, engaged sessions, engagement rate, user engagement, and views per session. The honest measure of whether traffic is doing anything once it arrives.
- **Pages and screens** — *Page path*, *Page title*, *Landing page*, *Full page URL*, *Hostname*, and *Content group* with views and key events attached, which is how content performance becomes a ranked list rather than an impression.
- **Events and key events** — *Event name*, *Is key event*, event count, event count per user, events per session, event value, and the session and user key event rates. Your conversion funnel at whatever definition your business uses.
- **Ecommerce and revenue** — purchases, transactions, add-to-carts, checkouts, purchase revenue, total revenue, refund amount, shipping and tax, average purchase revenue per user, and the cart-to-view and purchase-to-view rates. Everything finance will ask about.
- **Product and item detail** — *Item name*, *Item ID*, *Item brand*, *Item category*, and *Item variant* with item views, items added to cart, item purchase quantity, and item revenue, so catalogue performance comes out at the row level.
- **Promotions and item lists** — *Item promotion name*, *Item list name*, and *Item promotion creative name* with their views, clicks, and click-through rates. Merchandising placement, measured.
- **Audience demographics and interests** — *Age*, *Gender*, *Interests*, *Audience name*, and *New / returning*, so you can describe who converts rather than guess.
- **Geography** — *Country*, *Region*, *City*, and *Continent*, down to the grain your market reporting uses.
- **Technology and platform** — *Device category*, *Device brand*, *Mobile model*, *Browser*, *Operating system*, *OS version*, *Screen resolution*, and *Platform*, plus crash-affected users and crash-free users rate for app properties.
- **Paid media linkage** — the *Google Ads*, *SA360*, *DV360*, and *CM360* dimension families alongside ads clicks, ads cost, ads impressions, ads cost per click, and return on ad spend, which is how platform-reported spend meets site-side behaviour in one table.
- **Organic search performance** — organic Google search clicks, impressions, click-through rate, and average position for properties with Search Console linked, next to the behaviour those clicks produced.
- **Publisher and ad revenue** — publisher ad clicks, publisher ad impressions, ad unit exposure, and total ad revenue, with *Ad format*, *Ad source*, and *Ad unit*, for anyone monetising inventory rather than selling product.
- **Retention and cohorts** — *Cohort*, *Daily cohort*, *Weekly cohort*, and *Monthly cohort* with active users, the 1-, 7-, and 28-day active user counts, and the DAU/MAU, DAU/WAU, and WAU/MAU ratios. Stickiness, in a table you can chart.
- **Time grain** — *Date*, *Date + hour (YYYYMMDDHH)*, *Hour*, *Minute*, *Day of week*, *Week*, *Month*, *Year*, and the ISO week and year variants, so a report built for daily trending can be rebuilt for intraday or annual comparison without changing anything else.

On the BigQuery side you name the dataset and the table and choose the region a new dataset is created in, which matters as soon as data residency is a requirement. Coupler.io creates anything that does not exist yet and works out the table schema from the data itself.

## Why export Google Analytics to BigQuery — real life cases

Here is what teams actually do with the data once it lands in BigQuery.

- **Get a website dashboard that refreshes itself**

  The everyday case: sessions, users, channel mix, key events, and revenue need to be in a dashboard each morning, and nobody wants to open GA4 to copy them out. Coupler.io writes the report you defined into BigQuery on your schedule and Looker Studio, Power BI, or Tableau read straight from there. Everyone sees the same figures, including the people who find the GA4 interface actively unhelpful.

- **Report on every property as if it were one**

  Multi-brand, multi-region, and multi-domain businesses usually have a property each, which makes a group-level number a weekly copy-paste exercise. Pick them all in one flow and Coupler.io loads them into a single labelled BigQuery table, so the roll-up and the per-property breakdown come from the same place and agree with each other by construction.

- **See what a channel costs and what it returned, in one row**

  GA4 knows what visitors did and the ad platforms know what you paid. Coupler.io joins them on the way in, so blended CAC, cost per key event, and revenue per channel land in BigQuery already calculated, with no spreadsheet in the middle. The channels that only looked profitable in-platform tend to become obvious within a week.

- **Attach session behaviour to customers who actually paid**

  On-site conversion is a proxy; the invoice is the fact. Blend GA4 with your Stripe, Shopify, or HubSpot data before load and BigQuery holds the whole path: which acquisition source produced the signups, which signups became paying customers, and what they were still worth three months later. That makes lifetime value by channel something you report rather than something you estimate once a year.

- **Keep history past GA4's retention window**

  GA4 expires detailed data on a clock, and older ranges get thinner the longer you leave them. A scheduled append into BigQuery builds your own archive instead: years of daily sessions, events, and revenue at full granularity, in storage that costs cents per gigabyte. Year-over-year seasonality, pre- and post-redesign comparisons, and tracking changes from two years ago all stay answerable.

- **Analyse without sampling getting in the way**

  Deep explorations in GA4 start sampling exactly when the question gets interesting. Once the rows are in BigQuery the numbers are the numbers, however many segments you cross and however long a period you cover, and the analysis runs in the warehouse your team already uses rather than inside a reporting interface.

- **Put content and SEO performance next to the money**

  Page paths, landing pages, and organic search clicks on their own tell you what got traffic. Joined to key events and purchase revenue in BigQuery, they tell you which articles and landing pages produce customers, which is a very different list. Content and SEO teams stop arguing about pageviews and start arguing about contribution, which is a better argument.

- **Measure the funnel against the business, not just the site**

  Add-to-carts, checkouts, and purchases describe the shop. Joined to fulfilment, support, and refund data in BigQuery, they describe the business: which traffic sources produce orders that ship cleanly, and which produce the ones that come back. Coupler.io does the blending before load, so the funnel table that lands is already the one worth reviewing.

## Other BigQuery integrations

Every other source you can land in BigQuery with the same no-code setup:

- [Connect Facebook Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-ads-to-bigquery.md)
- [Connect Facebook Page Insights to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-facebook-page-insights-to-bigquery.md)
- [Connect Google Ads to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-google-ads-to-bigquery.md)
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
