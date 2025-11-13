# Onboarding Checklist — Shawn (VOC)
> **Status (2025-11-12):** The dedicated Lockard Employee Portal codebase (`~/admin/portal/employee`) was removed ahead of a full redesign. Continue using the PetPass staff tab (`/pp`) plus this handbook until the rebuilt portal ships.
> **Note:** The workspace now uses **Yarn 4** via Corepack. Any legacy `pnpm` commands shown below should be translated to their Yarn equivalents (`yarn install`, `yarn workspace <pkg> dev`, etc.) until the remainder of this guide is updated.

**Owner:** Shawn • **Visibility:** Internal

Shawn’s onboarding now lives inside the Lockard Employee Portal (migrating from the PetPass staff tab). Enable staff mode for now via `https://petpass.help/pp` (until the new portal at `~/admin/portal/employee` ships) with the lockard.llc email + PIN to view the NDA packet, scripts, and this workstation guide inline. Use this document as the canonical reference whenever you refresh the bundle or share enablement notes.

- Accounts: <skelly@lockard.llc>, <skelly@petpass.help>, Google Voice, Sheets access, secure folder, password manager, calendar.
- Templates: vouchers, intake replies, outreach emails, ledger math examples.
- Runbooks to review: safety/privacy, incident response, data retention/security.
- Dry-run: intake → voucher → clinic → invoice math walkthrough with Jerry.
- Agreements: boundaries & expectations, code of conduct, NDA + safety pledge.

## Current workspace snapshot (Nov 2025)

- **UI surfaces:** `apps/landing` (lockard.llc) and `apps/petpass` are active. The prior `admin/portal/{employee,admin}` workspaces were removed and will be replaced by the next-gen internal portal. All surfaces share the Lockard design tokens (`packages/shared/tokens/**`) for colors, spacing, and motion.
- **Backend:** `data/server` (Express + Firebase Admin) powers staff APIs, onboarding, intake, and analytics. Never call Firebase Admin from the client.
- **Firebase configs:** live under `firebase/landing` (lockard + admin) and `firebase/petpass`. Do not paste API keys elsewhere—import the config module.
- **Docs & memory:** any onboarding gap should be logged via `mem-end`, appended to `codex/memory/90_open_questions.md`, and cross-referenced inside `docs/handbook/onboarding_shawn.md`.

## Enable Staff Mode

1. Go to `https://petpass.help/pp` and click **Enable staff mode**.
2. Enter the lockard.llc email + PIN from Jerry (Bitwarden holds the backup).
3. After MFA, confirm you can see the classroom-style lessons (Lesson 1 — NDA, Lesson 2 — Accounts, etc.), each with copy-ready command blocks and doc links mirrored from this guide (they previously lived under `~/admin/portal/employee` and will return when the portal is rebuilt).
4. Capture any missing resource as a TODO in `codex/memory/90_open_questions.md` before ending the session.

> **Reminder:** Staff mode is session-based. Re-enable it whenever you open a fresh browser tab or after the inactivity timer expires.

---

## PetPass Workstation Setup Guide for Shawn

**Philosophy:** This is a “no-drama” workstation that lets Shawn run the web app, peek at the mobile app, and do ops work without drowning in DevOps. The goal is self-sufficiency for common workflows—viewing live data, verifying UI fixes, and handling operational checks—without needing a full developer environment.

Think of it as a **confidence station**: stable, lightweight, and capable of running PetPass locally without surprises.

---

## 📋 Table of Contents

1. [Tier 0 — Non-dev Essentials](#tier-0--non-dev-essentials)
2. [Tier 1 — Developer-lite (Run Web Locally)](#tier-1--developer-lite-run-web-locally)
3. [Tier 2 — Mobile Preview (Optional)](#tier-2--mobile-preview-optional)
4. [Tier 3 — Advanced Tooling (Rare)](#tier-3--advanced-tooling-rare)
5. [Automated Setup Script](#automated-setup-script)
6. [Daily Commands](#daily-commands)
7. [Troubleshooting](#troubleshooting)
8. [Quick Reference](#quick-reference)
9. [Onboarding Checklist](#onboarding-checklist)
10. [Getting Help](#getting-help)
11. [Tips for Success](#tips-for-success)

---

## Tier 0 — Non-dev Essentials

**Purpose:** Communication, PDFs, spreadsheets, and running the PetPass web app in a browser. This is the “ops-ready” baseline—everything needed to handle daily PetPass workflows without code or command-line tools.

### Core Tools

| Tool | Purpose | Installation |
|------|---------|--------------|
| **Google Workspace** | Gmail, Calendar, Drive (shared “PetPass” folder), Google Voice | Sign in via Chrome |
| **Chrome Browser** | Primary browser for consistency with devtools | [Download](https://www.google.com/chrome/) |
| **Bitwarden** | Password manager for shared credentials | `winget install Bitwarden.Bitwarden` |
| **Zoom** | Video calls | `winget install Zoom.Zoom` |
| **Loom** | Screen recording for bug reports | [Download](https://www.loom.com/) |

### Optional Productivity Tools

- **ShareX**: Advanced screenshot tool with annotation (`winget install ShareX.ShareX`)
- **Snip & Sketch**: Built-in Windows tool (Win+Shift+S)
- **Foxit PDF Reader**: Lighter alternative to Adobe (`winget install Foxit.FoxitReader`)
- **Notepad++**: Quick text editing (`winget install Notepad++.Notepad++`)

### Browser Setup

**Bookmarks to create:**

- Production: `https://petpass.help`
- Staff Hub: `https://petpass.help/hub`
- Intake Form: `https://petpass.help/intake`
- Voucher Management: `https://petpass.help/vouchers`
- Local Dev: `http://localhost:3000` (after Tier 1 setup)

**Chrome Extensions to install:**

- Bitwarden (password autofill)
- Loom (quick screen recording)
- JSON Formatter (for viewing API responses)
- React Developer Tools (optional, for debugging)

### vCard Support

Ensure double-clicking `.vcf` files opens Windows Contacts:

1. Right-click a `.vcf` file → “Open with”
2. Choose “Contacts” or “People”
3. Check “Always use this app”

> **Result:** Shawn can do intake → voucher → clinic → invoice and read the Staff Hub at `/hub` with zero coding knowledge required.

---

## Tier 1 — Developer-lite (Run Web Locally)

**Why:** Open a branch, run the **web** site locally, and click through UI—no Android or Docker required. This gives visibility into new changes before they go live and allows simple QA without a full dev stack.

### Prerequisites Checklist

Before starting, ensure:

- [ ] Windows 10 or 11 (64-bit)
- [ ] At least 8GB RAM (16GB recommended)
- [ ] 20GB free disk space
- [ ] Stable internet connection
- [ ] Admin access to the machine

### Step-by-Step Installation

#### 1. Install Git

```powershell
winget install Git.Git
```

**Verify installation:**

```powershell
git --version
# Should show: git version 2.x.x
```

**Configure Git (first time only):**

```powershell
git config --global user.name "Shawn YourLastName"
git config --global user.email "shawn@petpass.help"
git config --global core.autocrlf true
```

#### 2. Install Node.js via nvm-windows

```powershell
winget install CoreyButler.NVMforWindows
```

**Close and reopen PowerShell**, then:

```powershell
nvm install 20.18.0
nvm use 20.18.0
node -v
# Should show: v20.18.0
```

**Why nvm?** Allows easy switching between Node versions if needed later.

#### 3. Enable Package Managers (pnpm & Yarn)

```powershell
corepack enable
corepack prepare pnpm@latest --activate
corepack prepare yarn@stable --activate
```

**Verify:**

```powershell
pnpm -v
yarn -v
```

#### 4. Install VS Code + Extensions

```powershell
winget install Microsoft.VisualStudioCode
```

**Essential extensions** (install via Extensions sidebar in VS Code):

- **ESLint** (by Microsoft) - Code quality
- **Prettier** (by Prettier) - Code formatting
- **Tailwind CSS IntelliSense** - CSS autocomplete
- **GitLens** (by GitKraken) - Git visualization
- **EditorConfig** - Consistent formatting
- **Error Lens** - Inline error display
- **Auto Rename Tag** - HTML/JSX productivity

**Recommended VS Code settings** (File → Preferences → Settings → Open Settings JSON):

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "onFocusChange",
  "terminal.integrated.defaultProfile.windows": "PowerShell"
}
```

#### 5. Clone Repository & Run

```powershell
# Create workspace folder
mkdir $env:USERPROFILE\workspace
cd $env:USERPROFILE\workspace

# Clone repository
git clone https://github.com/lockard-llc/petpass
cd petpass

# Install dependencies (first time only - takes 2-5 minutes)
pnpm install

# Start development server
pnpm --filter petpass dev
```

**Open browser to:** [http://localhost:3000](http://localhost:3000)

You should see the PetPass homepage! 🎉

### Working with Branches

**To test a specific feature branch:**

```powershell
cd $env:USERPROFILE\workspace\petpass
git fetch origin
git checkout branch-name
pnpm install  # In case dependencies changed
pnpm --filter petpass dev
```

**To return to main:**

```powershell
git checkout main
git pull origin main
```

### Environment Variables

Some features require environment variables. Create a file called `.env.local` in the `petpass` folder:

```bash
# petpass/.env.local
NEXT_PUBLIC_API_URL=https://api.petpass.help
NEXT_PUBLIC_STRIPE_KEY=pk_test_...
NEXT_PUBLIC_LOCKARD_SERVER_URL=http://localhost:4050
# (Get actual keys + server endpoint from Bitwarden vault)
```

**Important:** Never commit `.env.local` to Git. It’s automatically ignored.

---

## Tier 2 — Mobile Preview (Optional)

> **Status:** `apps/petpass-mobile` has been archived. Keep these notes for future Android work, but the current pilot only runs the web app.

**Purpose:** View PetPass Mobile (React Native) in real time, either on an Android device or via emulator.

**⚠️ Only do this if you actually need mobile app testing.** This adds complexity.

### Method A: Real Phone (Recommended)

1. Install **Expo Go** from Play Store on your Android phone
2. Ensure phone and computer are on the same Wi-Fi network
3. Run the mobile app:

   ```powershell
   cd $env:USERPROFILE\workspace\petpass
   pnpm --filter petpass-mobile start
   ```

4. Scan the QR code with Expo Go app
5. App loads on your phone!

**Pros:** Simple, no emulator overhead, real device testing  
**Cons:** Requires physical device, same Wi-Fi network

### Method B: Android Emulator (More Complex)

#### 1. Install Android Studio

Download from [developer.android.com/studio](https://developer.android.com/studio)

During installation:

- Choose “Standard” installation
- Accept all licenses
- Wait for initial SDK download (can take 10-20 minutes)

#### 2. Install SDK Components

Open Android Studio → More Actions → SDK Manager:

**SDK Platforms tab:**

- [x] Android 14.0 (API Level 34) - recommended
- [x] Android 13.0 (API Level 33)

**SDK Tools tab:**

- [x] Android SDK Build-Tools
- [x] Android SDK Command-line Tools
- [x] Android Emulator
- [x] Android SDK Platform-Tools

**Note SDK location** (usually): `C:\Users\<YourName>\AppData\Local\Android\Sdk`

#### 3. Install Java 17

```powershell
winget install Azul.Zulu.17.JDK
```

**Alternative:** Temurin JDK

```powershell
winget install EclipseAdoptium.Temurin.17.JDK
```

#### 4. Set Environment Variables

**Open PowerShell as Administrator**, then run:

```powershell
# Set JAVA_HOME
[Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Azul\zulu-17", "Machine")

# Set ANDROID_HOME
[Environment]::SetEnvironmentVariable("ANDROID_HOME", "$env:USERPROFILE\AppData\Local\Android\Sdk", "Machine")

# Add to PATH
$paths = @(
  "$env:USERPROFILE\AppData\Local\Android\Sdk\platform-tools",
  "$env:USERPROFILE\AppData\Local\Android\Sdk\emulator",
  "C:\Program Files\Azul\zulu-17\bin"
)
$existing = [Environment]::GetEnvironmentVariable("Path", "Machine")
$new = ($existing + ";" + ($paths -join ";")).TrimEnd(";")
[Environment]::SetEnvironmentVariable("Path", $new, "Machine")
```

**Close and reopen PowerShell**, then verify:

```powershell
java -version
adb --version
emulator -version
```

#### 5. Create Virtual Device

1. Open Android Studio → More Actions → Virtual Device Manager
2. Click “Create Device”
3. Choose “Pixel 7” or “Pixel 6”
4. Select System Image: **API Level 34** (Android 14)
5. Click “Finish”
6. Click ▶️ play button to start emulator

**Wait 2-3 minutes for first boot.**

#### 6. Run Mobile App in Emulator

```powershell
# Legacy instructions – repo archived until mobile relaunch
cd $env:USERPROFILE\workspace\petpass
pnpm --filter petpass-mobile start
```

Press `a` when prompted to open in Android emulator.

### Troubleshooting Mobile Setup

**Emulator won’t start:**

- Enable virtualization in BIOS (VT-x or AMD-V)
- Disable Hyper-V: `bcdedit /set hypervisorlaunchtype off` (requires restart)

**“SDK location not found”:**

- Verify `ANDROID_HOME` is set: `echo $env:ANDROID_HOME`
- Should show: `C:\Users\<YourName>\AppData\Local\Android\Sdk`

**“Unable to load script”:**

- Ensure emulator and dev server are running
- Legacy command (repo archived): `pnpm --filter petpass-mobile start --clear`

---

## Tier 3 — Advanced Tooling (Rare)

Install only when specifically needed:

### Docker Desktop (for running services locally)

```powershell
winget install Docker.DockerDesktop
```

**Restart required after installation.**

### PostgreSQL (if local database needed)

```powershell
winget install PostgreSQL.PostgreSQL
```

### API Testing Tools

**Postman:**

```powershell
winget install Postman.Postman
```

**Bruno (lightweight alternative):**

```powershell
winget install Bruno.Bruno
```

### Image Processing (for voucher PDFs)

```powershell
winget install ImageMagick.ImageMagick
winget install GnuWin32.Ghostscript
```

---

## Automated Setup Script

Save this as `setup-shawn.ps1` and run **as Administrator** (right-click → “Run with PowerShell”):

```powershell
# setup-shawn.ps1 — PetPass Developer-lite bootstrap for Windows
# Version: 2.0

$ErrorActionPreference = "Stop"

# Check if running as Administrator
$isAdmin = ([Security.Principal.WindowsPrincipal][Security.Principal.WindowsIdentity]::GetCurrent()).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)
if (-not $isAdmin) {
    Write-Host "⚠️  Please run this script as Administrator (right-click → Run as Administrator)" -ForegroundColor Yellow
    exit 1
}

Write-Host "🚀 PetPass Workstation Setup" -ForegroundColor Cyan
Write-Host "================================`n" -ForegroundColor Cyan

# Function: Check if Winget is available
function Ensure-Winget {
    try {
        winget -v *> $null
    } catch {
        Write-Error "Winget not found. Please update 'App Installer' from Microsoft Store."
        exit 1
    }
}

# Function: Install if missing
function Install-IfMissing($id, $exe, $name) {
    Write-Host "Checking $name..." -ForegroundColor Yellow
    $exists = (Get-Command $exe -ErrorAction SilentlyContinue) -ne $null
    if (-not $exists) {
        Write-Host "  Installing $name..." -ForegroundColor Green
        winget install --id $id -e --accept-source-agreements --accept-package-agreements --silent
    } else {
        Write-Host "  ✓ $name already installed" -ForegroundColor Green
    }
}

# Ensure Winget is available
Ensure-Winget

# Install core tools
Install-IfMissing "Git.Git" "git" "Git"
Install-IfMissing "CoreyButler.NVMforWindows" "nvm" "NVM for Windows"
Install-IfMissing "Microsoft.VisualStudioCode" "code" "VS Code"

# Refresh PATH
$env:Path = [System.Environment]::GetEnvironmentVariable("Path", "Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path", "User")

# Install Node via NVM
Write-Host "`nSetting up Node.js..." -ForegroundColor Yellow
try {
    nvm install 20.18.0
    nvm use 20.18.0
    Write-Host "  ✓ Node.js 20.18.0 installed" -ForegroundColor Green
} catch {
    Write-Host "  ⚠️  NVM not in PATH yet. Please reopen PowerShell and run: nvm install 20.18.0 && nvm use 20.18.0" -ForegroundColor Yellow
}

# Enable Corepack (for pnpm/yarn)
Write-Host "`nEnabling package managers..." -ForegroundColor Yellow
try {
    corepack enable
    corepack prepare pnpm@latest --activate
    corepack prepare yarn@stable --activate
    Write-Host "  ✓ pnpm and yarn enabled" -ForegroundColor Green
} catch {
    Write-Host "  ⚠️  Run this after reopening PowerShell: corepack enable && corepack prepare pnpm@latest --activate" -ForegroundColor Yellow
}

# Optional: Java 17
Write-Host "`nOptional: Install Java 17 for Android development? (y/n): " -ForegroundColor Yellow -NoNewline
$response = Read-Host
if ($response -eq "y") {
    Install-IfMissing "Azul.Zulu.17.JDK" "java" "Java 17 (Zulu)"
}

# Check for Android SDK
$AndroidSDK = "$env:USERPROFILE\AppData\Local\Android\Sdk"
if (Test-Path $AndroidSDK) {
    Write-Host "`nFound Android SDK, setting environment variables..." -ForegroundColor Yellow
    
    [Environment]::SetEnvironmentVariable("ANDROID_HOME", $AndroidSDK, "Machine")
    [Environment]::SetEnvironmentVariable("JAVA_HOME", "C:\Program Files\Azul\zulu-17", "Machine")
    
    $add = @(
        "$AndroidSDK\platform-tools",
        "$AndroidSDK\emulator",
        "C:\Program Files\Azul\zulu-17\bin"
    )
    $existing = [Environment]::GetEnvironmentVariable("Path", "Machine")
    $new = ($existing + ";" + ($add -join ";")).TrimEnd(";")
    [Environment]::SetEnvironmentVariable("Path", $new, "Machine")
    
    Write-Host "  ✓ Android environment configured" -ForegroundColor Green
}

# Create workspace folder
$workspace = "$env:USERPROFILE\workspace"
if (-not (Test-Path $workspace)) {
    New-Item -ItemType Directory -Path $workspace | Out-Null
    Write-Host "`n✓ Created workspace folder: $workspace" -ForegroundColor Green
}

# Display versions
Write-Host "`n=== Installation Summary ===" -ForegroundColor Cyan
Write-Host "Git:       " -NoNewline; git --version
Write-Host "Node:      " -NoNewline; node -v 2>$null
Write-Host "pnpm:      " -NoNewline; pnpm -v 2>$null
Write-Host "Yarn:      " -NoNewline; yarn -v 2>$null
if (Get-Command java -ErrorAction SilentlyContinue) {
    Write-Host "Java:      " -NoNewline; java -version 2>&1 | Select-String "version"
}
if (Get-Command adb -ErrorAction SilentlyContinue) {
    Write-Host "ADB:       " -NoNewline; adb --version 2>&1 | Select-Object -First 1
}

Write-Host "`n✅ Setup complete!" -ForegroundColor Green
Write-Host "`n📝 Next steps:" -ForegroundColor Cyan
Write-Host "   1. Close and reopen PowerShell"
Write-Host "   2. cd $workspace"
Write-Host "   3. git clone https://github.com/lockard-llc/petpass"
Write-Host "   4. cd petpass"
Write-Host "   5. pnpm install"
Write-Host "   6. pnpm --filter petpass dev`n"
```

---

## Daily Commands

### Starting Work

```powershell
# Navigate to project
cd $env:USERPROFILE\workspace\petpass

# Get latest code
git pull

# Update dependencies if needed
pnpm install

# Start web app
pnpm --filter petpass dev

# Open in browser: http://localhost:3000
```

### Common Tasks

**Start Lockard staff server (for PIN + classroom APIs):**

```powershell
cd $env:USERPROFILE\workspace\admin\portal\server
npm install   # first run only (requires VPN/network)
npm run dev   # listens on http://localhost:4050
```

**Run mobile app (archived for now):**

```powershell
# Legacy instructions – repo archived until mobile relaunch
pnpm --filter petpass-mobile start
```

**Check what branch you’re on:**

```powershell
git branch
# * indicates current branch
```

**Switch branches:**

```powershell
git fetch origin
git checkout feature-branch-name
pnpm install  # If dependencies changed
```

**Return to main:**

```powershell
git checkout main
git pull
```

**Update documentation bundle:**

```powershell
node scripts/bundle-context.js
```

**Check versions:**

```powershell
node -v
pnpm -v
git --version
```

### Quick Commands Script

Add these to `package.json` in the project root:

```json
{
  "scripts": {
    "start:shawn": "node scripts/bundle-context.js && pnpm --filter petpass dev",
    "start:web": "pnpm --filter petpass dev",
    "doctor": "node -v && pnpm -v && git --version",
    "fresh-start": "pnpm install && pnpm --filter petpass dev"
  }
}
```

Then Shawn can just run:

```powershell
pnpm start:shawn
```

---

## Troubleshooting

### “pnpm: command not found”

**Solution:**

```powershell
corepack enable
corepack prepare pnpm@latest --activate
```

Close and reopen PowerShell.

### “Port 3000 already in use”

**Solution:**

```powershell
# Find process using port 3000
netstat -ano | findstr :3000

# Kill the process (replace PID with the number from above)
taskkill /PID <PID> /F
```

Or use a different port:

```powershell
$env:PORT=3001; pnpm --filter petpass dev
```

### Git: “fatal: not a git repository”

**Solution:** You’re not in the project folder.

```powershell
cd $env:USERPROFILE\workspace\petpass
```

### “Module not found” errors

**Solution:** Dependencies might be out of date.

```powershell
pnpm install
```

If that doesn’t work:

```powershell
# Clean install
rm -r node_modules
rm pnpm-lock.yaml
pnpm install
```

### VS Code: Terminal shows weird characters

**Solution:** Set PowerShell as default terminal:

1. Ctrl+Shift+P
2. Type “Terminal: Select Default Profile”
3. Choose “PowerShell”

### Can’t run scripts (execution policy error)

**Solution:**

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Android emulator: “PANIC: Cannot find AVD system path”

**Solution:** Verify ANDROID_HOME:

```powershell
echo $env:ANDROID_HOME
# Should show: C:\Users\<YourName>\AppData\Local\Android\Sdk
```

If not set, run setup script again as Admin.

### Web app loads but shows errors

**Check:**

1. Is `.env.local` file present in `petpass` folder?
2. Are API keys correct? (Check Bitwarden vault)
3. Is the API server running? (Usually `https://api.petpass.help`)

---

## Quick Reference

### File Locations

```bash
C:\Users\Shawn\workspace\petpass\          # Main project
├── petpass\                           # Web app
│   ├── .env.local                        # Local environment variables (create this)
│   └── src\
├── petpass-mobile\                        # (Archived) Mobile app placeholder
└── docs\                                  # Documentation
```

### Important URLs

| Purpose | URL |
|---------|-----|
| Production site | <https://petpass.help> |
| Staff Hub | <https://petpass.help/hub> |
| Local dev (web) | <http://localhost:3000> |
| Local dev (mobile) | Expo QR code |
| GitHub repo | <https://github.com/lockard-llc/petpass> |

### Keyboard Shortcuts (VS Code)

| Action | Shortcut |
|--------|----------|
| Open terminal | Ctrl + ` |
| Command palette | Ctrl + Shift + P |
| Quick file open | Ctrl + P |
| Find in files | Ctrl + Shift + F |
| Format document | Shift + Alt + F |
| Git panel | Ctrl + Shift + G |
| Split editor | Ctrl + \ |

### Common Git Commands

```powershell
git status              # See what changed
git pull                # Get latest code
git checkout main       # Switch to main branch
git branch              # List branches (* = current)
git log --oneline       # See recent commits
git diff                # See changes
```

### Package Manager Commands

```powershell
pnpm install            # Install dependencies
pnpm --filter <pkg> dev # Run dev server for specific package
pnpm run <script>       # Run a script from package.json
pnpm list               # List installed packages
```

---

## Onboarding Checklist

Print this and check off as you go:

**Tier 1 (Essential):**

- [ ] Install Git via `winget install Git.Git`
- [ ] Install nvm-windows
- [ ] Run `nvm install 20.18.0` and `nvm use 20.18.0`
- [ ] Enable Corepack: `corepack enable`
- [ ] Activate pnpm: `corepack prepare pnpm@latest --activate`
- [ ] Install VS Code
- [ ] Install VS Code extensions (ESLint, Prettier, GitLens)
- [ ] Clone repository to `%USERPROFILE%\workspace\petpass`
- [ ] Run `pnpm install` (takes 2-5 minutes)
- [ ] Create `.env.local` in `petpass` folder (get keys from Bitwarden)
- [ ] Run `pnpm --filter petpass dev`
- [ ] Open <http://localhost:3000> in Chrome
- [ ] Verify site loads correctly

**Tier 2 (Optional - Mobile):**

- [ ] Install Android Studio
- [ ] Install SDK Platform 34
- [ ] Install SDK Command-line Tools
- [ ] Install Java 17 (Zulu or Temurin)
- [ ] Set `ANDROID_HOME` environment variable
- [ ] Set `JAVA_HOME` environment variable
- [ ] Add Android SDK to PATH
- [ ] Verify `adb --version` works
- [ ] Create Pixel 7 AVD in Android Studio
- [ ] Test emulator launch
- [ ] (Legacy) Run `pnpm --filter petpass-mobile start`
- [ ] Press `a` to launch in emulator (once mobile repo returns)

**Configuration:**

- [ ] Configure Git name and email
- [ ] Set up Bitwarden browser extension
- [ ] Bookmark key PetPass URLs
- [ ] Join team Slack/Discord
- [ ] Get access to shared Google Drive folder
- [ ] Install Loom for screen recording
- [ ] Test screen sharing on Zoom

---

## Getting Help

**Internal Resources:**

- Staff Hub: <https://petpass.help/hub>
- Team documentation: `/docs/handbook/`
- Team chat: [Slack/Discord channel]

**External Resources:**

- Git tutorial: <https://learngitbranching.js.org/>
- VS Code docs: <https://code.visualstudio.com/docs>
- Node.js docs: <https://nodejs.org/docs>
- React docs: <https://react.dev>

**When asking for help, include:**

1. What you were trying to do
2. What command you ran
3. The full error message (screenshot or copy-paste)
4. Your OS version (Windows 10/11)
5. Output of `pnpm doctor` or `node -v`, `pnpm -v`

---

## Tips for Success

1. **Close and reopen PowerShell** after installing new tools (to refresh PATH)
2. **Always `git pull`** before starting work to get latest code
3. **Run `pnpm install`** after pulling if you see “module not found” errors
4. **Use branches** for testing—never commit directly to main
5. **Back up `.env.local`** to a secure location (it’s not in Git)
6. **Keep VS Code updated**—it auto-updates by default
7. **Use the terminal in VS Code** (Ctrl + `) instead of external PowerShell
8. **Save often**—VS Code can auto-save on focus change
9. **Use Git branches to test features** without affecting main code
10. **Take screenshots of errors** before asking for help

---

**Last updated:** November 2025  
**Maintainer:** PetPass Development Team  
**Questions?** Post in #dev-help channel
