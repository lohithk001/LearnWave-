# 🎓 LearnWave – AI-Powered Academic Social Platform

A comprehensive, production-ready learning management system built with Next.js, featuring AI-powered assistance, a full gamification engine, real-time community features, and peer leaderboards.

---

## ✨ Technology Stack

### 🎯 Core Framework
- **⚡ Next.js 16 (Turbopack)** – Full-stack React framework with App Router
- **📘 TypeScript 5** – Type-safe development across frontend and backend
- **🎨 Vanilla CSS + Tailwind utilities** – Custom design system with HSL-balanced tones

### 🧩 UI & Components
- **🧩 shadcn/ui** – Accessible components built on Radix UI
- **🎯 Lucide React** – Consistent icon library
- **🌈 Framer Motion** – Production-ready animations

### 🗄️ Database & Backend
- **🗄️ Prisma ORM** – Type-safe database access with PostgreSQL (Neon)
- **🔐 Custom JWT Auth** – Cookie + Bearer token authentication
- **🚦 Rate Limiting** – Per-IP request throttling on auth and voting endpoints

### 🤖 AI Integration
- **🧠 Google Gemini AI** – Academic assistant with RAG (PDF Q&A) support
- **🗣️ Web Speech API** – Voice navigation and commands

---

## 🏗️ Architecture

LearnWave is a monolithic full-stack Next.js application. Frontend and API routes coexist in the same repo.

### 📁 Project Structure

```
learnwave099/
├── src/
│   ├── app/
│   │   ├── page.tsx              # Landing page (/)
│   │   ├── auth/                 # Login / Register pages
│   │   ├── student/              # Student dashboard (SPA-style, all pages rendered client-side via ?page= param)
│   │   ├── mentor/               # Mentor dashboard
│   │   ├── admin/                # Admin dashboard
│   │   └── api/                  # All backend API routes
│   │       ├── auth/             # login, register, logout, me
│   │       ├── student/          # profile, avatar, settings, community, quiz, dashboard
│   │       ├── gamification/     # progress, leaderboard, achievements, missions, avatars
│   │       ├── community/        # college-rankings
│   │       ├── resources/        # list, upload, rate (voting), process (RAG)
│   │       ├── ai/               # chat, rag-chat, summarize, explain, generate-quiz
│   │       ├── messages/         # rooms, send, room detail
│   │       ├── sessions/         # booking, cancel
│   │       └── admin/            # analytics, users, mentor approval, settings
│   ├── components/
│   │   ├── student/              # Dashboard, Profile, Community, Settings, LevelPath, etc.
│   │   ├── mentor/               # MentorDashboard, MentorSettings, etc.
│   │   ├── admin/                # AdminDashboard, etc.
│   │   └── ui/                   # shadcn base components
│   ├── lib/
│   │   ├── db.ts                 # Prisma client singleton (with model aliasing)
│   │   ├── auth.ts               # JWT helpers (generateToken, verifyToken)
│   │   ├── xp-engine.ts          # XP award, level-up, reward grant logic
│   │   ├── xp-config.ts          # Level formula, MAX_LEVEL, xpForLevel
│   │   └── streak-engine.ts      # Login streak tracking
│   └── hooks/                    # useStudentProfile, useAuth, useVoice, etc.
├── prisma/
│   └── schema.prisma             # Full DB schema (users, gamification, community, etc.)
├── tests/
│   ├── api/smoke.test.ts         # API smoke tests (auth, XP, community post)
│   ├── lib/auth.test.ts          # JWT + password hashing unit tests
│   └── metrics/
│       └── performance.test.ts   # Latency SLAs, Gamification formula, & Bcrypt benchmarks
└── .github/workflows/ci.yml      # CI: typecheck → test → production build
```

### 🔄 Request Flow
```
[User Action in UI]
     ↓
[API Call to /api/... route handler]
     ↓
[JWT Auth check (cookie or Bearer header)]
     ↓
[Prisma DB query on Neon PostgreSQL]
     ↓
[JSON Response → React State Update]
```

---

## 🎨 Key Features

### 🎓 For Students

#### 📊 Dashboard & Profile
- Personalized dashboard with academic overview
- Profile card showing **level, XP badge, equipped title, and progress bar** (XP relative to next level threshold)
- Avatar upload (Base64 DB fallback on serverless; local file on dev)
- Equipped cosmetic **frame** rendered around the avatar

#### 🏆 Gamification System
- **XP Engine** – Earn XP for: Login (+10), Watch Video (+25), Forum Post (+10), Quiz Completed (+50), Quiz Score >90% (+30), Assignment Submitted (+80)
- **Level Formula** – `level = floor(sqrt(xp / 100)) + 1`, capped at **Level 50**
- **Level-Up Rewards** – Cosmetic frames (Bronze → Silver → Gold → Platinum → Diamond → Legendary) and milestone titles auto-granted on level-up
- **Level Path** – Full visual roadmap of all 50 level rewards, showing locked/unlocked state
- **Daily Caps** – Login XP capped at 1×/day, video watching at 5×/day, forum posts at 5×/day

#### 🏅 Leaderboard & Rankings
- **Global Leaderboard** – All-time top students by XP
- **Monthly Leaderboard** – Monthly XP ranking
- **Branch Leaderboard** – Peer ranking within enrolled branch
- **Semester filter** – Filter any leaderboard by semester (1–8)
- **College Rankings** – Aggregated college score rankings

#### 🌐 Community
- Social feed with post creation, likes, comments count
- Hashtag tagging on posts
- Study groups and campus events sidebar panels
- XP awarded for every forum post created

#### 📚 Resources
- Browse, filter and download study notes and past papers
- Inline PDF viewer (no download required)
- **Resource Voting** – Thumbs up/down rating on each resource card, persisted in DB
- Virtual Library tab for textbooks and reference materials

#### 🤖 AI Assistant
- Multi-provider AI chat (Gemini)
- **RAG mode** – answers cited page-by-page from uploaded PDFs (toggle "Use Internal Resources")
- Assignment help, concept explanation, and quiz generation

#### ⚙️ Settings (Tabbed Layout)
- **Profile Info** – Name, email, phone, university, college
- **Customization** – Choose equipped cosmetic frame and title from unlocked items
- **Security** – Change password
- **Notifications** – Toggle system notifications
- **UI Appearance** – Switch between Sidebar and Bottom Dock navigation layout

#### 💬 Messaging
- Real-time chat rooms by branch and year
- Direct messages and session-linked rooms

### 👨‍🏫 For Mentors
- **Premium Dark Dashboard** – Live stats: bookings, students, ratings, weekly contribution heatmap
- **Session Management** – Manage availability and time slots
- **Earnings Portal** – Transaction ledger and payout analytics
- **Resource Uploads** – Submit notes classified by VTU scheme, branch, semester, subject, module
- **Profile & Settings** – Bio, hourly rate, profile picture

### 👑 For Admins
- **Analytics Dashboard** – User counts, session stats, revenue
- **User Management** – Paginated directory with search and delete
- **Mentor Approvals** – Approve/reject pending mentor applications
- **Announcement Editor** – Publish system-wide announcements with priority
- **Resource Hub** – Upload and approve pending mentor submissions
- **Platform Settings** – Maintenance mode toggles per role

---

## 🚀 Quick Start

### Install & Run
```bash
# Install dependencies
npm install --legacy-peer-deps

# Generate Prisma client
npx prisma generate

# Push schema to database
npx prisma db push

# Start dev server
npm run dev
```

### Database Management
```bash
npx prisma db push          # Sync schema to DB
npx prisma migrate dev       # Create migration
npx prisma studio            # Browse tables in browser
```

### Seed Cosmetics (Milestone Rewards)
```bash
npx tsx scripts/seed/seed-cosmetics.ts
```

### 🔐 Test Credentials
| Role    | Email                     | Password    |
|---------|---------------------------|-------------|
| Admin   | `admin@learnwave.com`     | `admin123`  |
| Student | `student@test.com`        | `student123`|
| Mentor  | `mentor@test.com`         | `mentor123` |

---

## 🎤 Voice Navigation

1. Toggle **"🎤 Voice Navigation"** in the sidebar.
2. Speak commands in English:
   - **Student**: `home`, `dashboard`, `announcements`, `resources`, `mentors`, `ai assistant`, `community`, `quiz`
   - **Mentor**: `students`, `sessions`, `earnings`, `profile`, `settings`
   - **Admin**: `users`, `analytics`, `announcements`, `settings`

---

## 🤖 RAG – PDF Q&A

1. Upload PDF notes in the Resource Hub.
2. Process PDFs to extract chunks:
   ```bash
   npm run process-pdfs
   ```
3. Toggle **"Use Internal Resources"** on the AI Assistant page.
4. Ask questions – answers are cited by source page.

### Key RAG Endpoints
- `POST /api/resources/process` – Process a PDF by `resourceId`
- `POST /api/ai/rag-chat` – RAG query with optional `subject` and `semester` filters

---

## 🚦 CI/CD & Testing Pipelines

GitHub Actions workflow (`.github/workflows/ci.yml`) runs on every push to `main`:

```
npm ci → npx prisma generate → npx tsc --noEmit → npx vitest run → npx next build
```

The workspace maintains **11 active tests** spanning unit, integration, and performance SLA benchmarks.

### ⚡ Performance & Security Latency Metrics (SLAs)

We track system performance SLAs using automated benchmark unit tests:
* **JWT Cryptographic Throughput**: Token generation and validation execute at high speeds, averaging **~0.45ms** per roundtrip token operation (SLA threshold: `< 10ms`).
* **Gamification Engine Calculations**: Score calculations, titles, and XP-to-Level formula updates execute in sub-millisecond ranges, averaging **~29.66μs** per iteration (SLA threshold: `< 50μs`).
* **Bcrypt Security SLA**: Monitored password hashing execution cost ensures hashing takes **~450ms**, satisfying the secure delay margin (**> 30ms** to resist brute forcing, and **< 1000ms** to prevent API timeouts).

To run the performance metrics suite:
```bash
npm run test:metrics
```

### 📈 Code Coverage Metrics

The codebase uses `@vitest/coverage-v8` to track branch and statement coverage. Core services maintain high test coverage:
* **XP Config Engine**: **100%** code coverage
* **Auth Helper Layer**: **88.88%** code coverage
* **API Route Handlers**: Average **~72.22%** overall statements coverage

To run the coverage metrics pipeline:
```bash
npm run test:coverage
```

---

## 🎨 Design System

- **Student / Admin**: Warm beige tones (`#FDFBF9` bg, `#6B5844` accent)
- **Mentor**: Premium dark navy with glassmorphism and Recharts analytics
- **Gamification UI**: Cyan XP badges, gold level indicators, gradient level path roadmap
- **Layout Modes**: Bottom Dock or Side Sidebar (user-configurable in Settings)
- **Animations**: Micro-transitions on tab switches, hover states, XP bar fill

---

## 🆘 Troubleshooting

| Issue | Solution |
|-------|----------|
| Avatar shows broken image on Vercel | Expected for old `/uploads/` paths – new uploads auto-fallback to Base64 in DB |
| Gemini AI not responding | Check `GEMINI_API_KEY` in `.env.local`. Test: `npx tsx scripts/test-gemini.ts` |
| Microphone not working | Allow browser mic permissions; works best in Chrome/Edge |
| DB tables out of sync | Run `npx prisma migrate reset` to clean and reseed |
| `community_posts` undefined in tests | Ensure db mock includes `community_posts: { create: vi.fn() }` |
