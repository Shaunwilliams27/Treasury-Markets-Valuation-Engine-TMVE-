# Treasury & Markets Valuation Engine (TMVE)

## Business Requirements Document (BRD)

### Version

1.0

### Status

Draft – Business Review

### Document Owner

Head of Global Treasury & Markets

### Prepared By

Treasury & Markets

### Date

June 2026

---

# 1. Executive Summary

The Treasury & Markets division currently produces a daily PnL Flash report using manually extracted data from operational systems and spreadsheet-based calculations.

The current process relies heavily on manual intervention, including data extraction, spreadsheet updates, intercompany adjustments, reconciliations, and report generation. As the business continues to expand across multiple entities, assets, wallets, jurisdictions, and reporting requirements, the current process presents operational, audit, scalability and key-person dependency risks.

The objective of the Treasury & Markets Valuation Engine (TMVE) is to provide a controlled, auditable, repeatable and scalable platform for calculating daily positions, valuations, PnL and supporting management reporting.

Version 1 will focus on wallet-based asset valuation and PnL reporting.

The Treasury & Markets Valuation Engine is intended to support management reporting for the Mint, Markets and Treasury ("MMT") business segment and is not intended to produce statutory or full legal group reporting.

---

# 2. Business Objectives

The Treasury & Markets Valuation Engine shall:

* Automate the daily valuation process.
* Reduce spreadsheet dependency.
* Improve auditability and governance.
* Support re-running historical valuations.
* Support version-controlled flash reporting.
* Provide a trusted source of Treasury and Markets reporting data.
* Provide a data source for future Tableau dashboards.
* Provide a foundation for future risk analytics and Value-at-Risk calculations.
* Support month-end reconciliation processes with Finance.
* Support future expansion into liquidity pools, bank accounts and centralised exchange balances.

---

# 3. Business Problem Statement

The current process requires manual extraction and manipulation of data from multiple sources.

Key challenges include:

* Manual spreadsheet processing.
* Limited audit trail.
* Manual version management.
* Limited ability to re-run historical valuations.
* Manual intercompany adjustments.
* Increasing operational complexity as new entities and assets are added.
* Difficulty scaling reporting requirements.
* Key-person dependency.
* Limited reporting flexibility.

---

# 4. Scope

## 4.1 In Scope (Version 1)

### Source Data

* Wallet balances.
* Exchange rates.
* Master reference data.

### Valuation

* Position calculation.
* Net Asset Value (NAV) calculation.
* Trading PnL calculation.
* Revaluation PnL calculation.

### Reporting

* Daily PnL.
* Month-to-Date (MTD) PnL.
* Year-to-Date (YTD) PnL.

### Reporting Currencies

* South African Rand (ZAR).
* United States Dollar (USD).
* British Pound Sterling (GBP).

### Master Data

* Legal entities.
* Wallets.
* Assets.
* Strategies.

### Controls

* Version-controlled reporting runs.
* Published flash reports.
* Audit trail.
* Historical re-runs.

### Adjustments

* Manual intercompany journals.
* Manual reconciliation journals.
* Manual approved adjustments.

### Outputs

* Daily Flash PnL.
* MMT Group PnL reporting.
* Management reporting datasets.
* Tableau reporting datasets.

---

## 4.2 Out of Scope (Version 1)

### Asset Types

* Liquidity pools.
* Fiat bank accounts.
* Centralised exchange balances.

### Automation

* Automated intercompany journal generation.
* Automated interest calculations.
* Automated ledger reconciliations.

### Risk

* Value at Risk (VaR).
* Stress testing.
* Risk reporting.

---

# 5. Reporting Perimeter

The Treasury & Markets Valuation Engine shall support reporting for entities that form part of the Mint, Markets and Treasury ("MMT") business segment.

The MMT reporting perimeter represents the scope of management reporting produced by Treasury & Markets and does not represent the full legal group structure.

The solution shall support future additions and removals of entities through master data maintenance.

---

## 5.1 MMT Reporting Entities

The initial MMT reporting perimeter shall include:

* MINTZA
* MKTSZA
* TREASZA
* MKTSUK
* GRP TREAS
* MINTUK

Additional Mint, Markets and Treasury entities may be added in future versions.

---

## 5.2 Non-MMT Entities

The solution shall support maintaining balances and intercompany relationships with entities that fall outside the MMT reporting perimeter.

Examples may include:

* ZEAMZA
* Other operating subsidiaries
* Future non-MMT entities

These entities shall not form part of standard MMT PnL reporting outputs.

However, intercompany balances involving such entities shall be maintained for reconciliation and balance sheet purposes where required.

---

# 6. Reporting Hierarchy

The solution shall support reporting at the following levels:

* MMT Group
* Entity
* Wallet
* Strategy
* Asset

Users shall be able to drill down through each reporting level.

The solution shall support aggregation and reporting at any level within the hierarchy.

Strategy shall be assigned at wallet level through master data configuration.

The initial strategy classifications shall include:

* OTC Trading
* Liquidity Management
* Automated Market Making
* Liquidity Pools
* Treasury
* General Wallets

Additional strategy classifications may be added in future through master data maintenance.

---

# 7. PnL Methodology

The solution shall calculate:

### Trading PnL

Changes in position quantities between reporting dates.

### Revaluation PnL

Changes in asset value resulting from exchange rate movements.

### Economic PnL

Trading PnL plus Revaluation PnL.

### Reported PnL

Economic PnL adjusted for approved intercompany, reconciliation and other approved adjustments.

### MMT Group PnL

Consolidated reported PnL across all entities within the MMT reporting perimeter.

---

# 8. Intercompany Requirements

The solution shall support recording, maintaining and reporting intercompany balances.

Intercompany balances shall support:

* Receivables.
* Payables.
* Counterparty tracking.
* Asset-level balances.
* Historical balance tracking.
* Auditability of adjustments.

Version 1 shall support manual intercompany journals.

Future versions shall support automated intercompany movement generation through source transaction data.

All intercompany relationships shall be routed through GRP TREAS in accordance with Treasury policy.

The solution shall support intercompany balances between:

* MMT entities.
* Non-MMT entities.

Intercompany balances with non-MMT entities shall be maintained for balance sheet and reconciliation purposes but shall not result in inclusion of those entities within standard MMT management reporting outputs.

---

# 9. Month-End Requirements

The solution shall support month-end reconciliation processes between Treasury and Finance.

The solution shall:

* Retain historical daily balances.
* Support reconciliation to the General Ledger.
* Support recording reconciliation journals.
* Support recording intercompany interest journals.
* Maintain auditability of all adjustments.

Historical source data shall remain unchanged.

All adjustments shall be recorded separately from source data.

The General Ledger shall remain the accounting system of record.

---

# 10. Audit and Governance Requirements

The solution shall:

* Maintain a complete audit trail.
* Record all valuation runs.
* Support multiple versions of a valuation date.
* Record creation timestamps.
* Record approval timestamps.
* Record adjustment history.
* Support historical reconstruction of published reports.

Published reports shall remain immutable.

Subsequent corrections shall be recorded as new versions.

Historical source data shall never be overwritten.

---

# 11. Users

## Front Office

### Trading Assistant

Responsibilities:

* Execute valuation runs.
* Review validation checks.
* Submit reports for approval.

### Trading Manager

Responsibilities:

* Review valuation results.
* Approve flash reports.
* Approve adjustments.
* Approve publication of valuation runs.

## Finance

Responsibilities:

* Month-end reconciliation.
* Ledger alignment.
* Intercompany review.
* Validation of reconciliation adjustments.

---

# 12. Success Criteria

The project shall be considered successful when:

* Daily flash reporting is automated.
* Manual spreadsheet dependency is significantly reduced.
* Historical valuations can be reproduced.
* Published reports are version controlled.
* PnL calculations reconcile to existing approved methodology.
* Month-end reconciliation processes are supported.
* Tableau reporting datasets are available.
* MMT Group reporting can be produced consistently and accurately.
* The platform can support future enhancements without redesign.

---

# 13. Future Roadmap

## Version 2

* Effects file integration.
* Automated intercompany journal generation.
* Intercompany balance automation.

## Version 3

* Liquidity pool valuation.
* Liquidity pool PnL attribution.

## Version 4

* Bank account integration.
* Centralised exchange integration.
* Automated intercompany interest calculations.
* Reconciliation workflow enhancements.

## Version 5

* Value at Risk (VaR).
* Risk analytics.
* Stress testing.
* Exposure reporting.
* Advanced Treasury and Markets reporting.
