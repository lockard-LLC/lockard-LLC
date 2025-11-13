# File: `docs/templates/voucher_pdf_spec.md`

**Owner:** Shawn
**Visibility:** Internal/Partner

## Voucher PDF — Specification (Pilot)

## Purpose

Authorize clinic services up to a **cap (USD)** for a specific **Case ID**, with discount and invoicing terms.

## Required Fields

* **Header:** PetPass wordmark + “Community Visit Voucher”
* **Case ID:** `PFS-LEX-YYMM-####` (large, prominent)
* **Cap (USD):** e.g., `$100`
* **Expiry:** date (45 days from issue unless otherwise noted)
* **Partner Discount:** `30%`
* **Scope:** “Initial exam + basic diagnostics/medications only; other services require email pre‑approval.”
* **Billing Lines (for clinic invoice):** gross, discount %, co‑pay, **net due**, Case ID, date of service.
* **Payment Terms:** “Lockard LLC pays net‑15 on approved invoices.”
* **Contact:** [skelly@lockard.llc](mailto:skelly@lockard.llc) / [skelly@petpass.help](mailto:skelly@petpass.help) (Shawn Kelly)

## Design & Accessibility

* Paper size: US Letter (8.5×11); margin: 0.5".
* Type: Clear sans‑serif; headings ≥ 14px; body ≥ 12px.
* Color contrast: meet **WCAG 2.2 AA**.
* High‑visibility **Case ID** and **cap**.
* Optional watermark: “PetPass — Case ID only”.

## Optional Elements

* **QR code**: encodes a URL or text payload `PETPASS|<CASE_ID>|CAP:<USD>|EXP:<YYYY-MM-DD>`
* **Footer note:** “No survivor names on clinic paperwork. Use Case ID only.”

## File Naming & Storage

* Filename: `Voucher_<CASE_ID>_<YYYYMMDD>.pdf`
* Storage: restricted folder per case; linked from ledger.

## Example (text preview)

```bash
PETPASS — COMMUNITY VISIT VOUCHER
Case ID: PFS-LEX-2511-0007
Cap: $100     Expiry: 2026-01-01     Partner Discount: 30%
Scope: Initial exam + basic diagnostics/medications only.
Billing: Invoice Lockard LLC with Case ID only; show gross, discount %, co‑pay, net due. Payment net‑15.
Contact: skelly@lockard.llc / skelly@petpass.help (Shawn Kelly)
[QR: PETPASS|PFS-LEX-2511-0007|CAP:100|EXP:2026-01-01]
```
