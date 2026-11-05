Agency MRR/ARR Subscription & Operations System

A 5-sheet financial management workbook built in Google Sheets to track, audit, and forecast recurring subscription revenue for digital agencies, B2B SaaS platforms, and service businesses. This system automates client directory tracking, subscription transaction streams, backend MRR/ARR engine calculations, cohort revenue retention (NRR/GRR), and executive oversight controls.

🏛️ Architecture & System Data Flow

The system processes data across five functional layers:

Master Directory (01_Client_Master_Directory): Stores core account metadata, SLA tiers, account managers, contract dates, and baseline MRR.

Transaction Stream (02_Subscription_Billing_Ledger): Logs billing events, expansion upgrades, contraction downgrades, churn cancellations, and payment statuses.

Backend Engine (03_MRR_ARR_Calculation_Engine): Computes active MRR, annualized ARR, account LTV, dynamic churn warnings, and portfolio-wide ARPU.

Cohort Analytics (04_Financial_Pivots_Cohort): Summarizes service tier performance, account manager impact, Gross Revenue Retention (GRR), and Net Revenue Retention (NRR).

Executive Console (05_Executive_Operations_Console): Provides executive KPI oversight, operational risk alerts, and Google Apps Script execution triggers.

📋 Module & Sheet Specifications

1. 01_Client_Master_Directory

Holds 100 client records serving as the primary lookup directory.

Key Columns: Client_ID, Client_Name, Service_Tier, Account_Manager, Contract_Start_Date, Contract_Term_Months, Baseline_MRR, Status

Formats: Dates as YYYY-MM-DD, Baseline MRR as Currency ($#,##0.00).

2. 02_Subscription_Billing_Ledger

Tracks active billing runs and transactional account changes.

Key Columns: Billing_ID, Billing_Date, Client_ID, Event_Type, Billed_Amount, Expansion_MRR, Contraction_MRR, Net_MRR_Impact, Payment_Status

Event Types: New Subscription, Renewal, Expansion Upgrade, Contraction Downgrade, Churn Cancellation

Conditional Formatting: Green highlight for positive net impact, red for negative net impact and failed payments, yellow for pending invoices.

3. 03_MRR_ARR_Calculation_Engine

Computes active recurring revenue streams and flags account changes.

Key Columns: Client_ID, Client_Name, Service_Tier, Baseline_MRR, Net_Expansion_Contraction, Active_Current_MRR, Annualized_ARR, Estimated_LTV, Account_Status_Alert

Summary Block: Calculates Total Active MRR, Portfolio ARR, Total Estimated LTV, Active Client Count, Average Revenue Per Account (ARPU), and Total Churned Accounts.

4. 04_Financial_Pivots_Cohort

Aggregates financial performance across service tiers, manager portfolios, and cohort retention metrics.

Service Tier Breakdown: Evaluates Enterprise, Growth, and Starter tiers by active client counts, baseline MRR, net expansion/contraction, and ARPU.

Account Manager Breakdown: Measures managed accounts, total MRR under management, expansions closed, churned accounts, and net impact per manager.

NRR & Cohort Retention Block: Evaluates total portfolio retention through itemized formulas:

Net Ending Retained MRR: Starting Baseline + Expansion - Contraction - Churn

Gross Revenue Retention (GRR): (Starting Baseline - Contraction - Churn) / Starting Baseline

Net Revenue Retention (NRR): Net Ending Retained MRR / Starting Baseline

5. 05_Executive_Operations_Console

Executive dashboard compiling top-level KPIs, operational risk alerts, and automated re-scan tools.

KPI Summary Bar: Highlights Total MRR, Annualized ARR, NRR %, GRR %, and Active Client Count.

Operational Control Strip: Real-time counters tracking churned, contracted, expanded, and active stable accounts with dynamic health status text.

Tier Distribution Matrix: Shows revenue share percentage broken down by account tier.

⚙️ Core Formulas & Logic

Billed Amount Calculation (02_Subscription_Billing_Ledger E2):

=IF(D2="Churn Cancellation", 0, IFERROR(VLOOKUP(C2, '01_Client_Master_Directory'!A$2:G$101, 7, FALSE), 0) + F2 - G2)


Net Expansion & Contraction Impact (03_MRR_ARR_Calculation_Engine E2):

=SUMIF('02_Subscription_Billing_Ledger'!C$2:C$101, A2, '02_Subscription_Billing_Ledger'!F$2:F$101) - SUMIF('02_Subscription_Billing_Ledger'!C$2:C$101, A2, '02_Subscription_Billing_Ledger'!G$2:G$101)


Active Current MRR (03_MRR_ARR_Calculation_Engine F2):

=IF('01_Client_Master_Directory'!H2="Churned", 0, MAX(0, D2 + E2))


Account Status Alert (03_MRR_ARR_Calculation_Engine I2):

=IFS('01_Client_Master_Directory'!H2="Churned", "🔴 CHURNED ACCOUNT", F2>D2, "🟢 EXPANDED (UPGRADED)", F2<D2, "🟡 CONTRACTED (DOWNGRADED)", TRUE, "🔵 ACTIVE STABLE")

Tier Expansion Attribution (04_Financial_Pivots_Cohort D2):

=SUMPRODUCT(('01_Client_Master_Directory'!C$2:C$101=A2) * ('02_Subscription_Billing_Ledger'!F$2:F$101))


Tier Expansion Attribution (04_Financial_Pivots_Cohort D2):

=SUMPRODUCT(('01_Client_Master_Directory'!C$2:C$101=A2) * ('02_Subscription_Billing_Ledger'!F
