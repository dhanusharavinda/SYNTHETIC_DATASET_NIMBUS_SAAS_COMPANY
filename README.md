# SYNTHETIC_DATASET_NIMBUS_SAAS_COMPANY
Synthetic dataset generated for Data and AI project purpose, Company name: Nimbus(B2B SaaS, data analytics platform)

# Nimbus Analytics - Synthetic B2B SaaS Dataset

A reproducible, narrative-rich synthetic dataset for a fictional B2B SaaS
analytics company called **Nimbus Analytics**. Around 8,900 records across
four related tables spanning 24 months, deliberately seeded with four
findable business narratives and eight categories of intentional data
quality issues.

## At a glance

| | |
|---|---|
| Company         | Nimbus Analytics (fictional B2B SaaS) |
| Founded         | January 2024 |
| Data span       | 2024-01-01 to 2025-12-31 (24 months) |
| Customer count  | ~500 (350 SMB, 120 Mid-Market, 30 Enterprise) |
| Geography       | US, UK, Germany, France |
| Currency        | EUR throughout (Europe-realistic VAT applied) |
| Total records   | ~8,900 across four tables |

---

## Tables and relationships

Star-shaped schema with `crm_accounts` at the centre. Every dependent table
joins back via `customer_id`.

```
crm_accounts (500 rows)
   customer_id [PK]
        |
        |-- crm_deals (1,024 rows) ----- customer_id [FK]
        |       Lifecycle events: signup, expansion, downgrade, churn
        |
        |-- erp_invoices (5,352 rows) -- customer_id [FK]
        |       Monthly billing per active customer
        |       Country-specific VAT (DE 19%, UK / FR 20%, US 0%)
        |
        |-- support_tickets (2,032 rows) - customer_id [FK]
                subject, body, severity, status, csat_score, category
```

| File                    | Rows  | Format | What it holds |
|-------------------------|-------|--------|---------------|
| `crm_accounts.csv`      |   500 | CSV    | Customer master: company, segment, contact, geography, signup date |
| `crm_deals.csv`         | 1,024 | CSV    | Deal lifecycle events with signed MRR delta per event |
| `erp_invoices.csv`      | 5,352 | CSV    | Monthly invoices with `amount_excluding_vat`, `vat_rate`, `total_with_vat` |
| `support_tickets.json`  | 2,032 | JSON   | Customer-written tickets with severity, status, and CSAT |

Primary keys are sequential and clean (`CUST-0001`, `DEAL-000001`,
`INV-000001`, `TKT-000001`). Foreign-key integrity is enforced on every row.

---

## Four planted narratives

The data is intentionally seeded with four business stories findable via SQL.
They are what makes this dataset interesting for analytics demos: dashboards
have something real to surface, agents have something meaningful to discover.

### 1. SMB Pricing Crisis (August-September 2025)

The Starter tier price rose from EUR 89 to EUR 125 on 2025-08-15 (a 40%
increase). Internal projection was 5% SMB churn; actual was around 18% in
the eight weeks that followed.

Findable as: 60 SMB churn deals concentrated in the 2025-08-20 to 2025-09-30
window, ~100 complaint tickets in the same window mentioning the price
change, and a visible dip in SMB monthly invoice volume (Aug 263 invoices,
Sep 238).

### 2. Dashboard Rendering Bug (5-26 December 2025)

A three-week frontend regression caused dashboard charts to render blank or
as a white screen for many customers.

Findable as: ~70 high / critical severity tickets in the window with
characteristic keywords (blank, white screen, not loading, broken,
rendering).

### 3. Acme Corp Lifecycle Saga (Jan 2024 - April 2025)

The full journey of one Enterprise customer (`CUST-0001`): signup in
January 2024, three expansion deals during the first year, deteriorating
support tickets through Q1 2025, downgrade on 2025-03-28, and churn on
2025-04-30. Cumulative billing of EUR 115,500 across 16 invoices.

Good showcase for cohort analysis, customer lifetime value, expansion
revenue tracking, and churn-signal detection.

### 4. AI Insights Feature Launch (October 2025)

A flagship feature launched on 2025-10-01, with measurable downstream
impact.

Findable as: a spike of ~70 expansion deals for Mid-Market and Enterprise
customers in October-November, plus ~60 feedback tickets in the same window
(mix of praise and questions). Useful for demonstrating feature-adoption
measurement.

---

## Eight intentional data quality issues

The dataset is realistically messy. The Silver layer of a Medallion
architecture has real cleaning work to do.

| # | Issue                       | Example |
|---|-----------------------------|---------|
| 1 | Email hygiene               | Leading / trailing whitespace, mixed case, double dots |
| 2 | Company name duplicates     | `Globex Corp` / `GLOBEX CORP.` / `globex corp` / `Globex Corporation` |
| 3 | Date format inconsistency   | Mix of ISO, US `MM/DD/YYYY`, EU `DD-MM-YYYY` in the same column |
| 4 | Missing values              | ~4% null phones, ~10% null csat_score, ~2% null industries |
| 5 | Negative invoice amounts    | Legitimate refund rows |
| 6 | Suspicious round numbers    | Exactly EUR 99,999 invoices (test-data leaks) |
| 7 | Encoding artifacts          | Mojibake (e.g., `Ã©` instead of `é`) |
| 8 | Whitespace in text fields   | Double spaces, stray newlines in ticket subjects |

Primary keys and the narrative-anchor dates are always kept clean.



## Reproducible

Single `random.seed(42)` everywhere. Same code produces structurally
identical output across machines and runs.
