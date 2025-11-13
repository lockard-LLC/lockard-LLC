# PetPass Web UI Snapshot

**Owner:** Jerry • **Visibility:** Internal

## Splash screen

- **Pill identity:** Both the PETPASS badge and the entry CTA share the same white pill, silver border, radial halo, and letter-by-letter shimmer so the interaction feels cohesive.
- **Hero stack:** Shimmering badge → breathing “+” logomark → “Emergency vet access without losing privacy.” headline → CTA pill.
- **Background:** Soft indigo→rose gradient with light grid; cards keep rounded corners and subtle shadows for a modern Apple/Android feel.
- **Palette tokens:** The gradient + grid combo now lives in `apps/petpass/lib/theme.ts` and mirrors the shared tokens exported from `packages/shared/tokens/tokens.ts`, ensuring the same indigo-to-rose wash, holographic glow, and white glass card appear on every page.
- **Typographic rhythm:** Badge letters pulse in 150 ms increments, CTA letters trail by 120 ms, and the hero headline locks to 3rem/3.5rem ramps for consistent optical weight.
- **Accessibility:** Pill buttons ship with 4.5:1 contrast on focus/hover, all animations respect `prefers-reduced-motion`, and the CTA retains the large hit target on both desktop and touch.

## Pilot hub entry (`/pp`)

- **Header:** Gradient hero with animated stats, matching buttons, and quick actions (print, CSV exports, staff mode toggle).
- **Content:** Tabs reorganized into rounded cards with glassy borders, richer typography, and matching iconography. Overview now includes company/funding transparency referencing lockard.llc, while the Shawn tab doubles as the VOC onboarding portal (NDA, templates, training).
- **Staff mode:** `/api/staff` (now served by `data/server`) handles email + PIN lookup/registration server-side so the UI never talks to Firestore directly. First-time staff can set their PIN; returning staff verify and unlock onboarding panels.
- **Visual parity:** The hero chips, glass cards, and gradient overlay now mirror the public splash so clinics and staff see the same pastel glow, badge shimmer, and plus-icon halo.
- **Action row:** Print/CVS buttons inherit the pill chrome with purple icons, and the staff-mode toggles switch to the gradient primary pill for a single source of truth.
- **Stats & timeline:** KPI cards sit on white glass with drop shadows, while the right-column timeline uses the same rounded-rectangle language to avoid the former dark/light dissonance.
- **Classroom onboarding:** Staff mode now swaps the old static cards for a five-lesson accordion (NDA → accounts → workstation → outreach → practice). Each lesson gate stores progress in `localStorage`, pipes in doc links + resources, and exposes copy-ready command blocks with ```bash```/```md``` headers and a built-in Copy button so Shawn can paste commands straight into PowerShell. Motion values pull from `packages/shared/tokens/motion.ts`.
- **Migration note:** These lessons will ultimately live inside the rebuilt internal portal (legacy `~/admin/portal/employee` workspace was removed). PetPass now calls the shared Lockard server via `NEXT_PUBLIC_LOCKARD_SERVER_URL`.

## Deployment notes

- App lives under `apps/petpass` (Next.js App Router). Use `yarn dev` locally, `yarn build` + Firebase deploy from `firebase/petpass`.
- UI relies on `framer-motion` for shimmer/animation, Tailwind utility classes for layout, and shared styles to keep the splash + hub consistent.
- Shared gradients, pills, and card chrome live in `apps/petpass/lib/theme.ts`. Update those tokens (not random inline strings) when tweaking colors so both the splash and `/pp` stay in sync.
- Staff-mode panels (workstation tiers, daily commands, troubleshooting) append content pulled from `docs/handbook/onboarding_shawn.md`, so editing the doc immediately impacts the portal copy after rebundling.
- Staff APIs now live under `data/server`; PetPass calls them via `NEXT_PUBLIC_LOCKARD_SERVER_URL` (default `http://localhost:4050`, matching `npm run dev` for the server app).
- Set `CORS_ALLOW_ORIGIN` on the server when deploying to prod domains so employee/admin portals and PetPass can share the same API.
