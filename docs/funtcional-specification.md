# Treasury & Markets Valuation Engine

# Functional Specification

Version: 1.0 Draft
Document Owner: Treasury & Markets
Reporting Perimeter: MMT Group

---

# 1. Introduction

## 1.1 Purpose

The purpose of the Treasury & Markets Valuation Engine is to provide a controlled, auditable and scalable platform for valuation, PnL attribution, intercompany management, exposure reporting and future risk analytics.

The solution will replace the current spreadsheet-based process and provide a single source of truth for Treasury & Markets reporting.

The platform shall support:

* Daily PnL Flash Reporting
* NAV Reporting
* Exposure Reporting
* Intercompany Reporting
* Treasury Reporting
* Management Reporting
* Historical Reconstruction
* Auditability
* Future Risk Analytics

---

## 1.2 Scope

Version 1 shall support:

* Wallet Balances
* Exchange Rates
* Valuation Processing
* PnL Attribution
* Intercompany Processing
* Adjustment Processing
* Reporting
* Audit & Versioning

Version 1 shall exclude:

* Liquidity Pools
* Bank Accounts
* Centralised Exchange Accounts
* Transaction-Level PnL
* VaR
* Stress Testing

These items are documented within Future Enhancements.

---

## 1.3 Reporting Perimeter

The valuation engine shall support reporting for the Treasury, Markets and Mint businesses only.

This reporting perimeter shall be referred to as:

**MMT Group**

Examples include:

* MINTZA
* MKTSZA
* TREASZA
* MKTSUK
* MINTUK
* GRP TREAS

Non-MMT entities may participate in intercompany relationships but shall be excluded from standard MMT reporting outputs.

---

## 1.4 Objectives

The primary objectives are:

* Replace manual spreadsheet processing.
* Improve auditability.
* Improve control environment.
* Support reruns and versioning.
* Support multi-currency reporting.
* Support future Tableau reporting.
* Support future VaR and risk reporting.
* Provide a scalable Treasury & Markets reporting platform.

---

# 2. Source Data Specification

## 2.1 Overview

The valuation engine shall consume source data provided by operational systems.

Version 1 shall support CSV-based source files.

Future releases may support direct integrations and APIs.

---

## 2.2 Source Files

Version 1 shall support:

### Historical Balances File

Contains:

* Valuation Date
* Entity
* Wallet
* Asset
* Balance Quantity

Balances represent end-of-day wallet positions.

Balances are considered the source of truth for valuation processing.

---

### Exchange Rates File

Contains:

* Valuation Date
* Asset
* Reporting Currency
* Exchange Rate

Rates shall be maintained using:

Asset → Reporting Currency

Example:

1 USDC = 18.20 ZAR

---

## 2.3 Valuation Dates

Users shall explicitly select:

* Valuation Date
* Comparison Date

The system shall not assume a fixed T versus T-1 comparison.

This supports:

* Weekends
* Public Holidays
* Month-End Reporting
* Historical Reruns

---

## 2.4 Validation Requirements

The valuation engine shall validate:

* Required files present.
* Mandatory fields populated.
* No duplicate balance records.
* Exchange rates available.
* Valid exchange rate values.

Missing mandatory data shall result in a Hard Stop exception.

---

## 2.5 Future Source Files

Future releases may support:

* Liquidity Pool Files
* Bank Account Files
* Centralised Exchange Files
* Automated Effects Files
* Direct Database Integrations

---

# 3. Master Data Specification

## 3.1 Overview

Master Data provides the business structure required to support valuation, reporting, intercompany processing and auditability.

All reporting shall be driven through approved master data.

Master data shall support effective dating.

Historical reporting results shall not change due to future master data changes.

---

## 3.2 Effective Dating

All master records shall support:

* Effective From Date
* Effective To Date
* Active Flag

The valuation engine shall determine the applicable master record based on the valuation date being processed.

---

## 3.3 Entity Master

### Purpose

Defines all legal entities recognised by the valuation engine.

### Minimum Attributes

* Entity Code
* Entity Name
* Entity Type
* Reporting Group
* Functional Currency
* Effective From Date
* Effective To Date
* Active Flag

### Reporting Group

Entities shall be classified as:

* MMT
* Non-MMT

Only MMT entities shall be included in standard reporting outputs.

---

## 3.4 Wallet Master

### Purpose

Defines ownership and reporting attributes for all wallets.

### Minimum Attributes

* Wallet Identifier
* Wallet Name
* Public Key
* Network
* Entity
* Strategy
* Wallet Type
* Effective From Date
* Effective To Date
* Active Flag

Each wallet shall belong to:

* One Entity
* One Strategy

at a point in time.

---

## 3.5 Strategy Master

### Purpose

Defines the business purpose of each wallet.

### Version 1 Strategies

* OTC Trading
* Liquidity Management
* Automated Market Making
* Treasury
* General Wallets

Strategies shall be assigned at wallet level.

---

## 3.6 Asset Master

### Purpose

Defines all assets recognised by the valuation engine.

### Minimum Attributes

* Asset Code
* Asset Type
* Asset Issuer
* Asset Family
* Asset Currency
* Effective From Date
* Effective To Date
* Active Flag

### Asset Currency Examples

| Asset | Asset Currency |
| ----- | -------------- |
| USDC  | USD            |
| USDZ  | USD            |
| ZARZ  | ZAR            |
| EURC  | EUR            |
| BTC   | BTC            |
| XLM   | XLM            |

---

## 3.7 Asset Family Master

### Purpose

Provides grouping of economically similar assets.

### Examples

| Asset | Asset Family |
| ----- | ------------ |
| USDC  | USD          |
| USDZ  | USD          |
| ZARZ  | ZAR          |
| EURC  | EUR          |

The solution shall support reporting by:

* Asset
* Asset Family

---

## 3.8 Intercompany Routing Rules

All intercompany relationships shall be routed through:

GRP TREAS

This applies regardless of whether movements occur directly between operating entities.

---

## 3.9 Master Data Controls

### Hard Stop Controls

The valuation run shall fail if:

* Wallet mapping missing.
* Entity mapping missing.
* Strategy mapping missing.
* Asset mapping missing.
* Asset Family mapping missing.

### Warning Controls

Warnings shall be generated for:

* New Wallets
* Removed Wallets
* New Assets
* Removed Assets
* New Entities
* Removed Entities

Warnings shall not stop processing.

---

# 4. Run Lifecycle & Workflow

## 4.1 Overview

A valuation run represents a complete execution of the valuation process for a specific valuation date.

Multiple runs may exist for the same valuation date.

---

## 4.2 Run Parameters

Mandatory parameters:

* Valuation Date
* Comparison Date
* Reporting Currency

Supported reporting currencies:

* GBP
* USD
* ZAR

---

## 4.3 Run Creation

The Trading Assistant shall create a valuation run.

The system shall generate:

* Run ID
* Creation Timestamp
* User
* Status

Initial Status:

Draft

---

## 4.4 Source Data Upload

Following run creation, users shall upload:

* Historical Balances File
* Exchange Rates File

The system shall record:

* File Name
* Uploaded By
* Upload Timestamp
* Record Count

---

## 4.5 Validation Stage

The system shall perform:

### Source Data Validation

* Required files present.
* Mandatory fields populated.
* No duplicate balances.
* Exchange rates available.

### Master Data Validation

* Wallet mappings.
* Entity mappings.
* Strategy mappings.
* Asset mappings.
* Asset Family mappings.

---

## 4.6 Valuation Processing

Following successful validation the engine shall calculate:

* Positions
* NAV
* Trading PnL
* Revaluation PnL
* Economic PnL
* Reported PnL

---

## 4.7 Draft Run Review

The Trading Assistant shall review:

* Validation Exceptions
* PnL Results
* NAV Results
* Intercompany Adjustments
* Reconciliation Adjustments

Draft runs remain editable.

---

## 4.8 Approval Workflow

The Trading Manager shall review:

* Valuation Results
* Adjustments
* Validation Results
* Commentary

Approval of a run constitutes approval of:

* Valuation Results
* Intercompany Adjustments
* Reconciliation Adjustments
* Manual Adjustments

---

## 4.9 Publication

Published runs shall:

* Lock valuation results.
* Lock adjustments.
* Lock source file references.

Published runs become immutable.

---

## 4.10 Run Versioning

Multiple versions may exist for a valuation date.

Example:

| Run ID | Valuation Date | Version | Status    |
| ------ | -------------- | ------- | --------- |
| 1001   | 30-Jun-2026    | Draft   | Draft     |
| 1002   | 30-Jun-2026    | Draft   | Draft     |
| 1003   | 30-Jun-2026    | V1      | Published |
| 1004   | 30-Jun-2026    | V2      | Published |

Published versions remain available for audit purposes.

---

## 4.11 Re-Runs

Users shall be able to rerun historical valuation dates.

Reasons may include:

* Rate Corrections
* Master Data Updates
* Reconciliation Adjustments
* Audit Requests

Historical published versions shall remain unchanged.

# 5. Validation & Controls

## 5.1 Overview

The Treasury & Markets Valuation Engine shall perform validation, control and reconciliation checks before and after valuation processing.

The objectives are:

* Ensure completeness of source data.
* Ensure integrity of master data.
* Detect unexpected operational changes.
* Validate calculation accuracy.
* Improve auditability.
* Reduce operational risk.

Controls shall be classified as:

* Hard Stop Controls
* Warning Controls
* Reconciliation Controls
* PnL Attribution Controls

---

## 5.2 Hard Stop Controls

Failure of a Hard Stop Control shall result in:

Run Status = Failed

Valuation processing shall not continue until the issue is resolved.

### Source Data Controls

The run shall fail if:

* Historical Balances File missing.
* Exchange Rates File missing.
* Mandatory fields missing.
* Duplicate balance records exist.
* Invalid balance values exist.
* Invalid exchange rates exist.
* Exchange rates are zero or negative.

---

### Exchange Rate Controls

The run shall fail if:

* Valuation Date rate missing.
* Comparison Date rate missing.
* Reporting currency conversion unavailable.

No default rates shall be applied.

---

### Master Data Controls

The run shall fail if:

* Wallet mapping missing.
* Entity mapping missing.
* Strategy mapping missing.
* Asset mapping missing.
* Asset Family mapping missing.

---

### Effective Dating Controls

The run shall fail if:

* No active record exists.
* Multiple active records exist.
* Effective date ranges overlap.

---

## 5.3 Warning Controls

Warning Controls shall not prevent completion of the valuation run.

### Wallet Controls

Generate warnings for:

* New Wallets
* Removed Wallets
* Wallet Ownership Changes
* Wallet Strategy Changes

---

### Asset Controls

Generate warnings for:

* New Assets
* Removed Assets
* Asset Family Changes

---

### Entity Controls

Generate warnings for:

* New Entities
* Removed Entities
* Reporting Group Changes

---

### Counterparty Controls

Generate warnings for:

* New Intercompany Counterparties
* New Non-MMT Counterparties

---

### Materiality Controls

Support configurable thresholds for:

* Wallet NAV Movement
* Entity NAV Movement
* Asset Position Movement
* Daily PnL Movement
* Intercompany Movement
* Asset Family Exposure Movement

Thresholds shall be configurable without code changes.

---

### PnL Attribution Controls

Generate warnings where:

* Unexplained PnL exceeds threshold.
* Unexplained PnL materially changes.
* Entity-specific thresholds breached.

---

## 5.4 Reconciliation Controls

### Economic PnL Attribution Control

Economic PnL

=

Trading PnL

*

Revaluation PnL

*

Unexplained PnL

---

### Unexplained PnL

Unexplained PnL

=

Economic PnL

−

Trading PnL

−

Revaluation PnL

The run shall not fail solely due to Unexplained PnL.

Threshold breaches shall generate warnings.

---

### Reported PnL Control

Reported PnL

=

Economic PnL

*

Adjustment PnL

Any variance shall result in a Hard Stop exception.

---

### NAV Control

NAV Movement

=

Closing NAV

−

Opening NAV

Any variance shall result in a Hard Stop exception.

---

## 5.5 Exception Reporting

Every run shall generate:

* Hard Stop Exceptions
* Warning Exceptions
* Reconciliation Exceptions

Exception reports shall include:

* Severity
* Entity
* Wallet
* Asset
* Description
* Impact Value

---

## 5.6 Control Configuration

Control parameters shall support:

* Threshold maintenance
* Audit history
* User tracking
* Timestamp tracking

No code changes shall be required.

---

# 6. Valuation Methodology

## 6.1 Overview

The valuation engine shall calculate positions and NAV using wallet balances and exchange rates.

Supported reporting currencies:

* GBP
* USD
* ZAR

---

## 6.2 Valuation Principles

Valuation shall use:

* Historical Balances
* Exchange Rates
* Selected Reporting Currency

---

## 6.3 Position Determination

Position grain:

* Valuation Date
* Entity
* Wallet
* Strategy
* Asset

Balance Quantity shall represent the closing position.

---

## 6.4 Exchange Rate Methodology

Rates shall be maintained as:

Asset → Reporting Currency

Example:

1 USDC = 18.20 ZAR

---

## 6.5 NAV Calculation

NAV

=

Balance Quantity

×

Exchange Rate

---

## 6.6 Opening NAV

Opening NAV

=

Balance(T-1)

×

Rate(T-1)

---

## 6.7 Closing NAV

Closing NAV

=

Balance(T)

×

Rate(T)

---

## 6.8 NAV Movement

NAV Movement

=

Closing NAV

−

Opening NAV

---

## 6.9 Reporting Currency Requirements

Supported reporting currencies:

* GBP
* USD
* ZAR

The selected currency shall apply consistently across all reporting outputs.

---

## 6.10 Asset Family Valuation

Reporting shall support:

* Asset Level
* Asset Family Level

Asset Family NAV shall equal the sum of underlying asset NAV.

---

# 7. PnL Methodology

## 7.1 Overview

The valuation engine shall support structured PnL attribution.

---

## 7.2 PnL Hierarchy

Reported PnL

=

Economic PnL

*

Adjustment PnL

---

Economic PnL

=

Trading PnL

*

Revaluation PnL

*

Unexplained PnL

---

Adjustment PnL

=

Intercompany Adjustments

*

Intercompany FX Revaluation Adjustments

*

Intercompany Interest Adjustments

*

GL Reconciliation Adjustments

*

Manual Adjustments

---

## 7.3 Trading PnL

Trading PnL

=

(Balance(T)

−

Balance(T-1))

×

Rate(T)

---

## 7.4 Revaluation PnL

Revaluation PnL

=

Balance(T-1)

×

(Rate(T)

−

Rate(T-1))

---

## 7.5 Explained PnL

Explained PnL

=

Trading PnL

*

Revaluation PnL

---

## 7.6 Unexplained PnL

Unexplained PnL

=

Economic PnL

−

Trading PnL

−

Revaluation PnL

---

## 7.7 Economic PnL

Economic PnL

=

Trading PnL

*

Revaluation PnL

*

Unexplained PnL

---

## 7.8 Adjustment PnL

Adjustment PnL

=

Intercompany Adjustments

*

Intercompany FX Revaluation Adjustments

*

Intercompany Interest Adjustments

*

GL Reconciliation Adjustments

*

Manual Adjustments

---

## 7.9 Reported PnL

Reported PnL

=

Economic PnL

*

Adjustment PnL

---

## 7.10 MTD PnL

MTD PnL shall represent cumulative Reported PnL from the first day of the month to the valuation date.

---

## 7.11 YTD PnL

YTD PnL shall represent cumulative Reported PnL from the first day of the financial year to the valuation date.

---

# 8. Intercompany Framework

## 8.1 Overview

The Intercompany Framework shall support:

* Internal Funding
* Treasury Movements
* Intercompany Reconciliation
* Interest Calculations
* FX Revaluation

---

## 8.2 Core Principles

Intercompany balances:

* Represent economic value owed.
* Operate as running balances.
* Support receivable/payable reporting.
* Support historical reconstruction.

---

## 8.3 Routing Principle

All intercompany relationships shall be routed via:

GRP TREAS

---

## 8.4 Intercompany Account Types

### Asset Intercompany Account

Used for:

* Stablecoins
* Crypto Assets
* Digital Assets

Examples:

* USDC
* USDZ
* ZARZ
* BTC
* XLM

---

### Fiat Intercompany Account

Used for:

* Bank Transfers
* Fiat Settlements
* Cash Funding

Examples:

* ZAR
* USD
* GBP
* EUR

---

### Scope Limitation

Out of scope:

* Management Fees
* Service Charges
* Cost Allocations
* Non-Treasury Intercompany Balances

---

## 8.5 Running Balance Methodology

Intercompany balances shall operate as continuous running balances.

Repayments shall reduce existing balances.

Over-settlement shall reverse the sign of the balance.

---

## 8.6 Economic Settlement Principle

Settlement shall be based on economic value returned.

Repayment does not need to occur using the same asset.

---

## 8.7 Functional Currency Accounting

Each entity shall maintain intercompany balances in its functional currency.

Examples:

* MKTSZA = ZAR
* TREASZA = ZAR
* MKTSUK = GBP
* MINTUK = GBP

---

## 8.8 FX Risk Ownership

Version 1 assumption:

FX Risk Owner

=

GRP TREAS

unless configured otherwise.

---

## 8.9 Intercompany FX Revaluation Adjustments

Intercompany FX Revaluation Adjustments:

* Recorded separately.
* Fully auditable.
* Support month-end reconciliation.

Version 1 shall support manual processing.

---

## 8.10 Intercompany Interest

Interest applies to:

* Asset Intercompany Accounts
* Fiat Intercompany Accounts

Interest Calculation:

Daily Closing Balance

×

Interest Rate

÷

365

Version 1 shall support manual journals.

---

## 8.11 Intercompany Reporting

Support reporting by:

* Entity
* Counterparty
* Intercompany Account Type

Metrics:

* Opening Balance
* Movements
* Interest
* FX Revaluation
* Closing Balance

---

## 8.12 Non-MMT Entities

Non-MMT entities shall use the same intercompany framework.

Examples:

* ZEAMZA

Non-MMT entities shall be excluded from standard MMT reporting.

---

## 8.13 Future Automation

Future versions may support:

* Effects File Integration
* Automated Intercompany Detection
* Automated Interest
* Automated FX Revaluation
* Automated Balance Maintenance

# 9. Adjustment Framework

## 9.1 Overview

The valuation engine shall support adjustments required for management reporting, reconciliation and intercompany processing.

Adjustments shall be maintained separately from source valuation data.

Source data shall remain immutable.

---

## 9.2 Adjustment Principles

The adjustment framework shall operate using the following principles:

* Source data shall never be modified.
* Adjustments shall be recorded separately.
* Adjustments shall be version controlled.
* Adjustments shall be attributable to individual valuation runs.
* Adjustments shall be auditable.
* Historical published runs shall remain unchanged.

---

## 9.3 Supported Adjustment Types

### Intercompany Adjustments

Used to record intercompany receivables and payables arising from internal movements.

---

### Intercompany FX Revaluation Adjustments

Used to align intercompany balances where counterparties maintain balances in different functional currencies.

---

### Intercompany Interest Adjustments

Used to record interest charged or earned on intercompany balances.

Version 1 shall support manual journals.

---

### GL Reconciliation Adjustments

Used to align valuation results to the General Ledger.

The General Ledger remains the accounting system of record.

---

### Manual Adjustments

Used to support exceptional circumstances not covered by standard valuation processing.

---

## 9.4 Mandatory Adjustment Fields

All adjustments shall contain:

* Adjustment Date
* Entity
* Adjustment Type
* Amount
* Description
* Prepared By

The system shall additionally record:

* Run ID
* Created Timestamp

---

## 9.5 Optional Fields

* Counterparty
* Asset
* Supporting Reference

---

## 9.6 Processing

Adjustments shall be loaded after valuation processing and prior to publication.

Adjustments shall remain editable while a run is in Draft status.

---

## 9.7 Approval Process

The Trading Assistant shall prepare adjustments.

The Trading Manager shall review and approve adjustments as part of run approval.

---

## 9.8 PnL Impact

Adjustments shall impact:

Adjustment PnL

Adjustments shall not directly impact:

* Trading PnL
* Revaluation PnL
* Economic PnL

---

## 9.9 GL Reconciliation Treatment

GL Reconciliation Adjustments shall impact only the valuation date on which they are recorded.

Historical periods shall not be restated.

---

## 9.10 Audit Requirements

The valuation engine shall retain:

* Adjustment History
* User
* Timestamp
* Run ID

Historical adjustment records shall never be deleted.

---

## 9.11 Future Enhancements

Future releases may support:

* Reason Codes
* Supporting Documentation
* Automated Adjustment Workflows
* Workflow-Based Approvals

---

# 10. Reporting Framework

## 10.1 Overview

The reporting framework shall support:

* Daily Reporting
* MTD Reporting
* YTD Reporting
* PnL Attribution Reporting
* Exposure Reporting
* Intercompany Reporting
* Adjustment Reporting

---

## 10.2 Reporting Principles

Reporting shall support:

* Drill-down reporting
* Aggregated reporting
* Historical reporting
* Multi-currency reporting
* Exposure reporting
* PnL attribution reporting

All reporting shall be generated from approved valuation runs.

---

## 10.3 Reporting Currency

Supported reporting currencies:

* GBP
* USD
* ZAR

Default reporting currency:

GBP

---

## 10.4 Reporting Hierarchy

MMT Group

↓

Entity

↓

Strategy

↓

Wallet

↓

Asset Family

↓

Asset

Users shall be able to drill down through each level.

---

## 10.5 Daily Reporting

Support reporting of:

* NAV
* Trading PnL
* Revaluation PnL
* Unexplained PnL
* Economic PnL
* Adjustment PnL
* Reported PnL

---

## 10.6 Month-to-Date Reporting

MTD Reporting shall represent cumulative Reported PnL from month start to valuation date.

---

## 10.7 Year-to-Date Reporting

YTD Reporting shall represent cumulative Reported PnL from financial year start to valuation date.

---

## 10.8 PnL Attribution Reporting

The following categories shall be reported separately:

* Trading PnL
* Revaluation PnL
* Unexplained PnL
* Economic PnL
* Intercompany Adjustments
* Intercompany FX Revaluation Adjustments
* Intercompany Interest Adjustments
* GL Reconciliation Adjustments
* Manual Adjustments
* Adjustment PnL
* Reported PnL

---

## 10.9 Exposure Reporting

Support:

* Asset Family Exposure
* Asset Exposure
* Asset Issuer Exposure
* Entity Exposure
* Strategy Exposure

---

## 10.10 Asset Family Reporting

Example:

| Asset Family | Exposure |
| ------------ | -------- |
| USD          | £150,000 |
| ZAR          | £500,000 |
| BTC          | £75,000  |

---

## 10.11 Asset Reporting

Users shall be able to drill down into individual assets.

Example:

| Asset | Exposure |
| ----- | -------- |
| USDC  | £100,000 |
| USDZ  | £50,000  |

---

## 10.12 Asset Issuer Reporting

Example:

| Issuer | Exposure |
| ------ | -------- |
| Circle | £100,000 |
| Zeam   | £50,000  |

Supports:

* Counterparty Risk
* Stablecoin Monitoring
* Concentration Risk

---

## 10.13 Intercompany Reporting

Report:

* Opening Balance
* Movements
* Interest
* FX Revaluation
* Closing Balance

---

## 10.14 Adjustment Reporting

Support analysis by:

* Entity
* Strategy
* Reporting Currency
* Adjustment Type

---

## 10.15 Pivot Reporting

Examples:

* Entity × Asset Family
* Strategy × Asset Family
* Entity × Adjustment Type
* Asset Family × Asset Issuer

---

## 10.16 Dashboard Requirements

Future dashboards:

* Executive Dashboard
* Treasury Dashboard
* Markets Dashboard
* Exposure Dashboard
* Intercompany Dashboard

---

## 10.17 Future Risk Reporting

Future support for:

* VaR
* Stress Testing
* Liquidity Risk
* Counterparty Risk
* Concentration Risk

---

# 11. Output Datasets

## 11.1 Overview

The valuation engine shall generate structured datasets for reporting and analytics.

All datasets shall be derived from approved valuation runs.

---

## 11.2 Dataset Principles

Datasets shall be:

* Version Controlled
* Auditable
* Reproducible
* Historical

All datasets shall contain:

* Run ID
* Version Number
* Valuation Date

---

## 11.3 PnL Dataset

Purpose:

Provide detailed PnL attribution.

Grain:

* Valuation Date
* Entity
* Strategy
* Wallet
* Asset

Metrics:

* Trading PnL
* Revaluation PnL
* Unexplained PnL
* Economic PnL
* Adjustment PnL
* Reported PnL

---

## 11.4 Exposure Dataset

Purpose:

Treasury exposure management.

Metrics:

* Quantity
* NAV
* Exposure %
* Reporting Currency

---

## 11.5 Intercompany Dataset

Purpose:

Intercompany reporting and reconciliation.

Metrics:

* Opening Balance
* Movement Value
* Interest
* FX Revaluation
* Closing Balance

---

## 11.6 Adjustment Dataset

Purpose:

Adjustment reporting and auditability.

Grain:

One record per adjustment.

---

## 11.7 Validation Dataset

Purpose:

Exception and control reporting.

Metrics:

* Severity
* Description
* Impact Value

---

## 11.8 Master Data Snapshot Dataset

Purpose:

Preserve master data used in each valuation run.

Supports historical reconstruction.

---

## 11.9 Dataset Relationships

Common keys:

* Run ID
* Version Number
* Valuation Date
* Entity
* Strategy
* Wallet
* Asset

---

## 11.10 Historical Reporting

Users shall be able to retrieve data by:

* Valuation Date
* Run ID
* Version Number

---

## 11.11 Tableau Readiness

Tableau shall consume valuation engine outputs directly.

No valuation logic shall be recreated within Tableau.

---

## 11.12 Future Datasets

Future datasets may include:

* VaR Dataset
* Stress Testing Dataset
* Liquidity Dataset
* Counterparty Risk Dataset

---

# 12. Audit & Versioning

## 12.1 Overview

The valuation engine shall maintain a complete audit trail.

---

## 12.2 Audit Principles

* Published runs immutable.
* Historical results reproducible.
* Changes traceable to users.
* Master data auditable.

---

## 12.3 Run Versioning

Multiple runs supported per valuation date.

Each run shall have a unique Run ID.

---

## 12.4 Published Versions

Published versions:

* Immutable
* Auditable
* Reproducible

Superseded versions remain available.

---

## 12.5 Current Published Version

Only one version may be designated:

Current Published Version

for a valuation date.

---

## 12.6 Reproducibility Requirement

Published runs shall be reproducible using:

* Source Data Snapshot
* Master Data Snapshot
* Adjustment Dataset
* Run Parameters

---

## 12.7 Source Data Audit

Retain:

* File Name
* Uploaded By
* Timestamp
* Record Count

---

## 12.8 Master Data Audit

Retain:

* Previous Value
* New Value
* User
* Timestamp
* Effective Dates

Version 1 does not require maker-checker approval.

---

## 12.9 Adjustment Audit

Retain:

* Adjustment ID
* Type
* Amount
* User
* Timestamp

---

## 12.10 Validation Audit

Retain:

* Validation Type
* Severity
* Description
* Impact Value

---

## 12.11 User Activity Audit

Track:

* Run Creation
* Uploads
* Validation
* Adjustments
* Approvals
* Publication
* Master Data Changes

---

## 12.12 Master Data Snapshots

A snapshot shall be created for every valuation run.

---

## 12.13 Historical Reconstruction

Users shall be able to reconstruct results using:

* Run ID
* Version Number
* Valuation Date

---

## 12.14 Retention Policy

### Published Runs

Retain for:

5 Years Active

Archive thereafter.

Published runs shall not be deleted.

---

### Draft Runs

Retain for:

2 Years

May be purged thereafter.

---

### Archived Data

Archived data shall remain retrievable.

---

## 12.15 Future Enhancements

Future releases may support:

* Maker-Checker Approval
* Electronic Sign-Off
* Digital Audit Certificates

---

# 13. Future Enhancements

## 13.1 Version 2

* Liquidity Pool Valuation
* Bank Account Integration
* Automated Effects Processing
* Automated Intercompany Interest
* Automated FX Revaluation

---

## 13.2 Version 3

* Centralised Exchange Integration
* Transaction-Level PnL
* Automated Data Ingestion

---

## 13.3 Risk Enhancements

* Historical VaR
* Parametric VaR
* Monte Carlo VaR
* Stress Testing
* Exposure Limits
* Counterparty Risk Reporting

---

## 13.4 Treasury Enhancements

* Liquidity Reporting
* Treasury Funding Reporting
* Treasury Performance Reporting

---

## 13.5 Workflow Enhancements

* Maker-Checker Controls
* Multi-Level Approval Workflows
* Electronic Sign-Off

---

## 13.6 Reporting Enhancements

* Tableau Integration
* Scheduled Reporting
* Alerting Framework

---

## 13.7 Platform Enhancements

* Data Warehouse Integration
* Data Lake Integration
* APIs
* Self-Service Reporting

---

## 13.8 Regulatory Enhancements

* Regulatory Reporting
* Compliance Reporting
* Regulatory Audit Packs

---

## 13.9 Long-Term Vision

The Treasury & Markets Valuation Engine shall evolve into a central platform supporting:

* Position Management
* Valuation
* PnL Attribution
* Exposure Management
* Treasury Reporting
* Intercompany Management
* Risk Analytics
* Management Reporting

through a single controlled and auditable source of truth.
