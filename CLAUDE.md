# Treasure of Tens — Agent-Orchestrated Development

Hebrew-first educational math game for first-graders. Teaches decomposition (make-10 strategy) through a gem-splitting mechanic using the Concrete-Representational-Abstract (CRA) framework.

## Agent System

This repo uses role-specific prompts in `/prompts/` to turn Claude into specialized team members. When the founder names an agent, adopt that role by reading the prompt file and its required docs automatically.

### Invoking an agent

When the founder says any of these patterns, load the corresponding prompt and its dependencies:

| Founder says | Read prompt | Then read (if they exist) |
|---|---|---|
| "be PM-Problem-Discovery" / "discover the problem" | `/prompts/pm-problem-discovery.md` | `/sources/*`, `/docs/pdb.md` |
| "be PM-Solution-Discovery" / "explore solutions" | `/prompts/pm-solution-discovery.md` | `/sources/*`, `/docs/pdb.md`, `/docs/sdb.md` |
| "be PM-Requirements" / "write requirements" | `/prompts/pm-requirements.md` | `/sources/*`, `/docs/pdb.md`, `/docs/sdb.md`, `/docs/architecture.md`, `/docs/decision-log.md` |
| "be CTO-Architect" / "design the architecture" | `/prompts/cto-architect.md` | `/docs/prd.md`, `/docs/pdb.md`, `/docs/sdb.md`, `/docs/decision-log.md` |
| "be CTO-Implementation" / "build" / "implement" | `/prompts/cto-implementation.md` | `/docs/prd.md`, `/docs/architecture.md`, `/docs/backlog.md`, `/docs/decision-log.md` |
| "be DS-Strategy" / "evaluate ML" | `/prompts/ds-strategy.md` | `/docs/prd.md`, `/docs/pdb.md`, `/docs/sdb.md` |
| "be DS-Engineering" / "build ML" | `/prompts/ds-engineering.md` | `/docs/model-spec.md`, `/docs/architecture.md` |
| "be QA-Review" / "review code" | `/prompts/qa-review.md` | `/docs/prd.md`, `/docs/architecture.md`, `/src/**` |
| "be QA-Acceptance" / "acceptance test" | `/prompts/qa-acceptance.md` | `/docs/prd.md`, `/docs/architecture.md`, `/docs/qa-review-notes.md`, `/docs/backlog.md` |

**Always** read the prompt file first — it defines the agent's full role, behaviors, output format, and boundaries. Then read the listed docs. Skip any file that doesn't exist yet.

### Shorthand rules

- "next backlog item" → read `/docs/backlog.md`, find the first non-Done item, and implement it as CTO-Implementation.
- "update the PRD" → re-read `/docs/prd.md` as PM-Requirements and make surgical edits, don't regenerate.
- "review" with no qualifier → QA-Review against `/docs/prd.md` acceptance criteria.

## Project Status

**Completed artifacts:**
- `/docs/pdb.md` — Problem Definition Brief
- `/docs/prd.md` — Product Requirements Document (40KB, 10 sections)
- `/docs/backlog.md` — Prioritized backlog (25+ items across Pilot/V1/V2)
- `/docs/decision-log.md` — 11 major decisions with rationale

**Not yet created:**
- `/docs/sdb.md` — Solution Discovery Brief
- `/docs/architecture.md` — System architecture + ADRs
- `/src/`, `/tests/` — No code yet

**Current phase:** Pre-implementation. Architecture is the next major milestone.

## Repo Layout

```
/prompts/   → Agent role definitions (read-only reference, don't edit unless asked)
/sources/   → 7 pedagogical research papers (read-only reference for agents)
/docs/      → Shared artifacts that agents read and write
/src/       → Application code (not yet created)
/tests/     → Test suites (not yet created)
```

## Conventions

- **Commits:** Reference the agent and artifact, e.g. "PM-Requirements: update PRD scope for pilot"
- **Decision log:** When any agent makes a significant choice or encounters a conflict, append to `/docs/decision-log.md`
- **Backlog status:** Mark items as `Done` / `In Progress` / `Not Started` in `/docs/backlog.md` as work progresses
- **Hebrew-first:** Pilot is Hebrew RTL. All UI text and narration in Hebrew. Math notation stays LTR. i18n-ready architecture from day one.
- **No ML in Pilot:** DS agents are not needed until V1 at earliest.
