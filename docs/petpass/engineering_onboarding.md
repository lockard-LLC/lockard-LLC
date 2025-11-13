# Engineering Onboarding for Lockard-LLC Dev Team
**Owner:** Engineering • **Visibility:** Internal

Welcome to the Lockard LLC dev team! This onboarding program is designed to ramp you up within 30 days through modular, self-paced lessons you complete in the portal. Every lesson includes learning objectives, prerequisites, an interactive step-by-step guide, hands-on exercises, an auto-graded 5‑question quiz, curated resources, and estimated time. Track progress in the shared dashboard and pair with your onboarding buddy every Friday. By the end you will have a fully functional dev environment and have shipped production-quality work.

---

## Pre-Setup Checklist for a New Developer Workstation (60–90 min total)

| # | Focus | Details |
|---|-------|---------|
| 1 | **Account Access** (10 min) | Request GitHub org, Firebase console, GitHub Actions, and internal docs via HR portal. Log in to each system and bookmark dashboards. |
| 2 | **Hardware & OS** (5 min) | Minimum hardware: 16 GB RAM, SSD, modern CPU. On Windows enable WSL2 (Ubuntu). macOS/Linux use native shell. |
| 3 | **Terminal + Shell** (5 min) | Install Windows Terminal (Store) or iTerm2 (macOS). Set bash or zsh default. |
| 4 | **VS Code** (10 min) | Install from [code.visualstudio.com](https://code.visualstudio.com). Sign in with GitHub sync, install ESLint, Prettier, Tailwind CSS IntelliSense, Firebase Explorer, GitLens. |
| 5 | **Authentication** (10 min) | `ssh-keygen -t ed25519 -C "you@email"`, add public key to GitHub, create PAT if CI triggers needed. |
| 6 | **Verify Setup** (10 min) | Run `node --version` (should fail before installing), clone a test repo, and post `echo "Dev env ready"` screenshot to onboarding ticket. |

Need help? Join `#dev-setup` for live support. Success metric: screenshot + checklist stored in your onboarding ticket.

---

## Lesson 1: Core Runtimes and Package Managers (45 min)
- **Objectives:** Install nvm, switch Node versions, configure pnpm (or Yarn) for monorepos.
- **Prereqs:** Terminal access, sudo/admin permissions.
- **Guide:**
  1. Install nvm (`curl -o- ...install.sh | bash`, restart shell, verify with `nvm --version`).
  2. Install Node 20 (`nvm install 20`, `nvm use 20`, verify `node --version`). Add `nvm use 20` to shell profile.
  3. Install pnpm (`curl -fsSL https://get.pnpm.io/install.sh | sh -`). Configure `~/.npmrc` with `shamefully-hoist=false` and `link-workspace-packages=true`. Mention Yarn alternative.
  4. Review monorepo workflow via `pnpm-workspace.yaml` (apps/petpass, admin portal, etc.).
- **Exercises:** Switch to Node 18, run `test.js`, then back to 20. Initialize pnpm project, add lodash, log camelCase result.
- **Quiz:** nvm stands for? Why pnpm for monorepos? Command to list versions? T/F: npm always preferred? Provide pnpm version.
- **Resources:** Node docs, nvm GitHub, pnpm docs, Yarn/npm references.

## Lesson 2: JS and TypeScript Toolchain (60 min)
- **Objectives:** Configure strict TypeScript, linting, formatting, commit hooks.
- **Prereqs:** Lesson 1 complete.
- **Guide:**
  1. Install TS + types (`pnpm add -D typescript @types/node`), run `npx tsc --init`, set target ES2020, module ESNext, strict true.
  2. Add ESLint + TS plugin, run `npx eslint --init`, configure parser + extends.
  3. Add Prettier + config; extend ESLint with `eslint-config-prettier`.
  4. Add Commitlint with conventional config.
  5. Install Husky + lint-staged, wire pre-commit hook running lint-staged.
  6. Configure semantic-release with commit analyzer + notes plugin, run dry-run.
- **Exercises:** Build math util with lint errors, ensure Husky fixes before commit. Create breaking change commit and run semantic-release dry run.
- **Quiz:** Purpose of tsconfig, what runs on git commit, conventional commit example, Prettier vs ESLint, semantic-release dry run command.
- **Resources:** TS Handbook, ESLint docs, Prettier, Commitlint, Husky/lint-staged, Conventional Commits, semantic-release guide.

## Lesson 3: Frameworks and Bundlers (50 min)
- **Objectives:** Scaffold React/Next.js apps, understand Vite/SWC benefits.
- **Guide:** Vite React scaffold (`pnpm create vite`), Next.js alternative for SSR, highlight Vite dev server speed, enable SWC (Next `swcMinify: true`, Vite plugin). Measure build time before/after SWC.
- **Exercises:** Benchmark Vite vs Next dev startup, add CourseCard component, install `@vitejs/plugin-react-swc` and compare bundle sizes.
- **Quiz Resources:** React docs, Next.js Learn, Vite guide, SWC docs.

## Lesson 4: UI Systems, Tokens, Micro-Interactions (70 min)
- **Objectives:** Build accessible UI primitives with Tailwind, Radix, shadcn/ui, tokens, and light animations.
- **Guide:**
  1. Tailwind install + config, extend with school palette.
  2. `npx shadcn-ui` init, add button primitive, use in sample component.
  3. Add `lucide-react` + `framer-motion` for animated buttons.
  4. Sync tokens via Style Dictionary -> Tailwind.
- **Exercises:** Build CourseCard with tokens, animate button hover using Framer Motion.
- **Resources:** Tailwind docs, Radix UI, shadcn/ui, Lucide, Framer Motion, Style Dictionary, Tokens Studio.

## Lesson 5: Accessibility & Inclusive Design (55 min)
- **Objectives:** Apply WCAG 2.2 AA, ARIA roles, test with axe.
- **Guide:** Review WCAG key criteria (contrast, focus). Install axe DevTools, integrate `@axe-core/react`, run CLI audits. Demonstrate accessible modal using Radix Dialog.
- **Exercises:** Axe audit on Lesson 4 project, fix findings (alt text, focus states). Build accessible accordion with ARIA state announcements.
- **Resources:** WCAG quick reference, WAI-ARIA practices, axe DevTools, Inclusive Components.

## Lesson 6: Testing and QA (65 min)
- **Objectives:** Write unit + E2E tests with Vitest, Testing Library, Playwright, and hook into CI.
- **Guide:** Install Vitest + RTL, configure `test.environment = 'jsdom'`. Add sample unit test. Add Playwright, create login E2E, configure Lighthouse CI for perf.
- **Exercises:** Write CourseCard RTL test, create login E2E hitting mock Firebase, run Lighthouse CI.
- **Resources:** Vitest, Jest, Testing Library, Playwright, Cypress alternative, Lighthouse CI.

## Lesson 7: Firebase Stack Essentials (75 min)
- **Objectives:** Configure Firebase Auth, Firestore, Storage, security rules, App Check.
- **Guide:** Enable services in console, install firebase-tools, run `firebase init`, create `firebase.ts` with `initializeApp`, `getAuth`, `getFirestore`. Write CRUD sample. Define security rules allowing public reads + auth writes, run emulator tests, enable App Check, deploy hosting.
- **Exercises:** Implement email/password auth + Firestore addDoc, test rule that only teachers write to courses, use emulator to validate unauthorized write fails.
- **Resources:** Firebase quickstart, console, Firestore rules doc, App Check guide.

## Lesson 8: CI/CD DevOps and Automation (80 min)
- **Objectives:** Automate lint/test/build/deploy via GitHub Actions, containerize dev services, manage deps.
- **Guide:** Create CI workflow (checkout, setup-node@v4, pnpm install, lint, test). Write Dockerfile + docker-compose for app + Firestore emulator. Configure Renovate or Dependabot, add Turbo repo caching, mention Terraform for infra.
- **Exercises:** Trigger failing CI on PR, fix and watch succeed. Run `docker compose up` with live reload.
- **Resources:** GitHub Actions quickstart, Docker/Compose docs, Terraform intro, Ansible overview, Renovate, Changesets, Turbo docs.

## Lesson 9: Hosting, Edge, DNS, Observability (60 min)
- **Objectives:** Deploy to Vercel/Firebase Hosting, add Sentry, OpenTelemetry, logging.
- **Guide:** Connect repo to Vercel (preview deploys) or run `firebase deploy`. Configure custom domain + DNS records. Install Sentry, wrap app with ErrorBoundary, send traces via OpenTelemetry, ship logs to Logtail/Grafana.
- **Exercises:** Push branch, validate Vercel preview; trigger error and confirm Sentry alert.
- **Resources:** Vercel docs, Netlify/Cloudflare alternatives, Firebase Hosting, Sentry, OpenTelemetry, Logtail, Grafana/Prometheus.

## Lesson 10: Documentation, Onboarding, Diagrams (50 min)
- **Objectives:** Ship docs as code with Docusaurus/Mintlify, embed Mermaid + Excalidraw, document onboarding checklists.
- **Guide:** `pnpm create docusaurus my-docs`, add pages, run dev server. Embed Mermaid flow for CI pipeline. Use Excalidraw and Roadmap.sh content, add onboarding table.
- **Exercises:** Publish Getting Started page (pre-setup steps). Create Mermaid auth-flow diagram. Deploy docs to GitHub Pages.
- **Resources:** Docusaurus, Mintlify, Mermaid live editor, Excalidraw, Roadmap.sh.

---

## One-Click Employee Workstation Script

```bash
#!/bin/bash
set -euo pipefail

if ! command -v nvm >/dev/null 2>&1; then
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
fi

nvm install 20 --reinstall-packages-from=20
nvm use 20

if ! command -v pnpm >/dev/null 2>&1; then
  curl -fsSL https://get.pnpm.io/install.sh | sh -
fi

pnpm install -g firebase-tools

if command -v code >/dev/null 2>&1; then
  extensions=(dbaeumer.vscode-eslint esbenp.prettier-vscode bradlc.vscode-tailwindcss GitHub.copilot Firebase.firebase-vscode eamodio.gitlens)
  for ext in "${extensions[@]}"; do
    code --install-extension "$ext" || true
  done
fi

git config --global user.name "New Hire"
git config --global user.email "newhire@lockard.llc"

echo "Setup complete! Run 'pnpm dev' inside ~/lockard-llc."
```

Make it executable with `chmod +x bootstrap.sh` and commit via onboarding PR.

---

## Suggested 30/60/90 Learning Path

| Phase | Days | Focus | Milestones | Check-ins |
|-------|------|-------|------------|-----------|
| **Onboarding Sprint** | 1–7 | Pre-setup + Lessons 1–3 | Local env ready, tests green | Buddy call Day 3, post screenshot |
| **Productivity Ramp** | 8–30 | Lessons 4–6, ship first feature (e.g., student dashboard) | PR merged with tests + preview | Weekly standup, code review follow-up |
| **Ownership Phase** | 31–90 | Lessons 7–10, automation + docs | Adds CI improvements + observability, updates onboarding docs | Monthly review, mentor shadow |

---

## Final Checklist Before Mission Start

- [ ] `pnpm dev` and `pnpm test` succeed locally.
- [ ] Pre-commit hooks block bad commits (lint + format).
- [ ] CI pipeline green on sample PR with preview deploy links.
- [ ] Firebase + Sentry + GitHub Actions access verified (create test doc, view dashboard).
- [ ] Personal Notion/Confluence doc includes goals, buddy schedule, progress snapshots.

Questions? Ping `#eng-onboarding`. 🚀
