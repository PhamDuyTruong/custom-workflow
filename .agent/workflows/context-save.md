---
description: Save project context + append journal entry at end of session. Inspired by Trellis workspace journals.
---

# Context Save — End of Session

**When to use**: Before ending a conversation where significant work was done.

## Steps

### 1. Update Context File

Create/update `.claude/PRPs/context/{project-name}.context.md`:

```markdown
# {Project Name} — Session Context

## Last Updated
{date} — Session #{N}: {brief description}

## Current State
- **Phase**: P{N} — {phase name}
- **Branch**: {git branch}
- **Status**: {what's done, what's in progress}

## Architecture Decisions (Active)
- {List key ADRs and their status}

## Files Modified This Session
- {file path}: {what changed and why}

## Known Issues / Blockers
- {any bugs found, workarounds applied}

## Next Steps
1. {what to do next, in priority order}

## Tech Stack & Versions
- {framework}: {exact version}
- {key gotchas discovered}

## Key Patterns Established
- {e.g., "Theme managed via React Context in AppProviders.tsx"}

## PRP Document Locations
- PRD: {path}
- Plan: {path}
- Specs: {path}
- ADRs: {path}
```

### 2. Append Journal Entry

Append to `.claude/PRPs/context/{project-name}.journal.md`:

> Inspired by [Trellis workspace journals](https://github.com/mindfold-ai/trellis) — rolling session logs that preserve decision history.

```markdown
---
### Session #{N} — {date}
**Duration**: ~{time}
**Phase**: P{N}

#### What was done
- {bullet list of accomplishments}

#### Decisions made
- {why we chose X over Y}

#### Bugs found & fixed
- {bug}: {root cause} → {fix applied}

#### Lessons learned (for spec update)
- {gotcha discovered — e.g., "React 19: no setState in useEffect"}
- {pattern established — e.g., "Use next/dynamic ssr:false, not mounted pattern"}

#### Next session should
- {specific actionable tasks}
---
```

**Journal rules** (from Trellis):
- Max **2000 lines** per journal file
- When exceeded → create `{project-name}.journal-2.md`, etc.
- Never delete old journals — they are project memory

### 3. Update Specs with Lessons Learned

> Key Trellis principle: **"Don't just fix bugs — update specs when you discover new patterns"**

If any gotchas were discovered during the session, append them to relevant spec files:

```markdown
## Lessons Learned (auto-appended)

### {date}: {Framework} {Version} Gotchas
- ❌ Don't: {anti-pattern}
- ✅ Do: {correct pattern}
- 📍 Reference: {file where this was applied}
```

Update these spec files:
- `.claude/PRPs/specs/{name}.md` — design specs
- `.claude/PRPs/adrs/` — if new architecture decision

### 4. Update Plan Task Status

In the plan file, mark completed tasks:
```
- [x] Task B1: Theme + layout ✅
- [x] Task B2: Sidebar ✅
- [ ] Task D3: Export/Import (next)
```

### 5. Git Commit (if applicable)

```bash
git add -A
git commit -m "wip({scope}): {session summary}"
```

## Output

Context file + journal entry saved. Specs updated with lessons learned.
