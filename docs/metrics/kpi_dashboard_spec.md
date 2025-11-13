# KPI Dashboard Spec

**Owner:** Jerry • **Visibility:** Public (high‑level) + Internal (detail)

**Metrics:** Access (days‑to‑appt), Affordability (PetPass outlay), Satisfaction (CSAT), Safety (incidents), Portal Health (uptime, onboarding completion), Automation Coverage (% flows handled by dvoGPT).  
**Data sources:** `data/server` exports, ledger CSV, vouchers CSV, survey forms (scores 1–5), admin portal telemetry.  
**Transforms:** rolling averages; outlier flags (>95th percentile).  
**Charts:** single‑value cards; lines; bar by clinic/species; stacked bars for automation vs manual.  
**Cadence:** weekly updates; monthly export for sponsors.
