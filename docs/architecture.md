# System Architecture

> **Version:** 1.0
> **Date:** 2026-03-13
> **Author:** CTO-Architect Agent
> **Status:** Draft — awaiting founder review

---

## 1. Architecture Overview

**Pattern: Static Single-Page Application (SPA) — no backend for Pilot.**

Treasure of Tens is a client-side-only web application for Pilot. All game logic, state, and data live in the browser. The app is built as a static bundle deployed to a CDN — no application server, no database, no API. This is the simplest architecture that satisfies every Pilot requirement.

**Why this pattern:**

| Requirement | How the static SPA satisfies it |
|---|---|
| No student login / COPPA compliance | Zero data collection. Progress in localStorage. No server = no data to breach. |
| Solo founder / minimal ops | No servers to manage, no database to backup, no uptime to monitor. CDN handles availability. |
| Browser-based (Chrome + Safari) | React SPA runs in any modern browser. |
| ≤ 5 MB bundle | Achievable with code splitting and lazy-loaded audio. |
| ≤ 2s page load on school Wi-Fi | Static assets on CDN + lazy loading. |
| 500 concurrent students | CDN scales horizontally with zero config. No server bottleneck. |

**What triggers a migration to client-server:**

The Pilot architecture is intentionally server-free. A backend becomes necessary when:
1. **V1 Teacher Dashboard** — requires server-side data storage, teacher auth, and a REST API.
2. **V1 Student Profiles** — progress must persist across devices, requiring server-side state.
3. **V1 Adaptive Difficulty** — if the adaptation logic needs cross-session analytics beyond what localStorage provides.

When V1 begins, add a lightweight backend (Node.js + PostgreSQL on a managed platform). The client-side game engine remains unchanged — it gains an optional sync layer that pushes events to the server.

---

## 2. System Components

### 2.1 Game Shell

**Responsibility:** Application entry point, routing, global providers (i18n, audio context, theme), and screen orchestration.

**Technology:** React 18 + TypeScript, client-side routing via React Router.

**Interfaces:**
- Renders one of 5 screens based on route: Title, Map, Gameplay, LevelComplete, Settings.
- Provides i18n context (Hebrew default), audio context, and game-state context to all children.

**Data Store:** None directly — delegates to GameStateManager.

### 2.2 Gameplay Engine

**Responsibility:** Core game mechanics — gem rendering, splitter drag interaction, group combination, Power Gem formation, equation generation, and hint triggering. This is the heart of the application.

**Technology:** React components with DOM-based rendering. Drag interactions via `@use-gesture/react`. Animations via CSS transitions, CSS keyframes, and the Web Animations API (WAAPI).

**Interfaces:**
- Receives a `LevelDefinition` (from static JSON).
- Emits gameplay events (split, combine, complete, hint) to the EventLogger and GameStateManager.
- Reads hint configuration from the LevelDefinition.

**Data Store:** Transient in-memory state only (current level progress). Persisted state delegated to GameStateManager.

**Key sub-components:**

| Component | Role |
|---|---|
| `GemCluster` | Renders N gems in a line or 10-frame grid. Accepts a splitter position. |
| `Splitter` | Draggable divider within a GemCluster. Snaps to valid split points (between gems). |
| `GemGroup` | A sub-group after splitting. Draggable for combining. Displays count label. |
| `CombineZone` | Drop target where groups merge. Detects when count = 10 → triggers PowerGem. |
| `PowerGem` | Animated fused gem representing 10. Fills treasure chest slot. |
| `EquationBar` | Displays the decomposition equation after confirmation. |
| `HintOverlay` | Renders Tier 1 ghost splitter or Tier 2 walkthrough animation. |
| `NarratorBubble` | Speech bubble from the explorer character with narration text. |

### 2.3 Level Data System

**Responsibility:** Defines all level content — numbers, valid decomposition paths, hint positions, narration keys, and world metadata. Pure data, no logic.

**Technology:** Static JSON files bundled at build time, TypeScript types for validation.

**Interfaces:**
- Exposes `getLevelDefinition(worldId, levelId): LevelDefinition`.
- Exposes `getWorldDefinition(worldId): WorldDefinition` (level order, theme, intro narration).

**Data Store:** Imported JSON modules. ~50 KB total for Pilot (3 worlds, 15–20 levels).

```typescript
interface LevelDefinition {
  id: string;                    // e.g., "w1-l3"
  worldId: number;               // 1, 2, or 3
  type: 'free-split' | 'make-ten' | 'multi-addend';
  numbers: number[];             // e.g., [7, 5] or [9, 7, 5, 2]
  validTargets?: number;         // target group size (10 for World 2–3)
  requiredSplits?: number;       // min unique splits to complete (World 1)
  narrationKeys: {
    intro: string;               // i18n key for level intro
    success: string;             // i18n key for completion
    hint: string;                // i18n key for hint prompt
  };
  hintSplitPositions: number[];  // valid split indices for Tier 1 hint
}

interface WorldDefinition {
  id: number;
  nameKey: string;               // i18n key
  levels: string[];              // ordered level IDs
  theme: 'cave-entrance' | 'crystal-cavern' | 'treasure-vault';
}
```

### 2.4 Game State Manager

**Responsibility:** Tracks player progress (completed levels, current position on map), persists to localStorage, and provides state to all screens.

**Technology:** React Context + `useReducer`. Persistence via a thin localStorage adapter with JSON serialization.

**Interfaces:**
- `GameState` context consumed by Map screen and Gameplay screen.
- `dispatch(action)` for state mutations (completeLevel, startSession, etc.).
- Auto-saves to localStorage on every state change via `useEffect`.

**Data Store:** `localStorage` under key `treasure-of-tens-state`.

```typescript
interface GameState {
  sessionId: string;            // random UUID, regenerated per session
  completedLevels: string[];    // level IDs
  currentWorldId: number;
  currentLevelId: string | null;
  settings: {
    narrationEnabled: boolean;
    sfxEnabled: boolean;
  };
}
```

### 2.5 Audio Manager

**Responsibility:** Plays sound effects (SFX), manages narration (TTS), handles volume ducking (SFX ducks when narration plays), and respects user mute settings.

**Technology:** Howler.js for SFX (cross-browser audio sprites, volume control). Web Speech API (`speechSynthesis`) for Hebrew TTS narration with cloud TTS fallback if browser Hebrew voice quality is poor.

**Interfaces:**
- `playSfx(name: SfxName): void` — fire-and-forget SFX.
- `narrate(textKey: string): Promise<void>` — speaks localized text, resolves when done.
- `setNarrationEnabled(on: boolean): void`
- `setSfxEnabled(on: boolean): void`

**Data Store:** Audio sprite files (lazy-loaded, ~500 KB total). No persistent state beyond settings in GameState.

**Browser autoplay handling:** On first user interaction (title screen "Play" tap), call `Howler.ctx.resume()` and `speechSynthesis.speak(empty)` to unlock audio contexts. Display a "tap to start" splash if `Howler.ctx.state === 'suspended'`.

### 2.6 i18n System

**Responsibility:** Externalizes all UI text and narration scripts. Hebrew is the default and only Pilot locale. Architecture supports adding EN/ES locales by adding JSON files — no code changes.

**Technology:** `react-i18next` with JSON resource bundles.

**Interfaces:**
- `t(key: string, params?: object): string` — translation hook.
- `useDirection(): 'rtl' | 'ltr'` — derived from current locale.

**Data Store:** Locale JSON files:
```
/src/locales/
  he.json          ← Pilot (Hebrew)
  en.json          ← V1 (English, placeholder)
```

**RTL strategy:**
- Set `<html dir="rtl" lang="he">` from i18n config.
- Use CSS logical properties (`margin-inline-start`, `padding-inline-end`, `inset-inline-start`) instead of physical (`margin-left`, `padding-right`, `left`). This makes the layout automatically flip when `dir` changes to `ltr`.
- Math notation (equations, number lines) uses an explicit `dir="ltr"` override on its container.

### 2.7 Event Logger (Analytics)

**Responsibility:** Records anonymous gameplay events for Pilot success metrics. No third-party SDKs. No PII. COPPA-safe by design.

**Technology:** Custom lightweight module. In Pilot, events are stored in localStorage (alongside game state) and can be exported as JSON by the founder for analysis. In V1, events are posted to the backend API.

**Interfaces:**
- `logEvent(event: GameEvent): void`

**Data Store:** localStorage under key `treasure-of-tens-events`.

```typescript
interface GameEvent {
  type: 'session_start' | 'session_end' | 'level_start' | 'level_complete'
        | 'split_action' | 'combine_action' | 'hint_triggered';
  timestamp: number;
  sessionId: string;
  levelId?: string;
  data?: Record<string, unknown>;  // e.g., { splitPosition: 3, groupSizes: [5, 3] }
}
```

---

## 3. Tech Stack

| Layer | Choice | Rationale | Alternatives Considered |
|---|---|---|---|
| Language | **TypeScript 5** | Type safety catches bugs before runtime. Essential for a solo developer who can't rely on code review from others. | JavaScript (faster to write but error-prone at scale) |
| Framework | **React 18** | Largest ecosystem, best hiring pool if team grows, excellent accessibility tooling, mature. Component model maps cleanly to game UI elements. | Vue 3 (smaller bundle but smaller ecosystem); Svelte (excellent perf but less mature tooling); Phaser/PixiJS (canvas-based — better for complex games but worse for accessibility, no native DOM for WCAG) |
| Build Tool | **Vite 6** | Fast dev server (HMR < 100ms), optimized production builds, tree-shaking, code splitting. De facto standard for React projects. | Webpack (slower, more config); esbuild (no HMR dev server); Parcel (less configurable) |
| Styling | **CSS Modules + CSS Custom Properties** | Scoped styles prevent leaks. Custom properties enable theming (gem colors, dark mode future). Logical properties handle RTL natively. Zero runtime cost. | Tailwind (utility-first doesn't suit custom game UI); styled-components (runtime CSS-in-JS, bundle cost); Emotion (same concern) |
| Drag & Gesture | **@use-gesture/react** | Lightweight (~8 KB), works with DOM elements, supports drag/pinch/scroll gestures. Pairs with CSS transforms for animation. | react-dnd (heavier, designed for list reordering not game drag); Framer Motion drag (larger bundle, 30+ KB); Custom pointer events (more code to maintain) |
| Animation | **CSS Animations + Web Animations API** | Zero-dependency. CSS handles transitions (gem glow, color changes). WAAPI handles scripted sequences (Power Gem fusion, celebration particles). 60fps on target devices. | Framer Motion (~35 KB, capable but large for this use case); GSAP (powerful but licensed, heavy); react-spring (~25 KB, spring physics overkill here) |
| State Management | **React Context + useReducer** | Sufficient for Pilot's simple state tree (completed levels, settings). No external dependency. Easy to understand and debug. | Redux (overkill for this scale); Zustand (nice API but unnecessary dependency); Jotai (atomic model unnecessary here) |
| i18n | **react-i18next** | Industry standard for React i18n. JSON resource bundles, interpolation, pluralization. Supports RTL via locale metadata. | react-intl/FormatJS (comparable but less flexible resource loading); Custom solution (unnecessary reinvention) |
| Audio (SFX) | **Howler.js** | Mature cross-browser audio library (~10 KB). Audio sprites, volume control, format fallback (WebM → MP3). Solves mobile autoplay quirks. | Tone.js (synthesizer-focused, overkill); Web Audio API raw (too much boilerplate); HTML5 Audio (inconsistent cross-browser) |
| Audio (TTS) | **Web Speech API** (primary) / **Cloud TTS** (fallback) | Free, no API keys, works offline. Hebrew voice quality varies — test during development. If inadequate, switch to Google Cloud TTS (pre-generate audio files, not runtime API calls). | Pre-recorded human narration (warm but expensive to iterate on; target for V1 upgrade); All cloud TTS (adds API dependency and cost) |
| Testing | **Vitest + React Testing Library + Playwright** | Vitest is Vite-native (fast, compatible config). RTL encourages accessible component tests. Playwright for cross-browser E2E (Chrome + Safari). | Jest (slower, needs separate config from Vite); Cypress (heavier, weaker Safari support) |
| Linting | **ESLint + Prettier** | Standard code quality tooling. `eslint-plugin-jsx-a11y` enforces accessibility rules at lint time. | Biome (newer, fast, but less mature plugin ecosystem) |
| Hosting | **Cloudflare Pages** (recommended) | Free tier covers Pilot needs. Global CDN, HTTPS, preview deployments, < 50ms TTFB. No server to manage. | Vercel (excellent but bandwidth limits on free tier); Netlify (comparable, slightly higher latency in Israel); GitHub Pages (no preview deployments, slower builds) |
| CI/CD | **GitHub Actions** | Free for public repos, generous minutes for private. Runs lint → test → build → deploy pipeline. | GitLab CI (comparable); CircleCI (unnecessary migration) |
| Containerization | **Not needed** | Static site deployed directly to CDN. No runtime to containerize. | Docker (adds complexity for zero benefit in a static deployment) |
| ML Serving | **Not applicable** | No ML in Pilot per Decision #10 and PRD scope. | — |
| Monitoring | **Client-side error boundary + Sentry free tier** | React error boundaries catch rendering crashes gracefully (show "oops" screen instead of white screen). Sentry free tier (10K events/month) captures unhandled exceptions with stack traces — no PII, COPPA-safe. | LogRocket (records sessions — COPPA concern); Custom error logging (less actionable without stack traces); No monitoring (unacceptable — students can't report bugs) |

---

## 4. Data Architecture

### 4.1 Data Model

**Pilot has no database.** All data is either static (bundled at build time) or ephemeral (browser localStorage).

#### Static Data (bundled JSON)

```
Level Definitions ──────────────────────────────────────────────
  WorldDefinition (1:many) → LevelDefinition

  WorldDefinition { id, nameKey, levels[], theme }
  LevelDefinition { id, worldId, type, numbers[], validTargets,
                    requiredSplits, narrationKeys, hintSplitPositions[] }
```

#### Client-Side Persisted Data (localStorage)

```
GameState ──────────────────────────────────────────────────────
  { sessionId, completedLevels[], currentWorldId, currentLevelId,
    settings: { narrationEnabled, sfxEnabled } }

EventLog ───────────────────────────────────────────────────────
  GameEvent[] — append-only array of anonymous gameplay events
```

#### Entity Relationship (Pilot)

```
┌──────────────┐       ┌──────────────────┐
│  World (3)   │──1:N──│  Level (15–20)   │
│  id, name,   │       │  id, type,       │
│  theme       │       │  numbers[],      │
└──────────────┘       │  hints, narration│
                       └──────────────────┘
                              │
                         played by
                              │
                       ┌──────────────────┐
                       │ GameState (1)    │  ← localStorage
                       │ completedLevels[]│
                       │ settings         │
                       └──────────────────┘
                              │
                         generates
                              │
                       ┌──────────────────┐
                       │ EventLog (N)     │  ← localStorage
                       │ type, timestamp, │
                       │ sessionId, data  │
                       └──────────────────┘
```

### 4.2 Data Flow

```
[Build Time]
  Level JSON files ──→ Vite bundle ──→ CDN

[Runtime — Pilot]
  CDN ──→ Browser loads SPA
       ──→ React hydrates, reads GameState from localStorage
       ──→ Student plays level
            ├─→ Gameplay Engine processes interactions in memory
            ├─→ GameStateManager writes progress to localStorage
            └─→ EventLogger appends events to localStorage

[Founder Analytics — Pilot]
  Founder opens browser DevTools or a hidden /export route
  ──→ Exports EventLog as JSON file
  ──→ Analyzes in spreadsheet or script
```

### 4.3 Data Governance

| Concern | Pilot Approach | V1 Upgrade |
|---|---|---|
| **PII** | None collected. Zero PII in localStorage. Session IDs are random UUIDs with no user linkage. | Student pseudonyms (teacher-assigned) stored server-side. Still no real names or emails for students. |
| **Encryption at rest** | Not applicable — localStorage is device-local, protected by OS/browser security. | PostgreSQL with encryption at rest (managed service default). |
| **Encryption in transit** | HTTPS enforced by CDN (Cloudflare). | Same + API over HTTPS with TLS 1.3. |
| **Data retention** | localStorage persists until cleared. No server-side retention. | 1 academic year retention. Auto-purge after 12 months. Teacher can request deletion. |
| **Data residency** | Client-side only — data stays on the student's device. | Server hosted in EU region (for Israeli data proximity and GDPR alignment). |
| **Backup** | None needed — no server data. localStorage loss is an accepted Pilot risk (PRD Assumption #6). | Daily automated database backups, 30-day retention. |

---

## 5. API Design

### 5.1 API Style

**Pilot: No API.** The application is entirely client-side.

**V1: REST API** (when backend is added for teacher dashboard and student profiles).

REST is chosen over GraphQL because:
- The data model is simple (students, classes, levels, events) — no complex nested queries needed.
- REST is simpler to implement, debug, and cache for a solo developer.
- Standard HTTP caching (ETags, Cache-Control) works naturally with CDN.

### 5.2 Key Endpoints (V1 — planned, not Pilot)

These are documented here for forward-planning. They do not exist in Pilot.

| Method | Path | Purpose |
|---|---|---|
| `POST` | `/api/auth/login` | Teacher login (email + password) |
| `POST` | `/api/auth/logout` | Teacher logout |
| `GET` | `/api/classes` | List teacher's classes |
| `POST` | `/api/classes` | Create a class |
| `GET` | `/api/classes/:id/students` | List students in a class |
| `POST` | `/api/classes/:id/students` | Add a student (pseudonym) |
| `GET` | `/api/students/:id/progress` | Student progress (levels, attempts, hints) |
| `POST` | `/api/events` | Batch-upload gameplay events from client |
| `GET` | `/api/classes/:id/analytics` | Class-level analytics (hotspot levels, completion rates) |

### 5.3 Authentication & Authorization (V1 — planned)

| Concern | Approach |
|---|---|
| **Teacher auth** | Email + password with bcrypt hashing. JWT access tokens (15-min expiry) + HTTP-only refresh tokens (7-day expiry). |
| **Student auth** | None. Students access the game via a class-specific URL with an embedded class code. No login. |
| **RBAC** | Two roles: `teacher` (manages own classes, views own students) and `admin` (founder, manages all). Teachers cannot see other teachers' classes. |
| **API auth** | Bearer token in `Authorization` header. Event upload endpoint authenticated by class code (not teacher JWT) so the game client doesn't need teacher credentials. |

---

## 6. Integration Architecture

**Pilot: No external integrations.** The application is fully standalone.

**Future integrations (V1/V2):**

| Integration | Phase | Connection Method | Purpose |
|---|---|---|---|
| Google Cloud TTS | Pilot (if needed) | Pre-generated audio files at build time, not runtime API | High-quality Hebrew narration if Web Speech API is inadequate |
| Sentry | Pilot | Client-side JS SDK (~12 KB) | Error monitoring, crash reporting |
| Google Classroom | V2 | OAuth 2.0 + REST API | Roster sync for teacher dashboard |
| LMS (LTI 1.3) | V2 | LTI protocol | Embed game in school LMS platforms |
| Clever SSO | V2 | OAuth 2.0 / SAML | Single sign-on for school districts |

---

## 7. Infrastructure & Deployment

### 7.1 Deployment Topology

```
┌────────────────────────────────────────────────────┐
│                  Cloudflare Pages                   │
│                                                    │
│   ┌──────────┐    ┌──────────┐    ┌──────────┐   │
│   │  Edge    │    │  Edge    │    │  Edge    │   │
│   │  (EU)    │    │  (US)    │    │  (Asia)  │   │
│   └──────────┘    └──────────┘    └──────────┘   │
│         │                                         │
│         ▼                                         │
│   Static Assets:                                  │
│   - index.html                                    │
│   - JS bundles (code-split by route)              │
│   - CSS                                           │
│   - Audio sprites (lazy-loaded)                   │
│   - Gem/character images (lazy-loaded)            │
│   - Level JSON (bundled)                          │
│   - Locale JSON (bundled)                         │
└────────────────────────────────────────────────────┘
         │
         │ HTTPS
         ▼
┌────────────────────────────────────────────────────┐
│              Student's Browser                     │
│                                                    │
│   React SPA ──→ localStorage (state + events)     │
│                                                    │
│   No server calls during gameplay                  │
└────────────────────────────────────────────────────┘
```

**Environments:**

| Environment | URL | Purpose | Deploy Trigger |
|---|---|---|---|
| Production | `treasureoftens.com` (or similar) | Live game for pilot students | Merge to `main` branch |
| Preview | `<branch>.treasureoftens.pages.dev` | Review before merge | Push to any branch |
| Local Dev | `localhost:5173` | Development | `npm run dev` |

### 7.2 CI/CD Pipeline

```
Push to GitHub
  │
  ├─→ GitHub Actions workflow triggers
  │     │
  │     ├─ Step 1: Install dependencies (npm ci)
  │     ├─ Step 2: Lint (ESLint + Prettier check)
  │     ├─ Step 3: Type check (tsc --noEmit)
  │     ├─ Step 4: Unit + component tests (vitest)
  │     ├─ Step 5: Build (vite build)
  │     ├─ Step 6: Bundle size check (fail if > 5 MB)
  │     └─ Step 7: E2E tests (Playwright on Chrome + WebKit)
  │
  ├─→ On PR: Cloudflare Pages creates preview deployment
  │
  └─→ On merge to main: Cloudflare Pages deploys to production
```

**Build time target:** < 2 minutes for the full pipeline.

### 7.3 Infrastructure as Code

**Not needed for Pilot.** Cloudflare Pages is configured via the dashboard (one-time setup) and a `wrangler.toml` for build settings. No Terraform, CDK, or Pulumi.

When V1 adds a backend, use **Terraform** to manage:
- Managed PostgreSQL (e.g., Neon, Supabase, or Railway)
- Backend hosting (e.g., Fly.io or Railway)
- DNS records
- Environment secrets

---

## 8. Non-Functional Requirements (NFRs)

| Requirement | Target | Measurement | Priority |
|---|---|---|---|
| **Page load (LCP)** | ≤ 2s on 10 Mbps connection | Lighthouse CI in GitHub Actions; field measurement via `PerformanceObserver` | P0 |
| **Interaction latency** | ≤ 100ms drag response; 60fps animation | Chrome DevTools Performance panel; Playwright perf assertions | P0 |
| **Bundle size** | ≤ 5 MB compressed (all assets) | `vite build` output; CI size check gate | P0 |
| **Availability** | 99.9% (CDN-provided) | Cloudflare analytics dashboard | P1 |
| **Throughput** | 500 concurrent students | CDN handles this by default; no server bottleneck | P2 |
| **Recovery Time (RTO)** | < 5 minutes | CDN auto-heals; rollback = redeploy previous build | P2 |
| **Recovery Point (RPO)** | N/A (no server data) | localStorage is device-local | N/A |
| **Browser support** | Chrome 90+ (Chromebook), Safari 15+ (iPad) | Playwright E2E on both engines | P0 |
| **Accessibility** | WCAG 2.1 AA | axe-core in component tests; manual audit pre-pilot | P0 |

---

## 9. Security Architecture

### 9.1 Threat Model

| Attack Surface | Threat | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| **Static assets on CDN** | Asset tampering / CDN compromise | Very Low | Medium | Cloudflare provides DDoS protection and edge TLS. Subresource Integrity (SRI) hashes on script tags. |
| **localStorage** | Another site reads game data | Very Low | Very Low | localStorage is origin-scoped by browser. Data is non-sensitive (completed levels, anonymous events). |
| **localStorage** | Student clears data intentionally | Medium | Low | Accepted Pilot risk. V1 adds server-side persistence. |
| **JavaScript bundle** | XSS via injected content | Very Low | Medium | No user-generated content. All text comes from i18n bundles (static). React auto-escapes JSX. CSP header disallows inline scripts. |
| **Audio autoplay** | Browser blocks audio | High | Low | One-time "tap to start" interaction unlocks audio context. Tested across Chrome + Safari. |
| **Third-party dependencies** | Supply chain attack via npm | Low | High | `npm audit` in CI. Dependabot alerts enabled. Minimal dependency count (see Section 3). Lock file committed. |

### 9.2 Encryption

| Context | Approach |
|---|---|
| **In transit** | HTTPS enforced by Cloudflare (TLS 1.3). HSTS header with 1-year max-age. |
| **At rest** | No server-side data in Pilot. localStorage protected by OS-level device encryption (if enabled). |
| **Key management** | No application-level keys in Pilot. V1: secrets stored in environment variables, never in code. |

### 9.3 Access Control

**Pilot:** No access control needed. The game is a static site — anyone with the URL can play. This is intentional: no login means no authentication to implement, no credentials to protect, and full COPPA compliance.

**V1:** Teacher dashboard behind JWT-authenticated login. Students access game via class code URL (no login). RBAC enforced at API layer — teachers see only their own classes.

### 9.4 Audit & Logging

| What | Where | Retention | Tamper-Resistance |
|---|---|---|---|
| Gameplay events | localStorage (Pilot) | Until cleared | None (client-side, not sensitive) |
| Client errors | Sentry free tier | 90 days (Sentry default) | Sentry-managed |
| Build/deploy logs | GitHub Actions + Cloudflare | 90 days | Platform-managed |

### 9.5 Content Security Policy

```
Content-Security-Policy:
  default-src 'self';
  script-src 'self';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data:;
  media-src 'self';
  font-src 'self';
  connect-src 'self' https://*.sentry.io;
  frame-ancestors 'none';
```

`'unsafe-inline'` for styles is needed for CSS-in-JS custom properties set via `element.style`. If possible, eliminate this by using class-based theming instead.

---

## 10. ML Infrastructure

**Not applicable for Pilot or V1.** Per Decision Log #10 and the PRD, no ML is used until V2 at earliest. The adaptive difficulty system in V1 uses simple rule-based logic (3 clean completions → harder variant; 2 failures → easier variant), not ML.

---

## 11. Technical Risks

| # | Risk | Likelihood | Impact | Mitigation |
|---|---|---|---|---|
| 1 | **Hebrew TTS quality is poor** — Web Speech API Hebrew voices sound robotic or mispronounce math terms | High | High (narration is core to UX) | Test Hebrew TTS in Chrome + Safari during Sprint 1. If inadequate, pre-generate audio files via Google Cloud TTS at build time (~$4 for all Pilot narration). Budget 2 days for this fallback. |
| 2 | **Drag interaction is unreliable on iPad** — touch event handling differs between Chrome and Safari, especially with gestures near screen edges | Medium | High (core mechanic breaks) | `@use-gesture/react` normalizes mouse/touch/pointer events. Implement a hardware test on a real iPad early (Sprint 1). Use `touch-action: none` CSS on drag targets to prevent browser scroll interference. |
| 3 | **localStorage data loss** — school IT clears browser data, student loses progress | High | Medium (frustrating but not learning-blocking) | Accepted Pilot risk (PRD Assumption #6). Mitigate by keeping levels short (< 2 min each) so re-doing lost progress is fast. V1 adds server-side persistence. |
| 4 | **Bundle exceeds 5 MB** — audio assets and images push the bundle over budget | Medium | Medium (slow load on school Wi-Fi) | Lazy-load audio and images. Use WebP for images, Opus/WebM for audio with MP3 fallback. Code-split routes. Measure in CI and fail build if over budget. |
| 5 | **CSS logical properties not fully supported** — older Chrome/Safari versions on school devices don't support RTL logical properties | Low | Medium (broken Hebrew layout) | Browser targets (Chrome 90+, Safari 15+) fully support logical properties. Add `@supports` fallback for critical layout properties. Test on actual Chromebook hardware. |
| 6 | **Gem drag performance on low-end Chromebooks** — DOM-based drag with many elements may drop below 60fps | Medium | Medium (janky core interaction) | Profile on a low-end Chromebook (e.g., Lenovo N22). Max 9 gems per cluster in Pilot — DOM easily handles this. Use `will-change: transform` on draggable elements. Avoid layout thrashing during drag (use transforms, not top/left). |
| 7 | **Scope creep into V1 features during Pilot** — temptation to add teacher dashboard, login, etc. before validating the core game | Medium | High (delays pilot launch) | Architecture deliberately makes Pilot server-free. Adding V1 features requires adding a backend — the architectural boundary is a natural scope gate. |

---

## 12. Architecture Decision Records

### ADR-001: DOM-Based Rendering Over Canvas

**Status:** Accepted
**Date:** 2026-03-13

**Context:** The game requires interactive gem clusters with drag-to-split interactions, animations, and accessibility (WCAG 2.1 AA including aria-labels and keyboard navigation). Should the gameplay area use HTML/DOM elements or an HTML5 Canvas (via PixiJS, Phaser, etc.)?

**Decision:** Use DOM-based rendering with React components for all game elements.

**Rationale:**
- **Accessibility is a hard requirement.** Canvas elements are opaque to screen readers and require a parallel hidden DOM for aria-labels, keyboard focus, and tab order — effectively doubling the implementation work.
- **The visual complexity is low.** Pilot has at most 9 gems per cluster, a splitter line, and a combine zone. This is well within DOM rendering performance.
- **Single technology stack.** DOM rendering means the same React/TypeScript/CSS skills apply to both game mechanics and UI chrome. No context-switching between React for menus and PixiJS for gameplay.
- **Simpler hit testing.** DOM elements have native click/touch targets. Canvas requires manual coordinate-based hit detection.

**Alternatives Considered:**
- **PixiJS (Canvas):** Better for 100+ simultaneous animated objects, particle systems, complex sprite sheets. Rejected because our max object count is ~20 and accessibility would require a shadow DOM layer.
- **Hybrid (React UI + PixiJS gameplay):** Reduces accessibility burden for UI but still requires canvas accessibility for gameplay — the most interactive part. Rejected as worst of both worlds for this use case.

**Consequences:**
- Positive: Accessibility comes "for free" via semantic HTML and aria attributes. Simpler codebase.
- Negative: If V2+ introduces complex visual effects (50+ gems, particle systems, physics), may need to revisit. The architectural boundary is the `GemCluster` component — it can be reimplemented with Canvas internals while keeping the same React interface.

**Triggers for Revisiting:** Object count per screen exceeds 50; frame rate drops below 30fps on target devices; V2 requires complex particle/physics effects.

---

### ADR-002: No Backend Server for Pilot

**Status:** Accepted
**Date:** 2026-03-13

**Context:** The Pilot requires no student login, no teacher dashboard, and no cross-device progress sync. Should we build a backend anyway to "be ready" for V1, or ship a purely static app?

**Decision:** Ship Pilot as a static SPA with zero backend. All state in localStorage.

**Rationale:**
- **YAGNI.** Every server component adds operational burden (uptime, backups, security, cost) with zero Pilot benefit.
- **COPPA by architecture.** No server = no data collection = no COPPA compliance risk.
- **Speed.** Static site deploys in seconds. No backend provisioning, no database migrations, no auth implementation.
- **Cost.** Cloudflare Pages free tier handles Pilot traffic. A backend would require a paid hosting plan (~$5–25/month).
- **Scope gate.** Making V1 features require a backend prevents accidental scope creep.

**Alternatives Considered:**
- **Lightweight backend from day 1** (e.g., Supabase or Firebase): Would enable analytics and future features sooner. Rejected because it adds complexity, cost, and COPPA considerations for zero Pilot benefit.
- **Serverless functions for analytics only**: Would enable server-side event storage. Rejected because localStorage + manual export is sufficient for a single-school pilot.

**Consequences:**
- Positive: Simplest possible architecture. Zero ops. Zero cost. COPPA-safe by design.
- Negative: localStorage is fragile (school IT may clear it). Analytics export is manual. V1 migration requires building a backend from scratch — but the client-side game engine is unaffected.

**Triggers for Revisiting:** Pilot localStorage data loss is frequent enough to compromise success metrics. Or: founder decides to run multiple pilot schools simultaneously (needing centralized data).

---

### ADR-003: React + TypeScript + Vite as Core Stack

**Status:** Accepted
**Date:** 2026-03-13

**Context:** Selecting the primary development framework for a solo-founder, AI-agent-developed educational game that must ship quickly, run in Chrome + Safari, support RTL, and be maintainable.

**Decision:** React 18 + TypeScript 5 + Vite 6.

**Rationale:**
- **React** has the largest ecosystem, most mature accessibility tooling (`eslint-plugin-jsx-a11y`, React Testing Library), best AI coding assistant support (most training data), and widest hiring pool if the team grows.
- **TypeScript** prevents entire categories of bugs (wrong prop types, missing fields in level definitions, incorrect event shapes). Critical for a solo developer without human code reviewers.
- **Vite** is the React community standard build tool. Sub-second HMR, optimized production builds, built-in code splitting, and CSS Modules support.

**Alternatives Considered:**
- **Vue 3 + TypeScript:** Comparable capability but smaller ecosystem and community. Fewer accessibility-focused libraries.
- **Svelte/SvelteKit:** Excellent performance and DX but less mature ecosystem, fewer a11y tools, and smaller AI training data corpus (matters for AI-assisted development).
- **Vanilla TypeScript (no framework):** Maximum performance but massive boilerplate for component state, DOM updates, and accessibility. Not viable for solo development pace.

**Consequences:**
- Positive: Fast development with excellent tooling. Strong accessibility story. AI agents work well with React codebases.
- Negative: React's virtual DOM adds ~40 KB to bundle. Acceptable given the 5 MB budget.

**Triggers for Revisiting:** Only if React introduces a breaking paradigm shift (unlikely for a production-focused project) or if bundle size becomes a problem (monitor in CI).

---

### ADR-004: @use-gesture/react for Drag Interactions Over Framer Motion

**Status:** Accepted
**Date:** 2026-03-13

**Context:** The gem-splitting mechanic requires smooth drag-to-split and drag-to-combine gestures that work on both mouse (Chromebook) and touch (iPad). Several libraries can handle this.

**Decision:** Use `@use-gesture/react` (~8 KB) for gesture handling, paired with CSS transforms and the Web Animations API for visual feedback.

**Rationale:**
- **Small footprint.** 8 KB vs. 35+ KB for Framer Motion. Every KB matters for the 5 MB bundle budget.
- **Focused API.** `@use-gesture` does one thing (gesture recognition) and does it well. It outputs gesture state (position, velocity, direction) that we apply via CSS transforms — keeping animation control in our hands.
- **Cross-input normalization.** Handles mouse, touch, and pointer events transparently. Prevents the "works on desktop, breaks on iPad" problem.
- **Composable.** Works with any rendering approach (DOM, Canvas, WebGL). If ADR-001 is ever revisited, the gesture layer doesn't need to change.

**Alternatives Considered:**
- **Framer Motion:** Full animation + gesture library. Excellent DX but 35+ KB and includes animation features we're handling with CSS/WAAPI. Bundle cost not justified.
- **Custom pointer event handlers:** Zero dependency but ~200–300 lines of boilerplate for cross-input normalization, drag thresholds, velocity tracking, and touch-action handling. Maintenance burden for a solo developer.
- **react-dnd:** Designed for drag-and-drop list reordering, not freeform spatial dragging. Wrong abstraction for this use case.

**Consequences:**
- Positive: Minimal bundle impact. Clean separation between gesture logic and visual rendering.
- Negative: No built-in animation library — we own animation code via CSS/WAAPI. Acceptable because our animations are simple (transforms, opacity, scale).

**Triggers for Revisiting:** If animation complexity grows significantly in V1/V2 (chained sequences, spring physics, layout animations), evaluate upgrading to Framer Motion at that point.

---

### ADR-005: Cloudflare Pages for Hosting

**Status:** Accepted
**Date:** 2026-03-13

**Context:** Need a hosting platform for a static SPA that serves Israeli students with low latency, HTTPS, and zero operational burden.

**Decision:** Cloudflare Pages on the free tier.

**Rationale:**
- **Global CDN with Israeli PoP.** Cloudflare has an edge node in Tel Aviv, ensuring low latency for the pilot school.
- **Free tier is generous.** Unlimited bandwidth, 500 builds/month, 1 build concurrency. More than sufficient for Pilot.
- **Preview deployments.** Every PR gets a unique URL for testing before merge.
- **Zero config HTTPS.** Automatic TLS certificate provisioning and renewal.
- **Wrangler CLI.** Deploy from CI with one command.
- **Custom headers.** Can set CSP, HSTS, and caching headers via `_headers` file.

**Alternatives Considered:**
- **Vercel:** Excellent DX but free tier has 100 GB bandwidth limit. A class of 30 students loading 5 MB assets 3x/week = ~2 GB/week. Safe for Pilot but could be a concern if usage grows.
- **Netlify:** Comparable to Cloudflare Pages. No Israeli edge node (nearest is Frankfurt). Slightly higher latency.
- **GitHub Pages:** No preview deployments, slower builds, no custom headers configuration.

**Consequences:**
- Positive: Zero cost, low latency in Israel, excellent DX, security headers.
- Negative: Vendor lock-in for Pages-specific config (minimal — it's just a static file host). Migration to another CDN would take < 1 hour.

**Triggers for Revisiting:** Cloudflare Pages limits change, or V1 backend requires a platform that bundles hosting + compute (e.g., Railway, Fly.io).

---

### ADR-006: i18n-First Architecture with react-i18next

**Status:** Accepted
**Date:** 2026-03-13

**Context:** Pilot launches in Hebrew (RTL). V1 adds English (LTR). V2 adds Spanish (LTR). Should the codebase be i18n-ready from day one, or build Hebrew-only and refactor later?

**Decision:** Implement i18n from day one using `react-i18next`. All UI text and narration scripts are externalized into locale JSON files. Zero hardcoded strings.

**Rationale:**
- **Decision Log #10** explicitly calls for "i18n-ready architecture from the start to minimize rework."
- **PRD Open Question #10** recommends i18n from start "to reduce rework."
- **Cost of doing it later** is high: find-and-replace every hardcoded Hebrew string, add key-value mappings, test that nothing broke. Estimated 3–5 days of rework vs. ~2 hours of upfront setup.
- **RTL/LTR switching** is handled via CSS logical properties (ADR-001 consequence) + `<html dir>` attribute set by the i18n system. Building this in later would require refactoring all CSS.

**Alternatives Considered:**
- **Hardcode Hebrew, refactor later:** Faster for Sprint 1 by ~2 hours but creates technical debt that grows with every new screen and narration line.
- **FormatJS (react-intl):** Comparable to react-i18next but less flexible resource loading and slightly more verbose API. react-i18next's `useTranslation` hook is simpler.

**Consequences:**
- Positive: Adding English in V1 requires only: (1) create `en.json`, (2) add a locale switcher, (3) record/generate English narration audio. Zero code changes.
- Negative: Every string addition requires adding a key to the locale file instead of typing inline text. Minor workflow cost, but enforces discipline.

**Triggers for Revisiting:** None anticipated. i18n is a one-way door — once in place, it only gets more valuable.

---

### ADR-007: Howler.js + Web Speech API for Audio

**Status:** Accepted
**Date:** 2026-03-13

**Context:** The game needs two audio channels: (1) sound effects (chimes, clicks, fanfares) and (2) Hebrew narration (instructions, equation readouts, encouragement). Audio must work reliably on Chrome and Safari, handle mobile autoplay restrictions, and support independent volume/mute controls.

**Decision:** Use Howler.js for SFX and Web Speech API (`speechSynthesis`) for TTS narration, with a pre-generated audio file fallback for narration if TTS quality is poor.

**Rationale:**
- **Howler.js** (~10 KB) solves cross-browser audio pain points: audio context unlocking, format fallback (WebM → MP3), audio sprites (one file for all SFX), and programmatic volume control. It's the standard library for web game audio.
- **Web Speech API** is free, requires no API keys, and works offline. It avoids shipping ~2 MB of pre-recorded narration audio in the initial bundle.
- **Fallback plan:** If Hebrew TTS quality is inadequate (Risk #1), pre-generate narration audio files using Google Cloud TTS at build time. Store as lazily-loaded MP3s. The `AudioManager` interface stays the same — only the implementation behind `narrate()` changes.

**Alternatives Considered:**
- **All pre-recorded human narration:** Warmest voice quality but expensive to produce, impossible to iterate on quickly, and adds ~2–5 MB to the asset budget. Target for V1 if pilot validates the product.
- **All cloud TTS at runtime:** Requires API keys in the client (security risk), adds latency, fails offline. Rejected.
- **Tone.js:** Synthesizer/music library. Overkill for playing pre-recorded SFX files.

**Consequences:**
- Positive: Minimal bundle impact. Fast iteration on narration text (change the i18n string, TTS reads the new text). SFX are reliable cross-browser.
- Negative: TTS quality varies by device/browser. Hebrew voices on older Chromebooks may sound robotic. Mitigation: test early, switch to pre-generated files if needed.

**Triggers for Revisiting:** Hebrew TTS quality is unacceptable after testing on target devices. In that case, switch `narrate()` implementation to play pre-generated audio files — the rest of the code is unaffected.

---

## 13. Project Structure

```
treasure-of-tens/
├── public/
│   ├── _headers              # Cloudflare custom headers (CSP, HSTS)
│   └── favicon.ico
├── src/
│   ├── main.tsx              # Entry point: React root, providers
│   ├── App.tsx               # Router, screen orchestration
│   ├── components/
│   │   ├── game/             # Gameplay engine components
│   │   │   ├── GemCluster.tsx
│   │   │   ├── Splitter.tsx
│   │   │   ├── GemGroup.tsx
│   │   │   ├── CombineZone.tsx
│   │   │   ├── PowerGem.tsx
│   │   │   ├── EquationBar.tsx
│   │   │   ├── HintOverlay.tsx
│   │   │   └── NarratorBubble.tsx
│   │   ├── map/              # Progress map components
│   │   │   ├── CaveMap.tsx
│   │   │   └── LevelNode.tsx
│   │   └── ui/               # Shared UI components
│   │       ├── Button.tsx
│   │       ├── SpeakerToggle.tsx
│   │       └── SettingsOverlay.tsx
│   ├── screens/
│   │   ├── TitleScreen.tsx
│   │   ├── MapScreen.tsx
│   │   ├── GameplayScreen.tsx
│   │   └── LevelCompleteScreen.tsx
│   ├── state/
│   │   ├── GameStateContext.tsx   # React context + reducer
│   │   └── types.ts              # GameState, Action types
│   ├── audio/
│   │   ├── AudioManager.ts       # Howler + TTS orchestration
│   │   └── sfx/                  # Audio sprite files
│   ├── data/
│   │   ├── levels/               # Level definition JSON files
│   │   │   ├── world1.json
│   │   │   ├── world2.json
│   │   │   └── world3.json
│   │   └── types.ts              # LevelDefinition, WorldDefinition
│   ├── i18n/
│   │   ├── index.ts              # i18next config
│   │   └── locales/
│   │       └── he.json           # Hebrew strings
│   ├── analytics/
│   │   └── EventLogger.ts        # Anonymous event logging
│   ├── hooks/                    # Shared React hooks
│   │   ├── useGameState.ts
│   │   ├── useAudio.ts
│   │   └── useLevel.ts
│   └── styles/
│       ├── global.css            # CSS reset, custom properties, RTL base
│       └── tokens.css            # Design tokens (colors, spacing, typography)
├── tests/
│   ├── components/               # Component tests (Vitest + RTL)
│   └── e2e/                      # E2E tests (Playwright)
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
├── .eslintrc.cjs
├── .prettierrc
├── playwright.config.ts
└── wrangler.toml                 # Cloudflare Pages config
```

---

## 14. V1 Migration Path

When the Pilot validates the core game concept and V1 development begins, the following changes are needed:

| Change | Scope | Estimated Effort |
|---|---|---|
| **Add backend** (Node.js + Express or Fastify + PostgreSQL) | New code; client game engine unchanged | 2–3 weeks |
| **Teacher auth** (JWT + bcrypt) | Backend-only; add `/teacher` route to frontend | 1 week |
| **Student profiles** (pseudonym-based) | Backend + minor client changes (sync adapter) | 1 week |
| **Event sync** | Add `POST /api/events` call to EventLogger; keep localStorage as offline buffer | 2–3 days |
| **Teacher dashboard** | New React screens under `/teacher` route; consumes REST API | 2 weeks |
| **Worlds 4–5** | New level JSON + potentially new game components (gem pouches, subtraction mode) | 2–3 weeks each |
| **English localization** | Add `en.json` locale file + English narration audio + LTR testing | 1 week |

The architecture is designed so that V1 changes are **additive** — nothing in the Pilot codebase needs to be deleted or rewritten. The game engine gains an optional sync layer; the app gains new routes.

---

*Document version: 1.0*
*Created: 2026-03-13*
*Author: CTO-Architect Agent*
*Status: Draft — awaiting founder review*
