# File: `docs/ops/data_schema_templates.md`

**Owner:** Shawn
**Visibility:** Internal

## Data Schema & Templates (Pilot)

## 1) Ledger CSV (canonical)

Columns:
`case_id,opened_at,species,concern,zip,safe_contact,clinic,gross_amount,clinic_discount_pct,copay_collected,pfs_due,status,notes`

Example row:
`PP-LEX-2511-0001,2025-11-09,rabbit,head tilt,40508,email:alias@proton.me,Clinic TBD,200,30,10,130,OPEN,Pilot example`

## 2) Vouchers CSV

Columns:
`voucher_code,cap_usd,expires_on,status,case_id`

Example rows:
`PP-LEX-2511-0001,100,2026-01-01,UNUSED,`
`PP-LEX-2511-0002,100,2026-01-01,UNUSED,`

## 3) Clinics CSV

Columns:
`clinic_name,contact_email,contact_phone,discount_pct,pfs_slots_per_week,billing_notes`

## 4) Cases JSON (internal use)

```json
{
  "case_id": "PFS-LEX-2511-0001",
  "project": "petpass",
  "opened_at": "2025-11-09",
  "species": "rabbit",
  "concern": "head tilt",
  "zip": "40508",
  "safe_contact": "email:alias@proton.me",
  "clinic": "Clinic TBD",
  "voucher": { "cap_usd": 100, "expires_on": "2026-01-01" },
  "financials": { "gross": 200, "discount_pct": 30, "copay": 10, "pfs_due": 130 },
  "status": "OPEN",
  "notes": "Pilot example"
}
```

## 5) Validation Rules (min.)

* `case_id` must match `^PP-LEX-\d{4}-\d{4}$` (program code + region + YYMM + 4-digit counter).
* `clinic_discount_pct` ∈ {20–50}.
* `cap_usd` ≤ 100 (pilot).
* `pfs_due` = `round(gross × (1 − discount) − copay, 2)`, min 0.
* `status` ∈ {OPEN, SCHEDULED, VISITED, PAID, CLOSED}.
* All data entry happens through `data/server` endpoints; spreadsheets mirror the source of truth for quick exports only.
