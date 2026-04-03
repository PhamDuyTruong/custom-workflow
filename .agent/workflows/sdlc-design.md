---
description: Phase 3 — Spec-driven design (OpenSpec), UI/UX, atomic task breakdown
argument-hint: <plan path | feature description>
---

# P3: Design & Task Breakdown

**Input**: $ARGUMENTS — Read approved plan first.

## 1. Spec Artifacts

**If OpenSpec installed** (`which openspec`): run `/opsx:propose {feature-name}` → generates `openspec/changes/{name}/` with proposal.md, specs/, design.md, tasks.md.

**If not**: Create manually at `.claude/PRPs/specs/{name}/`:
- `proposal.md` — problem + proposed change + impact
- `specs/requirements.md` — FRs, NFRs, Given/When/Then scenarios
- `design.md` — architecture + data model + API + security
- `tasks.md` — implementation checklist

## 2. UI/UX (if frontend)

- User flow diagram (Mermaid)
- Component hierarchy
- Responsive breakpoints (mobile/tablet/desktop)
- Accessibility: keyboard nav, screen reader, WCAG AA contrast

Skip if backend-only.

## 3. Task Breakdown

Split into atomic tasks (max 1-2h each):

```
### Task {ID}: {Name}
- Type: Backend | Frontend | DB | Test
- Priority: P0-P3
- Depends: [task IDs]
- Action: {what}
- Files: {create/modify}
- Validate: {how to verify}
```

Separate Backend tasks and Frontend tasks. Define dependency graph.

## 4. Verification

List unclear points → ask user. Confirm scope boundaries.

## Gate

> **Phase 3 Complete.** Specs + {N} tasks.
> - ✅ Approve → `/sdlc-audit`
> - ✏️ Modify → update
> - ❓ Questions → discuss

**STOP. Wait for user approval.**
