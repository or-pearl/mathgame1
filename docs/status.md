# Project Status Dashboard

> **Last updated:** 2026-03-13 | **Current phase:** Architecture complete | **Next milestone:** Design Spec (UX-Design)

## Artifacts

| Artifact | Path | Status | Last Agent | Notes |
|----------|------|--------|------------|-------|
| Problem Definition Brief | `/docs/pdb.md` | Done | PM-Problem-Discovery | Covers personas, JTBD, constraints, evidence base |
| Solution Discovery Brief | `/docs/sdb.md` | Skipped | — | Gap accepted by founder (2026-03-14). PRD written without SDB. Will invoke PM-Solution-Discovery only if solution pivot is needed. |
| Product Requirements Doc | `/docs/prd.md` | Done | PM-Requirements | 40KB, 10 sections, Hebrew-first pilot scope |
| Backlog | `/docs/backlog.md` | Done | PM-Requirements | 35 items across Pilot / V1 / V2 (IDs #1–#35) |
| Decision Log | `/docs/decision-log.md` | Active | CTO-Architect | 20 decisions logged (7 new architecture decisions) |
| Architecture | `/docs/architecture.md` | Done | CTO-Architect | React 18 + TS + Vite, static SPA, Cloudflare Pages, 7 ADRs |
| Design Spec | `/docs/design-spec.md` | Not started | — | UX-Design agent output; **unblocked** — Architecture complete |
| Asset Manifest | `/docs/asset-manifest.md` | Not started | — | UX-Design agent output; blocked on Design Spec |
| Source Index | `/sources/README.md` | Done | — | 7 pedagogical references indexed |

## Backlog Summary

| Phase | Total | Done | In Progress | Not Started |
|-------|-------|------|-------------|-------------|
| Pilot | 21 | 0 | 0 | 21 |
| V1 | 10 | 0 | 0 | 10 |
| V2+ | 7 | 0 | 0 | 7 |

### Next up (top Pilot items)

| # | Priority | Item | Complexity | Blocked by |
|---|----------|------|------------|------------|
| 36 | P0 | Design system definition | M | Architecture |
| 37 | P0 | Screen designs for all Pilot screens | L | #36 |
| 38 | P0 | Asset manifest and gem sprite generation specs | M | #36 |
| 1 | P0 | Gem-splitting core mechanic | L | Architecture, #37 |
| 2 | P0 | Audio narration system (Hebrew TTS) | M | Architecture |

## Recent Decisions

| # | Date | Agent | Decision |
|---|------|-------|----------|
| 20 | 2026-03-13 | CTO-Architect | Howler.js + Web Speech API for audio; pre-generated fallback if TTS quality poor |
| 19 | 2026-03-13 | CTO-Architect | i18n-first with react-i18next — zero hardcoded strings |
| 18 | 2026-03-13 | CTO-Architect | Cloudflare Pages hosting (free tier, Tel Aviv edge node) |
| 17 | 2026-03-13 | CTO-Architect | @use-gesture/react for drag, CSS/WAAPI for animation |
| 16 | 2026-03-13 | CTO-Architect | React 18 + TypeScript 5 + Vite 6 core stack |
| 15 | 2026-03-13 | CTO-Architect | Static SPA, zero backend for Pilot |
| 14 | 2026-03-13 | CTO-Architect | DOM-based rendering over Canvas for WCAG 2.1 AA |
| 13 | 2026-03-02 | Founder | Remove broken Figma MCP config; UX-Design agent text-spec-only |

## Pending Flags

_Inter-agent flags that need founder attention. When an agent flags something for another agent, it adds a row here. Resolve by invoking the target agent, then mark the flag `Resolved`._

| # | Date | From | For | Summary | Ref | Status |
|---|------|------|-----|---------|-----|--------|
| — | — | — | — | No pending flags | — | — |

## What's Blocking Progress

1. ~~**Architecture not yet written.**~~ **Done** (2026-03-13). All P0 backlog items are now unblocked from the architecture dependency.
2. **Design spec not yet written.** Visual implementation items (gem sprites, screen layouts, animations) depend on the design spec. **Invoke UX-Design next** — Architecture dependency is resolved.
3. ~~**SDB skipped.**~~ **Gap accepted** (2026-03-14). Founder confirmed PM-Solution-Discovery is not needed retroactively — the PRD was written with sufficient solution clarity. Will invoke the agent only if a solution pivot is needed.
