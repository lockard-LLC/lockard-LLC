# Lockard Unified Admin & Employee Portal — System Overview
> **Status (2025-11-12):** The original `~/lockard-llc/admin/portal` code was removed; this document serves as the blueprint for the upcoming rebuild.
**Owner:** WebUX • **Visibility:** Internal

The Lockard Portal is a role-aware, adaptive workspace that merges the polish of a big-tech intranet with the flow of an online academy. It runs on a single Next.js codebase (`~/lockard-llc/admin/portal`), a shared design-token system (`packages/shared/tokens`), and a unified backend (`data/server`), but bends fluidly to each user’s authority level.

---

## Shared Core Design Language

- **Persistent top nav** with the Lockard “+” glyph, natural-language search (`⌘K` opens command mode), notifications, and profile control.
- **Glass-gradient hero banners** shift tone by role (sky blue for employees, deep indigo for admins) and carry live stats.
- **Hidden sidebar navigation** reveals on hover/edge gesture with parallax slide: *Dashboard, Modules, Checklists, Teams, System, Reports*.
- **Typography:** Inter for system UI, Satoshi (or geometric sans) for headings.
- **Status chips & progress rings** color-coded via tokens (green = done, amber = pending, red = overdue).

All components use the shared color, radius, blur, shadow, and motion tokens so admin and employee views feel like the same OS.

---

## Employee Mode — Learning & Workflow Hub

**Hero Section**
- Shows daily goals, total training time, current badge/level.
- Finishing a module triggers confetti and subtle lighting, implemented with Framer Motion + tokenized motion durations.

**Quick Access Strip**
- Tiles for `My Training`, `Assignments`, `Progress`, `Calendar`. Tiles expand inline to preview next steps using cached Firestore data.

**Course Carousel**
- Horizontally scrollable cards with percent completion, time remaining, resume buttons.
- Hover flips reveal supervisor, duration, checkpoint history.

**Checklists & Deadlines**
- Filterable to-dos (“Today / This Week / Overdue”) with drag-to-complete interactions; synced via Firestore listeners.

**Contextual Sidebar**
- Mini profile with status, action buttons (Join Session, Upload Assignment, Contact Mentor), upcoming deadlines, and cohort chat preview (Firestore subcollections).

---

## Manager & Admin Mode — Command Center View

When Firebase Auth custom claims indicate supervisor/admin, the same frames stack richer insights:

**Dashboard Hero**
- Company-wide metrics (team completion rates, active trainees, recent logins, avg certification times). Toggle live dashboards fed by Firestore/analytics summaries.

**Management Tiles**
- *Team Overview:* table of employees with progress bars and messaging/reassign controls.
- *Assign Training:* modal for bulk enrollment using Firestore batch writes.
- *Analytics:* Recharts/D3 visualizations for course performance.
- *System Config:* UI for editing Firebase Remote Config values (feature flags, color tokens, access toggles).

**Drop-down Tools**
- Collapsible menus for Audit Logs, Attendance Reports, Remote Theme Switcher, Active Sessions Monitor (Realtime Database channel).

**Extended Sidebar**
- Additional entries: `Reports`, `Remote Config`, `Roles`, `System Logs`, plus a “View as Employee” toggle for immediate role emulation.

---

## Dynamic Transitions & Microinteractions

- Sidebar reveal: 240 ms ease-out slide.
- Card entry: 50 ms staggered fade-up sequence.
- Hover states: glass reflection shimmer.
- Loading: blurred skeleton shimmer honoring `prefers-reduced-motion`.

All timings come from `motion.tokens.ts`, ensuring consistent feel across employee/admin experiences.

---

## Command Palette & Automation

The AI-assisted command bar (`⌘K`) executes actions like:

- `start module [Safety-101]`
- `assign training [Tech Ethics] to @Operations`
- `open analytics [Employee Progress]`
- `toggle feature darkMode`

Employee commands surface contextual suggestions; admin commands can invoke callable Firebase Functions to run secure server-side operations.

---

## Unified Classroom + Control Room Philosophy

Employees interact with lessons, checklists, and mentors in a space that adapts to their progress. As roles scale up, the UI reveals additional layers—never a separate product, just the same living system with more capabilities. Supervisors see employee dashboards plus oversight, while admins layer in analytics, remote config, and system logs.

Deliverable summary for engineering:

- Single Next.js app at `~/lockard-llc/admin/portal`.
- Shared tokens + motion from `packages/shared/tokens`.
- Firebase Auth + custom claims for role gates, Firestore for content/progress, Remote Config for feature flags, callable Functions for automation.
- All views wrap the same components with role-aware props, ensuring both portals evolve together.
