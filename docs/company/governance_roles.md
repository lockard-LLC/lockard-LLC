# Governance & Roles

**Owner:** Jerry • **Visibility:** Internal

## Titles & Areas

- **CEO (Jerry):** strategy, partnerships, tech direction, compliance, budget, hiring.
- **VOC (Shawn):** clinic outreach, intake triage, vouchers, scheduling, invoices, metrics, admin portal content.
- **Advisors/Contractors:** legal, accounting, accessibility, engineering (as engaged).
- **Engineering (Codex + partners):** shared backend (`data/server`), design tokens, PetPass + landing UIs, Firebase governance.

## Decision Rights

- CEO: exceptions to scope/cap; partner selection; policy; security incidents; spending > $250; approval of design-token changes and Firebase project access.
- VOC: vouchers ≤ $100; clinic scheduling; invoice reconciliation; routine comms; approves content additions to `admin.lockard.llc`.
- Engineering: proposes architecture/token changes, owns CI approvals, blocks deploys that break WCAG/security requirements.

## Escalation Path

Safety/privacy → CEO immediately. Finance disputes > $50 → CEO. Clinic exceptions → CEO.
Backend availability (data/server) → engineering lead; UI regressions → WebUX; doc drift → DocsAgent.

## RACI (examples)

- Micro‑MOU: R(CEO) A(CEO) C(VOC) I(Clinic)
- Voucher spec: R(VOC) A(CEO) C(Clinic) I(Staff)
- Incident response: R(CEO) A(CEO) C(VOC) I(Clinic)
- Admin portal release: R(WebUX/Engineering) A(CEO) C(VOC) I(Staff)
- Firebase rules update: R(Engineering) A(CEO) C(VOC) I(Partners)
