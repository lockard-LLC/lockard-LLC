# File: `docs/petpass/operating_manual.md`

**Owner:** Shawn
**Visibility:** Internal

## PetPass — Operating Manual (Pilot)

## 0) Definitions

* **Case ID:** `PP-LEX-YYMM-####` (e.g., `PP-LEX-2511-0007`).
* **Cap:** $100 per case.
* **Discount:** 30% partner rate.
* **Co‑pay:** $0–$10 sliding scale (collected by clinic).
* **Payment:** net‑15 on invoice with Case ID only.

## 1) Standard Flow: Intake → Voucher → Clinic

1. **Receive Intake** (form/email/text) → assign Case ID (via `data/server` intake endpoint to avoid duplicates).
2. **Safety Check**: confirm **safe channel** + subject wording.
3. **Eligibility**: self‑attestation or partner referral code; species: dogs/cats/rabbits.
4. **Clinic Match**: pick partner clinic; target slot **≤ 5 business days**.
5. **Voucher**: issue PDF (cap $100, 45‑day expiry), save to folder, log in Ledger.
6. **Intro Email (Clinic)**: scope, voucher, Case ID, contact; request confirmation.
7. **Confirm (Survivor)**: neutral confirmation + logistics (no DV terminology).
8. **Follow Visit**: status check; capture outcomes; prepare for invoice.

## 2) Voucher Template (PDF fields)

* **Case ID**
* **Cap** (USD) & **expiry date**
* **Scope:** exam + basic diagnostics/meds only
* **Discount:** 30% partner rate
* **Billing:** invoice to PetPass with Case ID only; payment net‑15

## 3) Emails (starter text)

**Clinic subject:** *Community visit — Case PP‑LEX‑2511‑0007*
**Clinic body (short):**

> We’d like to schedule an initial exam for **Case PP‑LEX‑2511‑0007**. Scope: exam + basic diagnostics/meds. Voucher cap: **$100** (expires **[DATE]**). **30% partner discount**. Invoice with **Case ID only**; payment **net‑15**. Preferred windows: [2–3 options]. Thank you. — Shawn (PetPass)

**Survivor subject:** *Your appointment details — Case PP‑LEX‑2511‑0007*
**Survivor body (short):**

> This confirms your clinic visit. Please bring the attached voucher. If you need to reschedule, reply here any time.

## 4) Ledger Entries (minimum columns)

* `case_id`, `opened_at`, `species`, `concern`, `zip`, `safe_contact`, `clinic`,
  `gross_amount`, `clinic_discount_pct`, `copay_collected`, `pfs_due`, `status`, `notes` (matches `data/server` export schema)

## 5) Invoice Reconciliation

* **Expected net** = `gross × (1 − discount)` − `co‑pay`.
* If mismatch **> $5** → email clinic for clarification.
* If match → queue for **net‑15** payment; mark **PAID**; attach invoice PDF.
* Monthly: sample 1–2 invoices to re‑check math.

## 6) Weekly/Monthly Cadence

* **Weekly:** clinic check‑in; update KPIs; export backups; purge PII > 90 days post‑close (logged via admin portal).
* **Monthly:** one‑page report (cases, days‑to‑appointment, cost/case, CSAT, wins/risks) + admin portal uptime + automation coverage stats.
