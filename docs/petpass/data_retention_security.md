# Data Retention & Security Controls

**Owner:** Jerry • **Visibility:** Internal

**Retention:** Active cases = needed; Closed = PII purge at 90 days; Finance = 7 years (Case ID only).  
**Backups:** weekly encrypted; verify quarterly; restore test semiannual.  
**Access control:** least privilege; password manager; 2FA; roles (Admin, VOC, Read‑only). Admin + VOC permissions flow through `data/server`; no direct Firebase Admin access from clients.
**Storage:** segregate PII vs billing; encrypt at rest when available.  
**Transport:** TLS everywhere; redact names in attachments sent to partners.  
**Logging:** audit actions by Case ID; access logs retained 1 year; `data/server` emits structured logs with request IDs for cross-system tracing.
**Hardening:** quarterly review; rotation of DKIM selectors; remove stale accounts within 24h.
**Config source:** Firebase settings housed under `firebase/landing` (admin) and `firebase/petpass` (program); rotate API keys via CI secrets, not hardcoded env files.
