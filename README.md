# Agent Orchestration Guide

## How This Works

This `/prompts` directory contains role-specific agent prompts that turn Claude Code into specialized team members on demand. Instead of managing 8 separate chat sessions, you switch agents by referencing a prompt file.

## Quick Start

In Claude Code, invoke an agent like this:

```
Read /prompts/pm-discovery.md for your role. Then read /docs/pdb.md if it exists.
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
│   ├── pm-discovery.md
│   ├── pm-requirements.md
│   ├── cto-architect.md
│   ├── cto-implementation.md
│   ├── ds-strategy.md
│   ├── ds-engineering.md
│   ├── qa-review.md
│   └── qa-acceptance.md
├── /docs                       # Shared artifacts (the "Project Knowledge")
│   ├── pdb.md                  # Problem Definition Brief
│   ├── prd.md                  # Product Requirements Document
│   ├── architecture.md         # System architecture + ADRs
│   ├── backlog.md              # Prioritized backlog
│   ├── decision-log.md         # Decision history
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

## Workflow: Idea → Shipped Product

### Phase 1: Discovery (Day 1)
```
Agent: pm-discovery
Input: Your idea / problem statement
Output: /docs/pdb.md
```
**You say:** "Read /prompts/pm-discovery.md. Here's what I want to build: [describe the problem or idea]."

### Phase 2: Feasibility (Day 1-2)
```
Agent: cto-architect (for technical feasibility)
Agent: ds-strategy (if ML is involved)
Input: /docs/pdb.md
Output: Preliminary /docs/architecture.md, /docs/model-spec.md (if applicable)
```
**You say:** "Read /prompts/cto-architect.md. Read /docs/pdb.md. Give me a preliminary architecture assessment — is this buildable? What's the simplest stack?"

If ML is involved:
**You say:** "Read /prompts/ds-strategy.md. Read /docs/pdb.md. Is this ML-tractable? What's the simplest approach?"

### Phase 3: Requirements (Day 2-3)
```
Agent: pm-requirements
Input: /docs/pdb.md, /docs/architecture.md
Output: /docs/prd.md, /docs/backlog.md, /docs/decision-log.md
```
**You say:** "Read /prompts/pm-requirements.md. Read /docs/pdb.md and /docs/architecture.md. Draft the PRD with a Pilot-first scope."

### Phase 4: Architecture (Day 3-4)
```
Agent: cto-architect
Input: /docs/prd.md
Output: Full /docs/architecture.md with ADRs, tech stack, data model
```
**You say:** "Read /prompts/cto-architect.md. Read /docs/prd.md. Produce the full system architecture."

If ML is involved:
```
Agent: ds-strategy
Input: /docs/prd.md, /docs/architecture.md
Output: /docs/model-spec.md
```

### Phase 5: Build (Days 5-20+)
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

### Phase 6: Review (Ongoing + Pre-Release)
```
Agent: qa-review
Input: /src, /docs/prd.md, /docs/architecture.md
Output: /docs/qa-review-notes.md
```
**You say:** "Read /prompts/qa-review.md. Review the code in /src against the PRD acceptance criteria in /docs/prd.md. Check for security and compliance."

### Phase 7: Acceptance + Release (Pre-Launch)
```
Agent: qa-acceptance
Input: /docs/prd.md, /docs/architecture.md, /docs/qa-review-notes.md
Output: /docs/release-readiness.md
```
**You say:** "Read /prompts/qa-acceptance.md. Run acceptance tests against the deployed staging environment. Produce the release readiness report."

## Tips for Solo Operation

1. **Don't skip discovery.** 45 minutes with PM-Discovery saves weeks of building the wrong thing.

2. **Run QA-Review after every 2-3 features**, not just before release. Catching a security issue early is 10x cheaper than catching it in production.

3. **The decision log is your memory.** When you come back to the project after a week off, `/docs/decision-log.md` tells you what was decided and why.

4. **Phase aggressively.** Your pilot should be embarrassingly small. PM-Requirements will help you cut scope.

5. **Git commit after every agent session.** Commit messages like "PM-Requirements: updated PRD scope for pilot" or "QA-Review: security review round 1" make the project history readable.

6. **When agents disagree**, that's a feature, not a bug. If CTO-Architect says a requirement is infeasible and PM-Requirements insists it's critical, you (the founder) make the call and document it in the decision log.
