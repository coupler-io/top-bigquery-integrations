---
title: Connect Xero to BigQuery with Coupler.io
slug: connect-xero-to-bigquery
canonical: https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-xero-to-bigquery.md
source: Xero
destination: BigQuery
meta_title: Connect Xero to BigQuery with Coupler.io
meta_description: Load Xero invoices, bank transactions, journals, contacts, payroll, and project data into BigQuery on a schedule. Multi-organisation reporting without a single manual export.
---

![Xero](https://static.coupler.io/importers/xero.svg) → ![BigQuery](https://static.coupler.io/importers/google_big_query.svg)

# Connect Xero to BigQuery with Coupler.io

Xero is excellent at running an organisation and awkward at reporting across several. Each tenant keeps its own books, reports come out one entity at a time, and anything that spans companies, currencies, or more than a couple of years ends up in a spreadsheet somebody maintains by hand. Coupler.io moves Xero data into BigQuery on a schedule, as tables you own, where invoices, journals, payroll, and project time can finally be queried together with everything else the business runs on.

## How to export Xero data to BigQuery

Authorise Xero once and work through three screens. No developer, no API client to register, and no per-entity export routine to remember. Here is each step and what belongs in it.

### Step 1. Basic settings

- **Xero tenant** — which organisation this flow reads from. If your practice or group holds several, you pick the one in scope here and set up a flow per entity, which is exactly what makes group consolidation possible later.
- **Data entity** — the choice everything else follows. Accounting records, payroll, or projects, down to the specific object. The full catalogue is further down the page.
- **Report type** — how much detail each record carries. The simple version gives you the standard fields for any entity; the detailed version opens up a much wider set, and is available for invoices, contacts, bank transactions, credit notes, manual journals, overpayments, and prepayments.
- **Projects** — when you are pulling tasks or time, the projects to draw from.

### Step 2. Date filters

- **Changed after** — the one to reach for on a scheduled flow: fetch only records touched since a given point, so a long-established ledger still refreshes in seconds. A relative setting such as *days ago*, *this month*, *last month*, *this year*, or *last year* keeps the window moving on its own; a macro like `{{365daysago}}` handles a precise trailing span.
- **Start date** — the opening of the period for timesheets and project time.
- **End date** — where that period closes. Keep it relative and every run stays current without anyone editing the flow.
- **Created after** — the earliest creation date for purchase orders and quotes.
- **Created before** — and the latest.
- **Expiring after** — for quotes, the start of the expiry window, which is how you build a pipeline view of what is about to lapse.
- **Expiring before** — the far edge of that expiry window.

### Step 3. Advanced settings

- **Statuses** — which invoice states to bring in, so drafts and voided documents do not inflate figures you are about to present.
- **Journal number more than** — begin the journal pull from a known point and move forward, which makes backfilling a long ledger manageable.
- **Journal number less than** — the ceiling on that range, for rebuilding one specific slice.
- **Split by** — turn a nested document into analysable rows. An invoice with several line items becomes one row per line, which is the shape you need before any product or account-level analysis is possible.
- **Status** — a single-state filter for pay runs, timesheets, linked transactions, purchase orders, and quotes.
- **Statuses** — for projects and project time, whether you want work in progress, closed, active, or already invoiced.
- **Advanced settings** — the fine control: filter on a property, sort the result, target specific contact or invoice IDs, search by term, or set decimal precision on unit amounts.
- **Include archived** — decide whether archived invoices, contacts, and tracking categories belong in the table. Usually yes for a historical archive, usually no for an operational dashboard.

### Step 4. Blend and transform data

- **Column management** — hide the fields nobody looks at, rename Xero's field names into labels a non-accountant understands, reorder them, and set the right data types.
- **Join** — connect Xero to another source on a shared key, so an invoice arrives already carrying the customer record or project it relates to.
- **Append** — stack the same entity from several Xero organisations into one table, labelled by tenant. This is the backbone of group reporting.
- **Aggregate** — roll transaction-level rows up to the monthly or quarterly grain your management accounts use.
- **Custom SQL** — for whatever is left. Write the query and Coupler.io loads the result.

### Step 5. BigQuery destination

- **Data location** — the region a new dataset is created in. Leave it blank for US, or set it where your finance data is obliged to sit. Aim at an existing dataset and it keeps the region it was created in, whatever you type here.
- **Dataset name** — an existing dataset or a new name; Coupler.io creates it if it is missing.
- **Table name** — the same. Name it and it appears on the first run.

Nothing further to configure. Coupler.io determines the table schema from your data, so no column mapping and no manual types.

### Step 6. Schedule

Choose the refresh interval — every 15 minutes at the quickest, or hourly, daily, weekly, monthly — and whether each run replaces the table or appends to it. After that it runs without supervision. Token refreshes, rate limits, pagination, and retries sit with Coupler.io.

### Bonus: or just ask the AI Agent to build it for you

Would rather not fill any of that in? You do not have to. The Coupler.io AI Agent lives inside the app. Open it and describe what you need in plain words:

> *"Pull authorised and paid Xero invoices for the last three years, split out by line item, with contact and account detail, and load them into BigQuery."*

It knows the entities Xero exposes and which filters apply to each, so it picks the data entity, sets the report type, chooses the statuses, splits the line items, sets the date window, and prepares the destination. Where there is a genuine decision for you, it asks. Where there is not, it proceeds.

Four things stay with you:

1. **Connect Xero** — authorising Xero is yours to do.
2. **Connect BigQuery** — as is your Google Cloud access.
3. **Pick the organisation** — you choose which tenant this flow covers.
4. **Name the dataset and table** — you decide where in the warehouse it lands.

That is the whole manual part. If you are combining several organisations into one table, you will finish that in the wizard; everything else the Agent does for you.

## What data can you export from Xero to BigQuery with Coupler.io?

Fifty-seven data entities are available across four groups: accounting, payroll for the UK and New Zealand, payroll for Australia, and projects. One flow carries one entity, so most finance teams run several together — transactional tables at the centre, reference tables around them. Coupler.io's built-in **Join** transformation can assemble them before the load, so BigQuery receives one ready-to-query table rather than a set of fragments. Load them raw and model them in the warehouse instead, if that is how your data team prefers to work. If you want Xero's prepared financial statements rather than the underlying records, there is a separate Xero Reports connector for that.

- **Accounts** — your chart of accounts, with types, codes, and current balances. The lookup table almost everything else joins back to.
- **Bank transactions** — money actually moving through your accounts, which is the reality check on anything the ledger claims.
- **Bank transfers** — funds moved between your own accounts, so internal movement is not mistaken for trading activity.
- **Branding themes** — the invoice templates in use, worth having when document presentation is part of a process audit.
- **Contact groups** — how your customers and suppliers are grouped, ready-made segments for revenue and spend analysis.
- **Contacts** — customers and suppliers with addresses, payment terms, tax settings, and balances. The dimension table for both receivables and payables work.
- **Credit notes** — money credited back, with the reason and the document it offsets. The difference between gross and net revenue.
- **Currencies** — the currencies your organisation operates in, which you need the moment reporting crosses borders.
- **Employees** — the staff records held in your accounting organisation.
- **Expense claims** — what people spent and claimed back, where a lot of quiet cost lives.
- **Invoices** — sales and purchase invoices with totals, tax, due dates, status, and the contact attached. Split by line item and this becomes the most valuable table in your warehouse.
- **Items** — the products and services you invoice for, with their sales and purchase pricing.
- **Journals** — every posting Xero has made, which is the complete audit trail and the foundation of any serious financial model.
- **Linked transactions** — billable costs tied to the customer they will be recharged to, so cost recovery can be tracked rather than assumed.
- **Manual journals** — the adjustments someone entered by hand, with narration. Always worth reviewing as a set rather than one at a time.
- **Organisation** — the organisation's own settings, including financial year end and base currency, which you need to interpret everything else correctly.
- **Overpayments** — customer money received beyond what was invoiced, sitting there waiting to be allocated.
- **Payments** — what was actually settled, against which document, and when. The basis for days sales outstanding and payment behaviour analysis.
- **Prepayments** — amounts paid ahead of the invoice, which matter for revenue recognition as much as for cash.
- **Purchase orders** — committed spend before it becomes a bill, which is the number budget holders actually need.
- **Receipts** — expense receipts captured against the organisation.
- **Repeating invoices** — the recurring billing schedule, which is the closest thing in Xero to a forward revenue view.
- **Tax rates** — the rates applied across your transactions, needed to check a filing against the ledger.
- **Tracking categories** — your custom dimensions, such as department, region, or project. These are what make a P&L sliceable rather than a single column of numbers.
- **Users** — who has access to the organisation and at what level, relevant to any controls review.
- **Quotes** — what you have proposed and not yet won, with expiry dates. The pipeline that sits in front of the ledger.
- **Projects** — project records with budgets, status, and the contact behind them.
- **Tasks by projects** — the tasks inside each project, with their rates and estimates.
- **Time by projects** — logged hours against projects and tasks, including whether each entry is chargeable and whether it has been invoiced. Pair it with project budgets and you have genuine project profitability.
- **Users** — the people working on projects, for utilisation and capacity reporting.
- **Payroll for the UK and New Zealand** — twelve entities covering Pay runs, Pay run calendars, Employees, Timesheets, Earning rates, Deductions, Benefits, Reimbursements, Leave types, Earnings orders, Accounts, and Tracking categories. Enough to analyse labour cost by period, team, and pay element without exporting a payslip.
- **Payroll for Australia** — fifteen entities covering Pay runs, Payroll calendars, Employees, Timesheets and timesheet lines, Leave applications and their leave periods, Leave types, Earnings rates, Deduction types, Reimbursement types, Super funds, Settings, Accounts, and Tracking categories. The same analysis with the local specifics intact.

On the BigQuery side you name the dataset and table and pick the region a new dataset is created in, which matters when finance data residency is a policy and not a preference. Coupler.io creates whatever is missing and works out the table schema from the data itself.

## Why export Xero to BigQuery — real life cases

There are two honest ways to run this, and the right one depends on your team, not on which is more sophisticated.

| | No-code route | Warehouse-led route |
| --- | --- | --- |
| **Who it's for** | Marketers, agency teams, finance — anyone without SQL, or with SQL but no spare analyst | Data teams who already own a modeling layer |
| **Where logic lives** | In Coupler.io: join, append, aggregate, and column management before load | In BigQuery: dbt models, scheduled queries, views |
| **BigQuery's job** | Storage and reporting — the tables arrive analysis-ready | Coordinator — it owns transformation and orchestration |
| **What you get** | A working dashboard today, and no ticket in someone else's backlog | Full version control and reuse across every source |

The cases below describe what teams actually do with the data. Both routes get you there; the no-code route just gets you there this afternoon.

- **Have the finance dashboard update without anyone exporting anything**

  The ordinary case, and the one most teams start with: revenue, outstanding invoices, cash in, and cash out need to be on a screen the leadership team can open any day of the week. Coupler.io refreshes those tables into BigQuery on your schedule, and Looker Studio, Power BI, or Tableau read straight from them. The board pack stops being a monthly production and becomes a link.

- **Consolidate a group of organisations into one set of numbers**

  Xero keeps each tenant separate, which is correct for bookkeeping and painful for group reporting. Run a flow per organisation and use Coupler.io's Append transformation to stack them with consistent columns and a tenant label, and BigQuery holds the whole group in one table. Comparing entities, rolling them up, and spotting the one that is drifting all become filters instead of a monthly consolidation workbook.

- **Shorten the month-end close**

  Close drags because the checks are manual and the data arrives late. With journals, invoices, payments, and bank transactions landing in BigQuery on a schedule, the reconciliations and variance checks you repeat every month turn into saved queries that have already run by the time you open them. The first conversation of close becomes about the exceptions rather than about assembling the file.

- **Match what was invoiced against what actually arrived**

  Invoices, payments, credit notes, overpayments, and prepayments in one warehouse make reconciliation a query. Short payments, unallocated receipts, duplicate bills, and credits nobody applied surface the same week rather than at year end, and the aged debt figure you quote stops needing an apology attached.

- **Turn tracking categories into a P&L you can actually slice**

  Tracking categories are the most underused thing in Xero. Bring journals and invoices into BigQuery with their tracking dimensions intact and you can report profit by department, region, brand, or programme without maintaining a mapping spreadsheet. The question "which part of the business is carrying the rest" becomes answerable from a chart.

- **See cash flow coming rather than reporting it afterwards**

  Invoices with due dates, purchase orders, repeating invoices, and recent payment behaviour give you a forward view instead of a historical one. In BigQuery you can project receipts and commitments week by week, adjust for how reliably each customer actually pays, and know in advance which weeks are tight. Refreshes as often as every 15 minutes mean the forecast moves when the bank does.

- **Work out whether projects and people pay for themselves**

  Project time, tasks, budgets, and payroll cost in the same warehouse answer the question every services business asks and few can evidence: which clients and which projects are genuinely profitable once the hours are counted properly. Chargeable versus non-chargeable time, budget burn, and realised rate all come out of tables that refresh themselves.

- **Keep an audit-ready archive of every posting**

  Journals and invoices appended into BigQuery build a permanent, granular record that costs very little to retain for years. Audit requests, historical restatements, and "what did this account look like in March two years ago" turn into queries rather than archaeology, and the answer is the same every time anybody asks.

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
- [Connect TikTok Organic to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-tiktok-organic-to-bigquery.md)
- [Connect WooCommerce to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-woocommerce-to-bigquery.md)
- [Connect YouTube Analytics to BigQuery](https://github.com/coupler-io/top-bigquery-integrations/blob/main/connect-youtube-analytics-to-bigquery.md)
