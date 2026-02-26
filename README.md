# Agent Orchestration Guide

## How This Works

This repo uses role-specific agent prompts in `/prompts/` that turn Claude Code into specialized team members on demand. Instead of managing 8 separate chat sessions, you switch agents with a one-liner.

Agent invocation is handled automatically by `CLAUDE.md`, which Claude Code reads on session start. You never need to manually specify which prompt file or docs to load.

## Quick Start

Open Claude Code in this repo and say:

```
be CTO-Architect
```

That's it. `CLAUDE.md` tells Claude to read `/prompts/cto-architect.md` for the role definition, then load `/docs/prd.md`, `/docs/pdb.md`, `/docs/sdb.md`, and `/docs/decision-log.md` for context. No manual file references needed.

### All agent triggers

| You say | Agent activated |
|---|---|
| "be PM-Problem-Discovery" / "discover the problem" | PM-Problem-Discovery |
| "be PM-Solution-Discovery" / "explore solutions" | PM-Solution-Discovery |
| "be PM-Requirements" / "write requirements" | PM-Requirements |
| "be CTO-Architect" / "design the architecture" | CTO-Architect |
| "be CTO-Implementation" / "build" / "implement" | CTO-Implementation |
| "be DS-Strategy" / "evaluate ML" | DS-Strategy |
| "be DS-Engineering" / "build ML" | DS-Engineering |
| "be QA-Review" / "review code" | QA-Review |
| "be QA-Acceptance" / "acceptance test" | QA-Acceptance |

### Shorthand commands

- **"next backlog item"** — Finds the first non-Done item in `/docs/backlog.md` and implements it as CTO-Implementation.
- **"update the PRD"** — Opens `/docs/prd.md` as PM-Requirements for surgical edits.
- **"review"** (no qualifier) — Runs QA-Review against `/docs/prd.md` acceptance criteria.

## Repo Structure

```
/your-product/
├── CLAUDE.md                      # Auto-bootstrap: agent dispatch + project context
├── /prompts                       # Agent role definitions (read-only reference)
│   ├── pm-problem-discovery.md
│   ├── pm-solution-discovery.md
│   ├── pm-requirements.md
│   ├── cto-architect.md
│   ├── cto-implementation.md
│   ├── ds-strategy.md
│   ├── ds-engineering.md
│   ├── qa-review.md
│   └── qa-acceptance.md
├── /sources                       # Founder-provided reference materials
│   ├── (articles, papers, guidelines, regulatory docs, competitor analysis...)
│   └── README.md                  # Optional: index of what each source is and why it's here
├── /docs                          # Shared artifacts (the "Project Knowledge")
│   ├── pdb.md                     # Problem Definition Brief
│   ├── sdb.md                     # Solution Discovery Brief
│   ├── prd.md                     # Product Requirements Document
│   ├── architecture.md            # System architecture + ADRs
│   ├── backlog.md                 # Prioritized backlog
│   ├── decision-log.md            # Decision history
│   ├── deployment.md              # Deployment guide
│   ├── model-spec.md              # ML model specification (if applicable)
│   ├── model-eval-report.md       # ML evaluation results (if applicable)
│   ├── qa-review-notes.md         # QA findings
│   ├── release-readiness.md       # Go/no-go assessment
│   └── kpis.md                    # Success metrics (if complex)
├── /src                           # Application code
├── /tests                         # Test suites
│   └── /acceptance                # Acceptance test scripts
├── /ml                            # ML code (if applicable)
│   ├── /prompts                   # LLM system prompts (if using foundation models)
│   ├── /pipelines                 # Data pipelines
│   ├── /models                    # Training and evaluation
│   ├── /serving                   # Model serving
│   └── /experiments               # Experiment log
└── README.md
```

## The `/sources` Directory

This is where you drop reference materials that agents should read before doing their work. PM-Problem-Discovery and PM-Requirements check this directory first, before any other input.

### What goes in `/sources`

- Research papers (PDFs, markdown summaries)
- Clinical guidelines and protocols
- Regulatory documents (HIPAA sections, MDR requirements, GDPR articles)
- Competitor product teardowns or reviews
- Market reports and landscape analyses
- API documentation for systems you need to integrate with
- UX research outputs, user interview transcripts
- Internal strategy docs or organizational constraints
- Standards documentation (HL7, FHIR, WCAG, ISO)
- News articles or industry analyses relevant to the problem

### How to organize `/sources`

Keep it simple. Flat directory is fine for small projects. For larger ones:

```
/sources/
├── README.md                   # Index: what each source is and why it matters
├── hipaa-164-530.pdf           # Regulatory: audit log retention requirements
├── morse-fall-scale.md         # Clinical: fall risk assessment protocol
├── competitor-analysis.md      # Market: teardown of top 3 competitors
├── user-interviews-q4.md       # Research: 12 user interviews from Q4
└── fhir-patient-resource.md    # Technical: FHIR spec for patient data model
```

A `/sources/README.md` index is optional but helpful when you accumulate more than 5-6 files:

```markdown
# Source Materials Index

| File | Type | Why It's Here |
|------|------|---------------|
| hipaa-164-530.pdf | Regulatory | Defines audit log retention requirements |
| morse-fall-scale.md | Clinical | Fall risk assessment protocol we're digitizing |
| competitor-analysis.md | Market | Competitor feature gaps our product addresses |
```

### How sources flow through the system

```
You drop files in /sources/
        │
        ▼
PM-Problem-Discovery reads them ──► Extracts problem-relevant insights
        │                    Cites them in PDB (Evidence Base section)
        │                    Flags what the sources DON'T cover (Source Gaps)
        │
        ▼
PM-Requirements reads them ──► Translates source mandates into testable requirements
        │                       Traces each requirement to its source (or marks "inferred")
        │                       Flags conflicts between sources and architecture
        │
        ▼
Downstream agents (CTO, DS, QA) read /docs/ artifacts
        which already contain source citations and traceability
        They consult /sources/ directly only when they need
        the original detail (e.g., checking an API spec)
```

### Two modes of source usage

**Default mode:** Sources are one input among several. Agents synthesize, challenge, and cross-reference them with their own reasoning. A research paper doesn't automatically become a requirement — PM-Requirements decides whether and how to incorporate it.

**Strict mode:** When you say "this is the definitive standard" or "follow this exactly," the agent constrains itself to the provided material. Use this for regulatory documents or organizational mandates that aren't open to interpretation.

### Adding sources mid-project

Sources aren't just a day-one input. You can add new materials at any phase:

- Found a relevant paper while building? Drop it in `/sources/` and say: "be PM-Requirements. I've added a new source in /sources/[filename]. Assess its impact on the current PRD."
- Got feedback from a stakeholder? Save it as a markdown file in `/sources/` and say: "be PM-Problem-Discovery — update the PDB with the new stakeholder feedback."
- Received API docs from a vendor? Drop them in `/sources/` and say: "be CTO-Architect — update the integration architecture."

The agents are designed to handle incremental source updates without regenerating everything from scratch.

## Workflow: Idea → Shipped Product

### Phase 1: Discovery (Day 1)
```
Agent: PM-Problem-Discovery
Input: Your idea + source materials (if any)
Output: /docs/pdb.md
```

**Without sources:**
"discover the problem — here's what I want to build: [describe the problem or idea]."

**With sources already in `/sources/`:**
"discover the problem — the sources include [brief note on what you've provided and why]. Here's the problem I want to explore: [describe the problem]."

**Strict source mode:**
"be PM-Problem-Discovery. /sources/clinical-guideline.md is the definitive protocol — define the problem strictly based on what this guideline describes. Don't introduce outside assumptions."

### Phase 2: Solution Discovery (Day 1-2)
```
Agent: PM-Solution-Discovery
Input: /docs/pdb.md, /sources/* (if any)
Output: /docs/sdb.md (Solution Discovery Brief)
```

**Standard:**
"explore solutions"

**With a founder leaning:**
"explore solutions — I'm leaning toward a browser-based educational game, evaluate that alongside alternatives."

**Updating after new info:**
"be PM-Solution-Discovery. I've added /sources/competitor-teardown.md — reassess the recommendation in light of this."

### Phase 3: Feasibility (Day 2-3)
```
Agent: CTO-Architect (for technical feasibility)
Agent: DS-Strategy (if ML is involved)
Input: /docs/pdb.md, /docs/sdb.md
Output: Preliminary /docs/architecture.md, /docs/model-spec.md (if applicable)
```
**You say:** "be CTO-Architect — give me a preliminary architecture assessment. Is this buildable? What's the simplest stack?"

If ML is involved:
**You say:** "evaluate ML — is this ML-tractable? What's the simplest approach?"

### Phase 4: Requirements (Day 3-4)
```
Agent: PM-Requirements
Input: /docs/pdb.md, /docs/sdb.md, /docs/architecture.md, /sources/* (if any)
Output: /docs/prd.md, /docs/backlog.md, /docs/decision-log.md
```

**Standard:**
"write requirements — draft the PRD with a Pilot-first scope."

**With regulatory source:**
"write requirements — I've added the HIPAA sections relevant to our product in /sources/. Make sure every HIPAA-derived requirement is explicitly cited and testable."

**Updating PRD with new source:**
"update the PRD — I've added /sources/vendor-api-docs.md. Assess how this affects integration requirements in Section 7."

### Phase 5: Architecture (Day 4-5)
```
Agent: CTO-Architect
Input: /docs/prd.md, /docs/sdb.md
Output: Full /docs/architecture.md with ADRs, tech stack, data model
```
**You say:** "design the architecture"

If ML is involved:
```
Agent: DS-Strategy
Input: /docs/prd.md, /docs/architecture.md
Output: /docs/model-spec.md
```

### Phase 6: Build (Days 6-20+)
```
Agent: CTO-Implementation
Input: /docs/prd.md, /docs/architecture.md, /docs/backlog.md
Output: /src, /tests, API docs
```
**You say:** "next backlog item"

Repeat for each backlog item. This is where you spend most of your time.

If ML is involved:
```
Agent: DS-Engineering
Input: /docs/model-spec.md, /docs/architecture.md
Output: /ml/*, model serving endpoints
```

### Phase 7: Review (Ongoing + Pre-Release)
```
Agent: QA-Review
Input: /src, /docs/prd.md, /docs/architecture.md
Output: /docs/qa-review-notes.md
```
**You say:** "review"

### Phase 8: Acceptance + Release (Pre-Launch)
```
Agent: QA-Acceptance
Input: /docs/prd.md, /docs/architecture.md, /docs/qa-review-notes.md
Output: /docs/release-readiness.md
```
**You say:** "acceptance test"

## Tips for Solo Operation

1. **Don't skip discovery.** 45 minutes with PM-Problem-Discovery saves weeks of building the wrong thing.

2. **Drop sources before you start, not after.** If you have research, guidelines, or competitor analysis, put it in `/sources/` before invoking PM-Problem-Discovery. It's much cheaper to inform the problem definition upfront than to retrofit sources into a half-built PRD.

3. **Run QA-Review after every 2-3 features**, not just before release. Catching a security issue early is 10x cheaper than catching it in production.

4. **The decision log is your memory.** When you come back to the project after a week off, `/docs/decision-log.md` tells you what was decided and why.

5. **Phase aggressively.** Your pilot should be embarrassingly small. PM-Requirements will help you cut scope.

6. **Git commit after every agent session.** Commit messages like "PM-Requirements: updated PRD scope for pilot" or "QA-Review: security review round 1" make the project history readable. Commit source files too — they're part of your project's evidence base.

7. **When agents disagree**, that's a feature, not a bug. If CTO-Architect says a requirement is infeasible and PM-Requirements insists it's critical, you (the founder) make the call and document it in the decision log.

8. **Source quality matters.** A peer-reviewed paper and a random blog post should not carry equal weight. PM-Problem-Discovery and PM-Requirements are instructed to challenge sources — but you should also curate what you put in `/sources/`. Garbage in, garbage out.

9. **Don't over-source.** Five well-chosen references are better than 30 loosely relevant articles. Each source the agents read takes context window space and processing time. Be selective.
