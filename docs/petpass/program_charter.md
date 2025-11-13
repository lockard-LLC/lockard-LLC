# File: `docs/petpass/program_charter.md`

**Owner:** Jerry
**Visibility:** Public summary + Internal full

## PetPass Program Charter

## Purpose

Coordinate **basic/urgent veterinary care** so survivors can keep their animals with them.

## Scope (pilot)

* **Included:** initial exam; basic diagnostics; essential medications; urgent care visits.
* **Pre‑approval required:** imaging (X‑ray/ultrasound), surgery, specialty referrals, or any costs above cap.

## Geography & Timeline

* **Lexington, KY** · **6 weeks** · start with **1 clinic**, expand to **2–3** if metrics hold.

## Financial Model (pilot)

* **Voucher cap:** $100/case
* **Clinic discount:** 30% partner rate
* **Co‑pay:** $0–$10 sliding scale
* **Payment:** net‑15 on invoice with **Case ID only**

## Case IDs

Format: `PP-LEX-YYMM-####` (e.g., `PP-LEX-2511-0001`). Case IDs appear on clinic‑facing documents; **no survivor names**. The shared backend enforces this pattern and rejects manual edits in spreadsheets/portals.

## Eligibility (pilot)

* Self‑attestation of need or partner referral code.
* Species: **dogs, cats, rabbits**.

## Success Metrics

* **Access:** ≥80% of cases scheduled **≤5 business days**.
* **Affordability:** Average PetPass outlay **≤ $120** per case.
* **Satisfaction:** Clinic + survivor CSAT **≥ 4/5**.
* **Safety:** **Zero privacy incidents**.

## Risks & Safeguards

* Capacity (clinic slots) → pre‑book weekly PFS blocks.
* Scope creep → require pre‑approval for imaging/surgery.
* Privacy → case IDs, neutral language, staff training, single backend (`data/server`) mediating all reads/writes.

## Technology alignment

- Public site lives at `apps/petpass` (Next.js, hosted via `firebase/petpass`).
- Staff/operations use the Lockard Admin & Employee portals built on shared tokens + `data/server`.
- Firebase configuration never lives inside the app—import from `firebase/petpass/config.js`.
