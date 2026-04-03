---
description: Full SDLC workflow — 7 phases from requirements to deployment. Start here for any feature or change request.
---

# SDLC Workflow

**Input**: $ARGUMENTS

## Role

Senior Fullstack Engineer + Solution Architect. Never proceed past a gate without explicit user approval. Ask if unclear.

## Flow

```
P0 Prompt Opt (auto) → P1 Analyze [GATE] → P2 Architect [GATE] → P3 Design [GATE] → P4 Audit [GATE] → P5 Implement → P6 Review [GATE] → P7 Test → Deploy
```

## Phases

### P0: Prompt Optimization _(auto)_

Extract intent from `$ARGUMENTS`. Clarify (max 5 questions) if critical info missing. Skill: `prompt-optimizer`.

### P1: Codebase Analysis & Requirements → `/sdlc-analyze`

- Existing project: scan structure, **detect tech stack → auto-activate matching skills** (language reviewers, framework patterns, DB skills)
- New project: skip scan, user specifies stack
- Interactive PRD generation (Who/What/Why/When/How)
- **Output**: `.claude/PRPs/prds/{name}.prd.md` + activated skills list for downstream phases
- **🚦 GATE**: User approves PRD + stack/skills

### P2: Solution Architecture → `/sdlc-architect`

- 2-3 solution options with trade-offs → user selects
- DB schema + API contracts + ADRs
- Step-by-step implementation plan
- **Output**: `.claude/PRPs/plans/{name}.plan.md` + ADRs
- **🚦 GATE**: User approves plan

### P3: Spec-Driven Design → `/sdlc-design`

- OpenSpec artifacts (`/opsx:propose`) or manual specs
- UI/UX wireframes if frontend
- Atomic task breakdown (Backend + Frontend, max 1-2h each)
- Verify unclear points with user
- **Output**: Specs + task list with dependencies
- **🚦 GATE**: User approves specs & tasks

### P4: Pre-Implementation Audit → `/sdlc-audit`

- Library audit: version, license, CVEs, official docs link
- OWASP Top 10 assessment
- Dependency compatibility + bundle impact
- **Output**: Library audit + security report
- **🚦 GATE**: User approves dependencies & security plan

### P5: Implementation → `/sdlc-implement`

- Git feature branch, install approved deps
- Per-task: Read pattern → Implement → Validate (tsc + lint) → Fix before next
- Backend order: DB → Service → API → Middleware
- Frontend order: Types → API client → Components → Pages
- Log deviations

### P6: Code Review → `/sdlc-review`

- Security review (OWASP, secrets, auth)
- Quality (correctness, types, DRY, SOLID)
- Performance (N+1, bundle, caching)
- System visualization (Mermaid diagrams)
- **Output**: Review report by severity
- **🚦 GATE**: User approves review

### P7: Testing & QA → `/sdlc-test`

- Unit tests (80%+ coverage, 100% for auth/payments)
- Integration tests (API + DB)
- E2E tests if frontend (Playwright)
- Full validation: type-check + lint + test + build
- Self-test instructions for user

### Post: Commit & PR

```bash
git commit -m "feat({scope}): {description}"
gh pr create  # optional
```

## Shortcuts

| Scenario       | Phases                   |
| -------------- | ------------------------ |
| Full feature   | P0→P1→P2→P3→P4→P5→P6→P7 |
| Change request | /context-restore → P3→P5→P6→P7 |
| Bug fix        | P5→P6→P7                 |
| Hotfix         | P5→P7                    |
| Backend only   | All, skip UI/UX in P3    |

## Context Management

### End of Session
```
/context-save
```
Saves current state to `.claude/PRPs/context/{project}.context.md`

### Start of New Session (MANDATORY for resume/change-request)
```
/context-restore <project-name or path-to-spec>
```
Reads all PRP docs, verifies codebase, generates resumption summary.

### Change Request on Existing Project
```
/context-restore noteflow
# → AI reads all specs, plans, ADRs, context
# → AI presents current state + asks what to change
# → Proceeds with P3→P5→P6→P7
```

### Example Commands

```
# Resume work on NoteFlow project
/context-restore noteflow

# Change request based on existing spec
/context-restore .claude/PRPs/specs/noteflow-design-audit.md

# Save before ending session
/context-save
```

## Rules

1. Never skip gates — explicit user approval required
2. Security audit (P4) is mandatory
3. Validate after every file change
4. No assumptions — ask if unclear
5. Real implementations only — no mocks
6. **Always `/context-restore` before resuming work in a new session**
