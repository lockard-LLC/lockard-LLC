# File: `docs/company/vision_roadmap_3y.md`

**Owner:** Jerry
**Visibility:** Internal (public summary allowed)

## Lockard LLC — Vision & 3‑Year Roadmap

## Vision

Keep families and companion animals together by coordinating fast, affordable, privacy‑respecting veterinary care—scaled by humane automation.

## Strategic Lanes

1. **PetPass Program** — deliver measurable impact via clinic partnerships, vouchers, and safety‑first operations.
2. **dvoGPT Platform** — build an autonomous browser agent to automate PetPass workflows and generalize to other social‑impact tasks.

## Milestones & Timeline

### 0–6 months (Pilot & Foundations)

* Run **Lexington pilot** (1 clinic, 3 vouchers, $100 cap, 30% discount).
* Publish public pages: privacy, terms, accessibility (hosted via `apps/landing` + `firebase/landing`).
* Land a **fiscal sponsor (Model C)**; align reporting & payment flows.
* Build **admin.lockard.llc** control hub (MVP): intake, vouchers, slots, invoices, metrics, onboarding lounge.
* dvoGPT **Phase 1**: Playwright automation + screenshot capture + action library.
* Stand up the shared backend at `data/server` and the design-token system in `packages/shared`.
* KPIs: ≥80% scheduled ≤5 business days; avg outlay ≤$120; CSAT ≥4/5; zero privacy incidents.

### 6–12 months (Operational Readiness)

* Expand to **2–3 clinics**; refine micro‑MOU and voucher rules.
* Secure **seed grants** via fiscal sponsor; add local donors.
* dvoGPT **Phase 2**: LLM planning + OCR + page‑state recognition; record/replay workflows.
* Dashboard v1.1: audit trails, role permissions, CSV import/export.
* Document **Incident Response** drills; quarterly accessibility review.

### 12–24 months (Scale & Automation)

* Add **2 more markets** (small cohorts) with local partners.
* Establish a **scholarship fund** line with sponsors; pilot higher cap for complex cases w/ pre‑approval.
* dvoGPT **Phase 3–4**: knowledge base, pattern reuse, multi‑agent orchestration.
* Integrations: email API, calendar slots, accounting export, metrics dashboard fed from `data/server`.
* Evaluate **SBIR/STTR** feasibility for dvoGPT modules.

### 24–36 months (Proof & Partner Network)

* Formalize **PetPass Partner Network** with certification and best practices.
* Publish outcomes report (access, affordability, satisfaction, safety).
* dvoGPT **Phase 5**: production hardening, auth, resilience; PetPass flows fully automated with human review gates.
* Explore licensing to clinics/sponsors or white‑label deployments leveraging the Lockard design system and backend APIs.

## Resourcing (indicative)

* **People:** CEO (Jerry), VOC (Shawn), part‑time engineer/contractor, fiscal sponsor ops liaison.
* **Budget:** pilot outlay $500 + tools; grants/donations via sponsor; modest hosting.

## Risks & Mitigations

* **Clinic capacity** → pre‑book slots; broaden partner list.
* **Privacy incidents** → strict Case ID usage, staff training, runbook.
* **Funding gaps** → fiscal sponsor pipeline, small‑dollar donors, program‑related investment (PRI) education.
* **Tech fragility** → staged rollouts; error budgets; manual fallback.

## Success Indicators

* Time‑to‑appointment, cost/case, CSAT (clinic + survivor), zero privacy incidents.
* Automation coverage of PetPass workflows (≥70% by Year 3).
* Sustainable funding mix with clear impact reporting.
