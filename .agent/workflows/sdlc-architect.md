---
description: Phase 2 — Solution architecture, ADRs, and implementation plan
argument-hint: <PRD path | feature description>
---

# P2: Solution Architecture

**Input**: $ARGUMENTS — Read approved PRD + codebase profile first.

## 1. Solution Options

Propose 2-3 approaches. For each: description, architecture, pros, cons, complexity, risk. Recommend one.
Wait for user selection.

## 2. Technical Design

**Database**: Schema (tables, relations, indexes, constraints), migration strategy, rollback plan.

**API**: Endpoints table (method, path, auth, request/response), error format, auth strategy (JWT/session/OAuth), authz model (RBAC/ABAC).

**ADRs**: For each significant decision → `.claude/PRPs/adrs/ADR-{NNN}.md`
Format: Status → Context → Decision → Consequences (positive/negative) → Alternatives.

## 3. Implementation Plan

Generate at `.claude/PRPs/plans/{name}.plan.md` (use `/prp-plan` template):
- Summary, user story, complexity
- Mandatory reading (files + lines + why)
- Patterns to mirror (actual code snippets)
- Files to change (create/update + justification)
- Step-by-step tasks (ACTION, IMPLEMENT, MIRROR, VALIDATE)
- Testing strategy, validation commands
- Risks

## Gate

> **Phase 2 Complete.** Plan: `.claude/PRPs/plans/{name}.plan.md`
> - ✅ Approve → `/sdlc-design`
> - ✏️ Modify → update
> - 🔄 Different approach → re-evaluate

**STOP. Wait for user approval.**
