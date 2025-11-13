# File: `docs/compliance/dns_email_auth.md`

**Owner:** Jerry
**Visibility:** Internal

## DNS & Email Authentication — petpass.help / lockard.llc (Pilot)

> Choose your mail provider’s specific values. Examples below include **Google Workspace** presets; replace placeholders as needed.

## 1) SPF (TXT)

* **Google Workspace:**
  Host: `@` → `v=spf1 include:_spf.google.com ~all`
* **Generic custom SMTP:**
  `v=spf1 ip4:<SENDER_IP> include:<ESP_DOMAIN> ~all`

## 2) DKIM (TXT)

* Generate keys in your mail admin. Publish:
  Host: `<selector>._domainkey`
  Value: `v=DKIM1; k=rsa; p=<PUBLIC_KEY>`
* Example (Google default selector):
  `google._domainkey` → `v=DKIM1; k=rsa; p=<PUBLIC_KEY>`

## 3) DMARC (TXT)

* Host: `_dmarc`
* Value (start in monitor → tighten):
  `v=DMARC1; p=quarantine; rua=mailto:dmarc@lockard.llc; ruf=mailto:dmarc@lockard.llc; fo=1; aspf=s; adkim=s; sp=reject; pct=100`

## 4) MX

* Point to your provider (e.g., Google MX):
  `ASPMX.L.GOOGLE.COM.` (priority 1) and secondaries.

## 5) Optional Hardening (later)

* **MTA‑STS:** `mta-sts.<domain>` A/AAAA + TXT `mta-sts` + HTTPS policy file.
* **TLS‑RPT:** `_smtp._tls.<domain>` TXT → `v=TLSRPTv1; rua=mailto:tlsrpt@lockard.llc`.
* **BIMI:** `default._bimi.<domain>` TXT with brand logo (SVG) + DMARC `p=quarantine` or `reject`.

## 6) Records per Domain

* Configure **both**: `lockard.llc` and `petpass.help` (emails like `skelly@lockard.llc` and `skelly@petpass.help`).
* Maintain separate DKIM selectors/keys; rotate quarterly.
