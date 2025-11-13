# File: `docs/handbook/employee_handbook.md`

**Owner:** Jerry
**Visibility:** Internal (public summary allowed)

## Lockard LLC — Employee Handbook (Pilot‑scale)

> This handbook sets expectations for a small, pilot‑scale team. It is not legal advice and does not create a contract of employment. Adapt as headcount grows.

## 1) Culture & Conduct

* We lead with **dignity, safety, and inclusion**.
* No harassment or discrimination.
* Treat survivor information with extreme care; follow **Case ID** practices.

## 2) Equal Opportunity & Anti‑Harassment

* We do not discriminate based on protected characteristics.
* Report issues to CEO; we act promptly and fairly.

## 3) Remote Work & Hours

* Pilot cadence favors **daytime ET** availability.
* Use agreed tools (email, Google Voice, Sheets, admin.lockard.llc).
* Keep status notes in the ledger or task tracker; log blockers in the portal’s Training → Daily Ops tab.

## 4) Data Protection

* Store PII separately from billing; restrict access; use PetPass case IDs inside `data/server`.
* Use password manager + 2FA.
* Purge PII **90 days** after case closure; financial records **7 years** (Case ID only).

## 5) Safety & Communications

* Neutral language in clinic/survivor comms.
* Confirm safe contact channels; honor no‑voicemail/text preferences.

## 6) Expenses & Purchases (Pilot)

* Pre‑approved budget only (voucher cap **$100**; 30% discount; co‑pay $0–$10).
* Invoices paid **net‑15** after math check; attach PDFs to ledger.

## 7) Issues & Incidents

* Use the **Incident Response Runbook** for any suspected privacy/safety issue.
* When unsure, **pause and escalate**.

## 8) Feedback & Updates

* Monthly retro: what worked, what didn’t, proposed changes.
* Handbook updates logged in changelog and mirrored into the employee portal knowledge base.

## 9) Tooling snapshot

- **Admin Portal:** admin.lockard.llc (glass UI, command palette, staff onboarding lounge).
- **Shared backend:** data/server (Express + Firebase Admin). Staff do not use service accounts directly.
- **Design system:** packages/shared/tokens/**; all UI work must adopt these tokens for consistency across landing, PetPass, and portals.
