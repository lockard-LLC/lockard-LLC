# File: `docs/company/overview.md`

**Owner:** Jerry
**Visibility:** Public summary + Internal full

## Lockard LLC — Company Overview

> **Elevator pitch (public)**
> Lockard LLC builds practical tools that keep families and their companion animals together. **PetPass** coordinates discounted, fast-access veterinary care; **dvoGPT** is our automation engine that will eventually run PetPass workflows end‑to‑end.

## Mission

Keep families and companion animals together by coordinating affordable, rapid vet care—without forcing separation.

## Values

* **Dignity & Safety:** Survivor safety and privacy come first.
* **Do the small things well:** Tiny pilot, strong learning loops.
* **Access & Inclusion:** WCAG‑compliant public surfaces; neutral language.
* **Evidence over ego:** Measure outcomes and adjust.

## Two‑lane model

* **Lane 1 (Charitable Program):** PetPass pilot with clinic discounts + micro‑vouchers, delivered in partnership with a **fiscal sponsor** (Model C) when grants/charitable funds are used. Lockard LLC provides services at market‑rate under a services agreement.
* **Lane 2 (For‑profit Tech):** dvoGPT automation, staff dashboards, and tooling. Licensable to sponsors, clinics, and other programs.

## What we do (today)

* Operate a **6‑week Lexington pilot**: 1 clinic, **3 vouchers**, **$100 cap**, **30% discount**, **$0–$10 co‑pay**.
* Provide a **case‑ID voucher** and pay clinic invoices **net‑15**.

## Where we’re going (summary)

* Expand clinic partners (2–3) after pilot.
* Bake PetPass into dvoGPT for intake → voucher → scheduling → invoice.

## 2025 operating model (tech + ops)

- **Surfaces:** `apps/landing` (lockard.llc), `apps/petpass` (petpass.help), `data/server` (shared backend), and `pp.lockard.llc` as the PetPass knowledge hub. The prior `admin/portal/{employee,admin}` internal UI was removed and will return when the redesign is ready.
- **Shared configuration:** Firebase projects live under `firebase/landing` (Lockard + admin) and `firebase/petpass` (PetPass). Every surface imports config from those folders—no inline API keys.
- **Design system:** All UIs consume the shared tokens defined in `packages/shared/tokens/` so the “quiet tech” aesthetic stays consistent across landing, portals, and PetPass.
- **Control hub:** Admin work now centers on `admin.lockard.llc`, a glassmorphic command center that exposes training, tooling, analytics, and theme control inside one surface (documented in `docs/tech/admin_portal_design.md`).

## 2025 priorities

1. **Stabilize the shared backend (`data/server`)** so landing, PetPass, and portals rely on one API for auth, staff data, vouchers, and analytics.
2. **Document every ops workflow** inside `docs/ops/**` and sync with PetPass memory to keep staff onboarding fast.
3. **Tokenize everything**: colors, spacing, motion, and typography now originate from `packages/shared` and cascade through Tailwind + CSS variables. New apps must adopt those tokens on day one.
4. **Retire legacy `/pp` modes** by shipping the dedicated employee/admin portals and migrating staff to `admin.lockard.llc`.

## Contact

* Program: **petpass.help** • **[skelly@petpass.help](mailto:skelly@petpass.help)**
* Company: **lockard.llc** • **[skelly@lockard.llc](mailto:skelly@lockard.llc)**
* Staff: **pp.lockard.llc**
* General: **[hello@lockard.llc](mailto:hello@lockard.llc)**
