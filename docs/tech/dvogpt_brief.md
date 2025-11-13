# File: `docs/tech/dvogpt_brief.md`

**Owner:** Jerry
**Visibility:** External collaborators

## dvoGPT — Technical Brief (v0)

**Concept:** An autonomous browser agent that executes multi‑step web tasks from natural language—plan, see, click, type—using Playwright, vision (OCR), and LLM planning. **PetPass module** automates intake → voucher → clinic scheduling → invoice reconciliation.

## Architecture (high level)

* **NL Interface:** parse task → action graph; ask clarifying questions.
* **Visual Browser Understanding:** screenshot analysis; element detection; layout reasoning; OCR fallback.
* **Planning Engine:** goal decomposition; conditional strategies; retries/backoff.
* **Execution Layer:** Playwright‑controlled browsers; multi‑tab; downloads/uploads.
* **Learning & Memory:** workflow recording; pattern reuse; knowledge base (site logins, form fields).
* **Safety:** approval workflows for sensitive actions; audit logs; rate limiting; sandboxed containers.

## Tech

* **Backend:** Node/Python
* **Automation:** Playwright
* **Vision:** OpenCV + Tesseract
* **LLM:** GPT‑vision + text
* **Infra:** Docker, Postgres, Redis, MQ (e.g., RabbitMQ)
* **Integration points:** `data/server` (to read/write PetPass data), Firebase (case metadata), admin portal (surface workflows/results)

## Phases

1. Core automation → single‑page flows.
2. Intelligence layer → LLM planning + visual understanding.
3. Learning system → record/replay, pattern mining.
4. Scale → multi‑agent orchestration, isolation.
5. Production → dashboard, auth, resilience.

## Success Metrics

* Task completion rate; time saved vs manual; extraction accuracy; workflow reuse; user satisfaction.
* Direct contribution to PetPass ops: % of vouchers scheduled autonomously, reduction in admin portal task backlog, number of safe retries logged without human escalation.
