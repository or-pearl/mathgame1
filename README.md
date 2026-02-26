# Agent Orchestration Guide

## How This Works

This `/prompts` directory contains role-specific agent prompts that turn Claude Code into specialized team members on demand. Instead of managing 8 separate chat sessions, you switch agents by referencing a prompt file.

## Quick Start

In Claude Code, invoke an agent like this:

```
Read /prompts/pm-problem-discovery.md for your role.
Check /sources/ for reference materials, then read /docs/pdb.md if it exists.
I need to define the problem for [your product idea].
```

Or for implementation:

```
Read /prompts/cto-implementation.md for your role.
Read /docs/prd.md and /docs/architecture.md for context.
Implement the next item in /docs/backlog.md.
```

## Repo Structure

```
/your-product/
├── /prompts                    # Agent role definitions (this directory)
│   ├── pm-problem-discovery.md
│   ├── pm-solution-discovery.md
│   ├── pm-requirements.md
│   ├── cto-architect.md
│   ├── cto-implementation.md
│   ├── ds-strategy.md
│   ├── ds-engineering.md
│   ├── qa-review.md
│   └── qa-acceptance.md
├── /sources                    # Founder-provided reference materials
│   ├── (articles, papers, guidelines, regulatory docs, competitor analysis...)
│   └── README.md               # Optional: index of what each source is and why it's here
├── /docs                       # Shared artifacts (the "Project Knowledge")
│   ├── pdb.md                  # Problem Definition Brief
│   ├── sdb.md                  # Solution Discovery Brief
│   ├── prd.md                  # Product Requirements Document
│   ├── architecture.md         # System architecture + ADRs
│   ├── backlog.md              # Prioritized backlog
│   ├── decision-log.md         # Decision history
│   ├── deployment.md           # Deployment guide
│   ├── model-spec.md           # ML model specification (if applicable)
│   ├── model-eval-report.md    # ML evaluation results (if applicable)
│   ├── qa-review-notes.md      # QA findings
│   ├── release-readiness.md    # Go/no-go assessment
│   └── kpis.md                 # Success metrics (if complex)
├── /src                        # Application code
├── /tests                      # Test suites
│   └── /acceptance             # Acceptance test scripts
├── /ml                         # ML code (if applicable)
│   ├── /prompts                # LLM system prompts (if using foundation models)
│   ├── /pipelines              # Data pipelines
│   ├── /models                 # Training and evaluation
│   ├── /serving                # Model serving
│   └── /experiments            # Experiment log
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

- Found a relevant paper while building? Drop it in `/sources/` and re-invoke PM-Requirements: "Read /prompts/pm-requirements.md. I've added a new source in /sources/[filename]. Assess its impact on the current PRD."
- Got feedback from a stakeholder? Save it as a markdown file in `/sources/` and re-invoke PM-Problem-Discovery to update the PDB.
- Received API docs from a vendor? Drop them in `/sources/` and invoke CTO-Architect to update the integration architecture.

The agents are designed to handle incremental source updates without regenerating everything from scratch.

## Workflow: Idea → Shipped Product

### Phase 1: Discovery (Day 1)
```
Agent: pm-problem-discovery
Input: Your idea + source materials (if any)
Output: /docs/pdb.md
```

**Without sources:**
"Read /prompts/pm-problem-discovery.md. Here's what I want to build: [describe the problem or idea]."

**With sources already in `/sources/`:**
"Read /prompts/pm-problem-discovery.md. Check /sources/ for reference materials I've provided. Here's the problem I want to explore: [describe the problem]. The sources include [brief note on what you've provided and why]."

**With inline sources:**
"Read /prompts/pm-problem-discovery.md. Here's a research paper that describes the problem space: [paste or attach]. I want to build a product that addresses [specific aspect]. Use this as primary context."

**Strict source mode:**
"Read /prompts/pm-problem-discovery.md. Read /sources/clinical-guideline.md. This is the definitive protocol — define the problem strictly based on what this guideline describes. Don't introduce outside assumptions."

### Phase 2: Solution Discovery (Day 1-2)
```
Agent: pm-solution-discovery
Input: /docs/pdb.md, /sources/* (if any)
Output: /docs/sdb.md (Solution Discovery Brief)
```

**Standard:**
"Read /prompts/pm-solution-discovery.md. Check /sources/ for reference materials, then read /docs/pdb.md. Explore solution approaches for this problem."

**With a founder leaning:**
"Read /prompts/pm-solution-discovery.md. Read /docs/pdb.md. I'm leaning toward a browser-based educational game — evaluate that alongside alternatives."

**Updating after new info:**
"Read /prompts/pm-solution-discovery.md. Read /docs/sdb.md. I've added /sources/competitor-teardown.md — reassess the recommendation in light of this."

### Phase 3: Feasibility (Day 2-3)
```
Agent: cto-architect (for technical feasibility)
Agent: ds-strategy (if ML is involved)
Input: /docs/pdb.md, /docs/sdb.md
Output: Preliminary /docs/architecture.md, /docs/model-spec.md (if applicable)
```
**You say:** "Read /prompts/cto-architect.md. Read /docs/pdb.md and /docs/sdb.md. Give me a preliminary architecture assessment — is this buildable? What's the simplest stack?"

If ML is involved:
**You say:** "Read /prompts/ds-strategy.md. Read /docs/pdb.md and /docs/sdb.md. Is this ML-tractable? What's the simplest approach?"

### Phase 4: Requirements (Day 3-4)
```
Agent: pm-requirements
Input: /docs/pdb.md, /docs/sdb.md, /docs/architecture.md, /sources/* (if any)
Output: /docs/prd.md, /docs/backlog.md, /docs/decision-log.md
```

**Standard:**
"Read /prompts/pm-requirements.md. Check /sources/ for reference materials, then read /docs/pdb.md, /docs/sdb.md, and /docs/architecture.md. Draft the PRD with a Pilot-first scope."

**With regulatory source:**
"Read /prompts/pm-requirements.md. Check /sources/ — I've added the HIPAA sections relevant to our product. Read /docs/pdb.md, /docs/sdb.md, and /docs/architecture.md. Draft the PRD and make sure every HIPAA-derived requirement is explicitly cited and testable."

**Updating PRD with new source:**
"Read /prompts/pm-requirements.md. I've added /sources/vendor-api-docs.md. Assess how this affects the current PRD — specifically integration requirements in Section 7."

### Phase 5: Architecture (Day 4-5)
```
Agent: cto-architect
Input: /docs/prd.md, /docs/sdb.md
Output: Full /docs/architecture.md with ADRs, tech stack, data model
```
**You say:** "Read /prompts/cto-architect.md. Read /docs/prd.md and /docs/sdb.md. Produce the full system architecture."

If ML is involved:
```
Agent: ds-strategy
Input: /docs/prd.md, /docs/architecture.md
Output: /docs/model-spec.md
```

### Phase 6: Build (Days 6-20+)
```
Agent: cto-implementation
Input: /docs/prd.md, /docs/architecture.md, /docs/backlog.md
Output: /src, /tests, API docs
```
**You say:** "Read /prompts/cto-implementation.md. Read /docs/prd.md, /docs/architecture.md, and /docs/backlog.md. Implement the top backlog item."

Repeat for each backlog item. This is where you spend most of your time.

If ML is involved:
```
Agent: ds-engineering
Input: /docs/model-spec.md, /docs/architecture.md
Output: /ml/*, model serving endpoints
```

### Phase 7: Review (Ongoing + Pre-Release)
```
Agent: qa-review
Input: /src, /docs/prd.md, /docs/architecture.md
Output: /docs/qa-review-notes.md
```
**You say:** "Read /prompts/qa-review.md. Review the code in /src against the PRD acceptance criteria in /docs/prd.md. Check for security and compliance."

### Phase 8: Acceptance + Release (Pre-Launch)
```
Agent: qa-acceptance
Input: /docs/prd.md, /docs/architecture.md, /docs/qa-review-notes.md
Output: /docs/release-readiness.md
```
**You say:** "Read /prompts/qa-acceptance.md. Run acceptance tests against the deployed staging environment. Produce the release readiness report."

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
