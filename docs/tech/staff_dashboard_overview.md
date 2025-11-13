# File: `docs/tech/staff_dashboard_overview.md`

**Owner:** Jerry
**Visibility:** Internal

## Staff Dashboard (admin.lockard.llc) — Overview

## Goals

* One place to manage **intakes, vouchers, clinic slots, invoices, and metrics**.
* Minimal PII; **Case ID first**; all actions **audited**.

## Key Features

* **Intake Manager**: view/triage; assign Case ID; mark safe channel; capture minimal notes; powered by `data/server`.
* **Voucher Maker**: generate PDF; email to clinic; attach to case; logs live in Firebase via shared backend.
* **Clinic Scheduler**: show partner slots; propose/confirm times; reminders; tokens ensure consistent UI.
* **Invoice Center**: upload/check math; compute expected net; approve for payment; export to accounting.
* **Metrics Panel**: Access, Affordability, Satisfaction, Safety sourced from the metrics data model.

## Roles & Permissions

* **VOC (Shawn):** full on cases, vouchers, scheduling, invoices via employee portal context.
* **Admin (Jerry):** everything + configuration (Firebase keys, design tokens, staff onboarding).
* **Read‑only:** transparency metrics only (future partners/fiscal sponsor dashboards).

## Audit & Safety

* Every action writes an **audit record** (who/what/when) via `data/server`.
* No survivor names in audit; link by Case ID.
* Log access to sensitive attachments.

## Privacy by Design

* Clinic comms generated from templates (neutral wording).
* Case ID enforced in all outbound docs.
* PII storage segregated; time‑boxed retention tasks automated through the backend.

## Integrations (later)

* Outbound email (SMTP/API), calendar (slots), accounting export (CSV), dvoGPT hooks for automation.
