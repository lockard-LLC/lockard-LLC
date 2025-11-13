# Lockard Admin Portal Experience

**Owner:** WebUX • **Visibility:** Internal

Think of the **Lockard Admin Portal** as something you’d expect from a tech titan — sleek, quiet confidence wrapped in precision. When you log in, the interface feels weightless: a soft-dark theme balanced by bright accent colors, glassy transparency layers, and fluid transitions that never distract.

## Navigation

- A **hidden sidebar** glides in only when summoned (edge gesture or hover near the left margin).  
- Icon-only navigation glows with subtle gradients and routes to deep yet organized modules: *Training, Tools, People, Settings, Analytics*.  
- When collapsed, the workspace stretches edge to edge and feels immersive, almost like a dedicated app surface.

## Workspace Canvas

- The **main workspace** stays cinematic: wide margins, modular cards, adaptive grids that reflow smoothly per context (dashboards breathe; lessons take the full frame).  
- Sections flow into one another as if they live on a single continuous surface.  
- Typography is modern and tightly spaced, paired with high-contrast color bands that align across pages for rhythm and polish.

## Global Header & Commanding

- A **glassy, pinned global header** only shows essentials: search, notifications, profile.  
- Search expands into a command palette to jump between tools, people, or documents instantly.

## Training & Onboarding

- Presented like a digital university lounge: structured modules, collapsible chapters, smooth scroll snapping between sections.  
- Persistent progress checkmarks follow users in real time.  
- Micro-animations celebrate key states (card flips on completion, gentle pulse when new content unlocks) to reward progress without noise.

## Overall Feel

- Unified, deliberate, no harsh edges or visual clutter.  
- Designed to feel like you’re operating from a single, living system where **design, code, and purpose align**.  
- Whether you’re managing Firebase configs, onboarding employees, or updating global themes, everything happens within this refined control hub at **admin.lockard.llc**.

## Token & motion contracts

- Color/typography/radii/motion originate from `packages/shared/tokens/{tokens,motion}.ts`; the CSS variable bridge lives under `apps/**/src/styles/tokens.css`.
- Tailwind configs must import those tokens so admin, employee, and PetPass surfaces stay visually consistent.
- Motion spec (durations, easing, springs) is authoritative—deviations require approval and must respect `prefers-reduced-motion`.
