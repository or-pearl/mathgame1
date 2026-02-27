# Project Status Dashboard

> **Last updated:** 2026-02-27 | **Current phase:** Pre-implementation | **Next milestone:** Architecture

## Artifacts

| Artifact | Path | Status | Last Agent | Notes |
|----------|------|--------|------------|-------|
| Problem Definition Brief | `/docs/pdb.md` | Done | PM-Problem-Discovery | Covers personas, JTBD, constraints, evidence base |
| Solution Discovery Brief | `/docs/sdb.md` | Not started | — | Blocked on: founder invoking PM-Solution-Discovery |
| Product Requirements Doc | `/docs/prd.md` | Done | PM-Requirements | 40KB, 10 sections, Hebrew-first pilot scope |
| Backlog | `/docs/backlog.md` | Done | PM-Requirements | 35 items across Pilot / V1 / V2 (IDs #1–#35) |
| Decision Log | `/docs/decision-log.md` | Active | Founder | 12 decisions logged |
| Architecture | `/docs/architecture.md` | Not started | — | Next major milestone |
| Design Spec | `/docs/design-spec.md` | Not started | — | UX-Design agent output; blocked on Architecture |
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
| 12 | 2026-02-27 | Founder | Add UX-Design agent; Figma MCP integration (Path B) |
| 11 | 2026-02-25 | PM-Requirements | RTL layout as default for Hebrew Pilot |
| 10 | 2026-02-25 | Founder | Hebrew-first for Israeli market; EN deferred to V1 |
| 9 | 2026-02-19 | PM-Requirements | Game name: "Treasure of Tens" |

## Pending Flags

_Inter-agent flags that need founder attention. When an agent flags something for another agent, it adds a row here. Resolve by invoking the target agent, then mark the flag `Resolved`._

| # | Date | From | For | Summary | Ref | Status |
|---|------|------|-----|---------|-----|--------|
| — | — | — | — | No pending flags | — | — |

## What's Blocking Progress

1. **Architecture not yet written.** All P0 backlog items depend on tech stack and system design decisions. Invoke CTO-Architect next.
2. **Design spec not yet written.** Visual implementation items (gem sprites, screen layouts, animations) depend on the design spec. Invoke UX-Design after Architecture.
3. **SDB skipped.** PM-Solution-Discovery was never run. The PRD was written without a formal Solution Discovery Brief. Consider running it retroactively or accepting the gap.
