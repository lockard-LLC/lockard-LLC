# 🧭 Lockard LLC

The **Lockard LLC** is a memory-first environment for PetPass, internal portals, and R&D initiatives like dvoGPT. It unifies human operators and Codex agents under a consistent Linux-style layout (WSL/macOS/cloud friendly) with Codex enforcing truth-first development.

Automation, documentation, and app code all share this repository so changes to policy or UI stay in sync with the Codex knowledge graph.

---

## 🧱 Workspace Layout

```bash
lockard-llc/
├── apps/                 # Product apps (PetPass, dvoGPT, internal dashboards)
│   ├── README.md         # Catalog of current/future apps
│   └── petpass/          # PetPass web app (+ ops tooling) → see apps/petpass/README.md
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
- **Lockard Internal Portal (next-gen)**: prior `admin/portal/{server,employee,admin}` workspaces were removed on 2025-11-12; the next iteration will be re-scaffolded once the new UX brief lands.
- **dvoGPT R&D**: prototyping area under `apps/` (documented in `apps/README.md` as projects spin up).

All apps share themes, packages, and Codex memory so UI updates reflect policy, and documentation stays in lockstep with tooling.

---

## ⚙️ Tooling & Environment

- **Node 20+**, Corepack-enabled **Yarn 4** (node_modules linker), and Zulu/Temurin 17 for Java when running Android tools.
- `.bashrc` exports `LOCKARD_STASH=~/.hidden` to keep `$HOME` clean and provides helpers:
  - `lockard bundle` → run `scripts/bundle-context.js`
  - `lockard web` → run PetPass dev server
  - `lockard server` → run the shared API (`data/server`, port 4050)
  - `lockard-session-begin` / `lockard-session-end`

The shell also auto-rebuilds symlinks for `.cache`, `.local`, etc. so stray folders are removed every time you source the profile.

### Scripts (automation)

This repo contains lightweight automation scripts in `scripts/` intended to be safe, readable, and easy to run locally or in CI. Key scripts and their purpose:

- `scripts/bundle-context.js` — compiles `codex/memory/` and `docs/` into `codex/context/bundle.json` used by agents and automation. Run as part of session end or CI to produce the latest knowledge bundle.
- `scripts/memory-update.js` — helper utilities for session harvesting and writing structured session notes into `codex/memory/sessions/`.
- `scripts/apply-docpack.js` — splits heredoc docpacks into files under `docs/` for bulk documentation imports.
- `scripts/autobundle-daemon.sh` — a small watcher that rebundles the Codex bundle on file changes (default cadence ≈2m). It's safe to run in a local development machine or a lightweight VM.

Typical usage examples:

```bash
# create the codex bundle (one-off)
node scripts/bundle-context.js

# run autobundle in a background terminal for fast local feedback
./scripts/autobundle-daemon.sh &
```

CI integration

- The bundle script should be part of any automation that needs the latest memory/docs (e.g., pre-run for any agent-based job).
- Keep `bundle-context.js` fast and deterministic; prefer Node-only libs for portability.

### Yarn workspace shortcuts

Root-level Yarn scripts fan out across workspaces so you can stay in `/home/jerry/lockard-llc`:

| Command | Description |
| --- | --- |
| `yarn install` | Installs all workspace deps (e.g., `data/server`). |
| `yarn dev` / `yarn dev:server` | Runs `data/server` on port 4050 via `yarn workspace lockard-server dev`. |
| `yarn build` | Builds the shared staff API workspace (`lockard-server`). |
| `yarn lint` | Runs `yarn workspace lockard-server lint`. |
| `yarn test` | Reserved for future workspaces with a `test` script (currently no-op). |

Use `yarn workspace <name> <script>` for package-specific commands (e.g., `yarn workspace lockard-server lint`).

## 🚀 Quickstart (developer)

Copy these commands to get a local dev environment running (Linux-like shell):

```bash
# install dependencies (Corepack enabled)
yarn install

# create the Codex bundle used by agents and automation
node scripts/bundle-context.js

# run autobundle in background (developer convenience)
./scripts/autobundle-daemon.sh &

# start the staff API (server workspace)
yarn workspace lockard-server dev
```

- Notes:

- Use `AUTOBUNDLE_CADENCE` or `AUTOBUNDLE_LOG` env vars to configure `autobundle-daemon.sh`.
- For CI, prefer `node scripts/bundle-context.js --once` (or `--ci`) in your workflow so bundle runs deterministically.

## 🛠️ Common tasks

- Rebundle after editing memory/docs:
  - `node scripts/bundle-context.js`
- Run a one-shot autobundle (CI/cron):
  - `./scripts/autobundle-daemon.sh --once`
- Run firebase emulators (per-project folder):
  - `cd firebase/petpass && npx firebase emulators:start`

## 🤝 Contributing / PR checklist

Before you open a PR, please:

1. Run tests and linters locally: `yarn test && yarn lint`.
2. Run `node scripts/bundle-context.js` if you modified `codex/memory/` or `docs/` and include the updated bundle only when automation depends on it.
3. Keep PRs small and focused (target ≤200 LOC). Explain the developer contract in the PR description: inputs, outputs, error modes, and any migration steps.
4. Use Conventional Commits for commit messages and include a short changelog note if you updated memory/docs.
5. If your change affects architecture or policy surfaces, add an ADR under `docs/tech/adr/`.

## 🐞 Troubleshooting

- If `bundle-context.js` fails, run it with `DEBUG=1 node scripts/bundle-context.js` to see stack traces.
- If `autobundle-daemon.sh` isn't picking up changes, ensure `inotify-tools` is installed for event-driven mode, or set a shorter `AUTOBUNDLE_CADENCE`.
- Use `bin/docs recent 20` to see recently edited docs and memory files.

### Firebase layout & deploy notes

Firebase configuration lives under the `firebase/` folder with per-project subfolders. This repo uses two logical projects:

- `firebase/landing/` — landing + admin hosting, App Check metadata, and security rules for the Lockard org.
- `firebase/petpass/` — PetPass hosting, Firestore rules, and site-specific configs.

Each project directory includes a `firebase.json`, `.firebaserc`, `firestore.rules`, and `storage.rules`. Treat these folders as the single source-of-truth for deployment configuration.

- **Mapping:** `apps/landing` deploys via `firebase/landing` (serving `lockard.llc` + docs), while PetPass (`petpass.help`) deploys via `firebase/petpass`. Keep each app’s `.env*` in sync with the matching `firebase/config.js` file and never cross wires (e.g., don’t point the landing site at the PetPass project).

Deploy guidance

```bash
# from repo root, target the petpass project
cd firebase/petpass
npx firebase deploy --only hosting --project petpass

# deploy landing/admin hosting or security rules from landing folder
cd ../landing
npx firebase deploy --only functions,firestore:rules --project lockard-llc
```

Operational notes

- Keep secrets and service-account keys out of the repo; load them via CI secrets or environment variables.
- Use `firebase emulators:start` locally (from the per-project folder) when running integration tests or rule/unit tests.

### `bin/` helpers and CLI

Lightweight CLI scripts in `bin/` provide common developer ergonomics (see `bin/` for a list). Example utilities include:

- `bin/docs` — quick helpers for working with `docs/` (open in editor, search, recent files, tree, path). This script is intended as a small wrapper around ripgrep/find/editor workflows.
- `bin/lockard` (if present) — top-level convenience to run workspace shortcuts (`lockard bundle`, `lockard web`, etc.). These wrappers are safe to use and intentionally minimal.

When adding new `bin/` scripts, keep them POSIX-friendly, idempotent, and check for environment preconditions (e.g., `LOCKARD_REPO`, `DOCS_ROOT`).

### Docs workflow

The `docs/` directory is the canonical place for public-facing and internal documentation. Use the `bin/docs` helper to quickly open or query the documentation tree. Recommended workflow:

1. Draft new content in `docs/` under the appropriate subfolder (e.g., `docs/tech/`, `docs/handbook/`).
2. Run `node scripts/apply-docpack.js` if you have a docpack to import structured content.
3. Run `node scripts/bundle-context.js` or rely on the `autobundle-daemon.sh` to keep `codex/context/bundle.json` up to date.

```bash
# quick search across docs
bin/docs search "voucher"

# open the docs root in your editor
bin/docs open
```

### Safety & deployment checklist (quick)

- Run `node scripts/bundle-context.js` and confirm `codex/context/bundle.json` was updated.
- Run `yarn test` and `yarn lint` locally.
- Ensure any environment changes are captured in `codex/config/env.schema.ts` (see codex memory for env guidance).

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
