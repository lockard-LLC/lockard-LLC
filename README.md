# 🧭 Lockard LLC

The **Lockard LLC workspace** is a memory-first environment for PetPass, internal portals, and R&D initiatives like dvoGPT. It unifies human operators and Codex agents under a consistent Linux-style layout (WSL/macOS/cloud friendly) with Codex enforcing truth-first development.

Automation, documentation, and app code all share this repository so changes to policy or UI stay in sync with the Codex knowledge graph.

---

## 🧱 Workspace Layout

```bash
lockard-llc/
├── apps/                 # Product apps (PetPass, dvoGPT, internal dashboards)
│   ├── README.md         # Catalog of current/future apps
│   └── petpass/          # PetPass web app (+ ops tooling) → see apps/petpass/README.md
│
├── admin/portal/         # Internal portals + shared API
│   ├── server/           # Lockard staff API (Next.js, port 4050)
│   ├── employee/         # Employee classroom/onboarding UI
│   └── admin/            # CEO dashboard + lesson management
│
├── packages/shared/      # Reusable components, contracts, utilities
│
├── docs/                 # Canonical documentation (handbook, partners, tech)
├── ops/                  # Policies, checklists, compliance templates
├── codex/                # Memory/, context/, tasks/, config/ for agents
├── scripts/              # Automation scripts (bundle-context, memory-update, docpack)
├── data/                 # Sanitized data drops
├── var/                  # Logs, scratch space
├── srv/                  # Future hosting/static assets
├── opt/                  # Optional tooling
├── bin/                  # CLI wrappers (e.g., `bin/lockard`)
├── etc/                  # System configs (mirrors Linux hierarchy)
└── ~/.hidden/            # Stash for tool caches (.cache, .local, .npm, .config, .vscode-server symlink here)
```

> See [`codex/config/RULES.md`](codex/config/RULES.md) and [`AGENTS.md`](AGENTS.md) for memory precedence, safety rules, and automation etiquette.

---

## 🧩 Core Concepts

### Context-First Development

- Agents and humans read `codex/memory/` first, then docs, then generated context.
- Conflicts between docs and memory are **escalated**, not overwritten.
- Every change lands in `codex/memory/99_changelog.md` for traceability.

### Codex Bundle

- `scripts/bundle-context.js` produces `codex/context/bundle.json`, merging memory, docs, and config into a single knowledge graph for automation or manual review.

### Session Hygiene

```bash
lockard-session-begin
…work…
lockard-session-end
lockard-bundle
```

- Sessions capture TODOs, open questions, and file diffs so nothing is lost.

### Visibility & Safety

- Every doc starts with `Owner` + `Visibility` (Public | Partner-only | Internal).
- No PII; Case IDs only (`PP-LEX-YYMM-####` for PetPass).
- Net-new policies go in `docs/` and must update `codex/memory/`.

---

## 🧠 Product & Portal Overview

- **PetPass** (public app): `apps/petpass/` — [See dedicated README](apps/petpass/README.md).
- **Lockard Internal Portals**: `admin/portal/{server,employee,admin}` — shared staff API, classroom UI, and CEO dashboards.
- **dvoGPT R&D**: prototyping area under `apps/` (documented in `apps/README.md` as projects spin up).

All apps share themes, packages, and Codex memory so UI updates reflect policy, and documentation stays in lockstep with tooling.

---

## ⚙️ Tooling & Environment

- **Node 20+**, Corepack-enabled pnpm/yarn, and Zulu/Temurin 17 for Java when running Android tools.
- `.bashrc` exports `LOCKARD_STASH=~/.hidden` to keep `$HOME` clean and provides helpers:
  - `lockard bundle` → run `scripts/bundle-context.js`
  - `lockard web` → run PetPass dev server
  - `lockard server` → run the shared API (`admin/portal/server`, port 4050)
  - `lockard-session-begin` / `lockard-session-end`

The shell also auto-rebuilds symlinks for `.cache`, `.local`, etc. so stray folders are removed every time you source the profile.

---

## 🛡️ Safety, Privacy, Retention

- No survivor or clinic PII in code, docs, or logs.
- Neutral language in all public/partner materials.
- Purge PII 90 days after case closure; keep financials (Case ID only) for 7 years.
- Escalate safety/privacy issues or spending > $250 to the CEO.
- Respect safe-contact instructions when communicating externally.

---

## 🚀 Philosophy

1. **Memory First** — Codex and humans share the same source of truth.
2. **Safety by Default** — Privacy, language, and retention policies are non-negotiable.
3. **Automation with Oversight** — Every agent action is explainable and reversible.

For program-specific details, follow links within this README:

- [PetPass README](apps/petpass/README.md)
- [Apps catalog](apps/README.md)
- [Project brief](PROJECT.md)
