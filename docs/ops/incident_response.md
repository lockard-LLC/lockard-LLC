# File: `docs/ops/incident_response.md`

**Owner:** Jerry
**Visibility:** Internal

## Incident Response Runbook (Privacy/Safety)

## Severity Levels

* **SEV‑1:** Active risk to survivor privacy/safety.
* **SEV‑2:** Exposure of limited data; no immediate risk.
* **SEV‑3:** Process/policy violation with no exposure.

## First Actions (any SEV)

1. **Pause** related communications.
2. **Record** facts: what, when, who, where (no speculation).
3. **Notify** CEO (Jerry) immediately.
4. Create Case Incident ID: `IR-LEX-YYMM-####`.
5. Capture relevant portal logs / `data/server` request IDs so engineering can trace access.

## Containment

* Remove/lock access to affected docs/folders.
* Request deletion/recall from unintended recipients if applicable.
* Coordinate with clinic partner if they were involved.

## Communication

* Internal: concise timeline + decisions in the incident doc.
* External: survivor/clinic notifications **only** with CEO approval; use neutral language.

## Remediation

* Redact/rotate credentials if needed.
* Update templates and checklists to prevent recurrence.
* Schedule refresher training if staff error; update the admin portal onboarding module.

## Post‑mortem (within 7 days)

* Summary, timeline, root causes, corrective actions, owners, due dates, plus links to relevant `docs/` updates or memory entries.
* Close when actions verified.
