# Firebase Setup Guide for Lockard LLC

**Owner:** WebUXAgent • **Visibility:** Internal  
**Last updated:** 2025-11-11

This guide keeps the Lockard workspace (landing, admin, PetPass) aligned with the two Firebase projects we actually use today.

---

## 1. Repository Layout (authoritative)

```bash
lockard-llc/
├── apps/
│   ├── landing/                 # lockard.llc (Next.js, UI only)
│   └── petpass/                 # petpass.help (Next.js, UI only)
├── admin/
│   └── portal/
│       ├── admin/               # exec/ops portal (Later)
│       └── employee/            # staff portal (Later)
├── data/
│   └── server/                  # shared backend (Express + Firebase Admin)
├── firebase/
│   ├── landing/                 # lockard/admin/docs hosting + rules
│   └── petpass/                 # petpass.help hosting + rules
└── docs/, codex/, scripts/, …
```

**Golden rule:** No app hard-codes Firebase JSON. Every surface imports its config from `firebase/landing/config.js` or `firebase/petpass/config.js`. All privileged work (Firestore Admin, secrets) runs inside `data/server`.

---

## 2. Firebase Projects & Domains

| Project | Purpose | Hosting Sites | Domains |
| --- | --- | --- | --- |
| `lockard-llc` | Landing + admin + docs + Genkit | `lockard-llc-business`, `lockard-llc-docs`, `lockard-llc-admin` (optional) | `lockard.llc`, `admin.lockard.llc`, `docs.lockard.llc` |
| `petpass-lockard` | PetPass public app | `lockard-llc-petpass` | `petpass.help` |

Domains live inside `firebase.json` under each folder. The root `.firebaserc` stores project aliases so `firebase use lockard-llc` works from `firebase/landing` and `firebase use petpass-lockard` works from `firebase/petpass`.

---

## 3. Prerequisites

1. **Node + Corepack** (already in repo setup; ensures Yarn 4 works).
2. **Firebase CLI**

   ```bash
   npm install -g firebase-tools
   firebase login
   ```

3. **Project access**  
   Jerry must invite you to both projects in Firebase Console with **Editor** (or a narrower custom role).

---

## 4. Lockard Project (`firebase/landing`)

### Services Enabled

- Hosting (multi-site)
- Firestore
- Cloud Storage
- Remote Config
- App Check
- Genkit / Cloud Functions (optional)

### Key Files

| Path | Description |
| --- | --- |
| `firebase/landing/firebase.json` | Hosting + emulator config (3 sites) |
| `firebase/landing/.firebaserc` | Project alias (`lockard-llc`) |
| `firebase/landing/firestore.rules` | Firestore auth |
| `firebase/landing/storage.rules` | Storage auth |
| `firebase/landing/ai.rules` | Genkit / AI rules |
| `firebase/landing/config.js` | Web SDK config exported for apps |
| `firebase/landing/remote-config/` | Remote Config templates |
| `firebase/landing/appcheck/` | App Check metadata |

### Hosting Targets

| Alias | Folder served | Domain |
| --- | --- | --- |
| `lockard-llc-business` | `apps/landing/public` | `lockard.llc` |
| `lockard-llc-petpass`  | `apps/petpass/public` (fallback static export) | (backup for marketing or static shell) |
| `lockard-llc-docs`     | `docs/` | `docs.lockard.llc` |
| `lockard-llc-admin` *(optional)* | (future) admin portal static export — legacy `admin/portal/admin/out` removed | `admin.lockard.llc` |

### Deploy Commands

From repo root:

```bash
cd firebase/landing
firebase use lockard-llc

# Everything (hosting + rules + remote config + functions)
firebase deploy

# Just hosting for lockard.llc
firebase deploy --only hosting:lockard-llc-business

# Firestore / Storage rules
firebase deploy --only firestore:rules
firebase deploy --only storage
```

---

## 5. PetPass Project (`firebase/petpass`)

**Services Enabled:**

- Hosting (single site)
- Firestore
- Storage
- App Check

**Key Files:**

| Path | Description |
| --- | --- |
| `firebase/petpass/firebase.json` | Hosting + emulator config |
| `firebase/petpass/.firebaserc` | Alias `petpass-lockard` |
| `firebase/petpass/config.js` | Web SDK config for PetPass |
| `firebase/petpass/firestore.rules` | Firestore rules (tighten before prod) |
| `firebase/petpass/storage.rules` | Storage rules |
| `firebase/petpass/appcheck/` | App Check metadata |

**Hosting:**

- Target: `lockard-llc-petpass`
- Domain: `petpass.help`
- Public directory: `apps/petpass/public` (Next.js static export)

**Deploy Commands:**

```bash
cd firebase/petpass
firebase use petpass-lockard
firebase deploy --only hosting
firebase deploy --only firestore:rules
firebase deploy --only storage
```

---

## 6. Environment & Secrets

- **Client apps** import config from `firebase/landing/config.js` or `firebase/petpass/config.js`. They should not redefine API keys in `.env`.
- **Shared backend (`data/server`)** uses service-account credentials loaded via env vars (`GOOGLE_APPLICATION_CREDENTIALS` or discrete `FIREBASE_CLIENT_EMAIL`/`FIREBASE_PRIVATE_KEY`). Keep those in Bitwarden/CI secrets.
- **App Check** site keys stay public; secret tokens belong in `.env.server`.

Example server env snippet:

```bash
export LOCKARD_FIREBASE_PROJECT_ID="lockard-llc"
export LOCKARD_FIREBASE_CLIENT_EMAIL="firebase-adminsdk@lockard-llc.iam.gserviceaccount.com"
export LOCKARD_FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
export PETPASS_FIREBASE_PROJECT_ID="petpass-lockard"
# ...
```

---

## 7. Local Development & Emulators

### Landing/Admin Project

```bash
cd firebase/landing
firebase emulators:start
```

Ports (default):

- Hosting: `http://localhost:5000`
- Firestore: `8080`
- Storage: `9199`
- Functions (Genkit): `5001`
- Emulator UI: `http://localhost:4000`

### PetPass Project

```bash
cd firebase/petpass
firebase emulators:start
```

Use `NEXT_PUBLIC_FIREBASE_EMULATOR` flags or `.env.local` to point apps at emulators when testing locally.

---

## 8. Custom Domains

Use Firebase Console → Hosting → "Add custom domain" to obtain DNS records. Typical records:

| Domain | Record |
| --- | --- |
| `lockard.llc` | Firebase-provided `A` + verification TXT |
| `admin.lockard.llc` | `CNAME admin lockard-llc-business.web.app` (or use separate site alias) |
| `docs.lockard.llc` | `CNAME docs lockard-llc-docs.web.app` |
| `petpass.help` | `CNAME petpass lockard-llc-petpass.web.app` |

After DNS propagates, re-run `firebase hosting:sites:list` to verify status = `CONNECTED`.

---

## 9. Rules & Security Reminders

- **Firestore**  
  Rules live in each project folder (`firebase/landing/firestore.rules`, `firebase/petpass/firestore.rules`). Harden them before December 11, 2025 (current temporary window).
- **Storage**  
  Defaults are deny-all; open only what’s necessary.
- **AI/Genkit**  
  `firebase/landing/ai.rules` governs who can call AI pipelines; keep it restrictive.
- **Audit**  
  `data/server` should log every privileged operation (case creation, voucher issuance) with request IDs so we can match Firebase logs later.

---

## 10. Monitoring & Troubleshooting

- Firebase Console per project (Analytics, Hosting, Firestore).  
- `firebase target:list` to confirm hosting aliases.  
- `firebase functions:log` (if using Genkit/Cloud Functions).  
- For deploy issues: `firebase projects:list`, `firebase use`, then redeploy with `--debug` if needed.

---

## 11. Quick Checklist

1. `npm install -g firebase-tools` & `firebase login`
2. Configure `.firebaserc` (already in repo) and verify `firebase use`
3. Build apps (`yarn build` / `yarn export`) so `apps/landing/public` and `apps/petpass/public` exist
4. Deploy from the correct folder (`firebase/landing` or `firebase/petpass`)
5. Update DNS if new domains are added
6. Tighten Firestore rules before the permissive window expires
7. Keep configs in `firebase/**/config.js` synced with Jerry’s canonical JSON
8. Document any changes in `docs/FIREBASE_SETUP.md`, `codex/memory/99_changelog.md`, and rebundle context

---

Questions? Ping Jerry or drop a TODO in `codex/memory/90_open_questions.md` before ending your session.
