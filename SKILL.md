---
name: engineering-protocol
description: "Combined build protocol: source context → architecture methodology → pipeline → cleanup. Activated by trigger phrase 'build mode' or engineering-protocol script."
---

# Engineering Protocol

**One protocol to rule them all.** When "build mode" is activated, read this file and follow the entire workflow.

## How It Activates

- **Trigger phrase:** Luis says "build mode" (in any message) → I read this skill → protocol is live
- **Script:** `./scripts/pr-workflow start "name"` → also activates the protocol
- **Default:** Even without trigger, MEMORY.md directive means I use this for every coding task

---

## Phase 0: Init

1. Read this skill (done ✅)
2. Announce "Engineering mode activated" so Luis knows we're locked in
3. Start feature branch if not already on one
4. Proceed through phases

---

## Phase 1: Context

*Reference: [source-code-context](../source-code-context/SKILL.md)*

**Read only what you need.** Never dump the whole codebase.

1. **Map the scope** — what files / types / APIs does this task touch?
2. **Read selectively** in this order:
   - Main file(s) to be modified (read first)
   - Type definitions / schemas / interfaces they depend on
   - Parent components or imports (only if the task modifies interfaces)
   - Utilities / helpers (only if changing shared logic)
3. **Skip noise:**
   - `node_modules` (obviously)
   - Config files unless the task changes them
   - Files unrelated to current scope
   - The entire routing tree when changing one component
4. **Verify understanding** — exports, data flow, styling system, existing patterns

---

## Phase 2: Architecture

*Reference: [code-structure-methodology](../code-structure-methodology/SKILL.md)*

**Separate policy from mechanics from day one.**

| Concept | Definition | Where it lives |
|---------|-----------|----------------|
| **Policy** | Business logic, domain rules, decisions. Changes when the product changes. | Actions / routes / handlers |
| **Mechanics** | Implementation details, formatting, calculations, transforms. Rarely changes. | Services / utilities / lib |

### Golden Rule

> Policy calls mechanics. Mechanics never call policy.

```
actions/  ← policy (orchestration, business rules)
   ↓ calls
services/ ← mechanics (formatting, calculations, API normalization)
```

### Examples

```typescript
// BAD: mixed
function handleCheckout(req) {
  const tax = req.amount * 0.08       // mechanic
  const formatted = '$' + tax.toFixed(2) // mechanic
  if (req.user.tier === 'premium') { /* */ } // policy
}

// GOOD: separated
function handleCheckout(req) {
  const tax = calculateTax(req.amount)     // mechanic → service
  if (req.user.tier === 'premium') { /* */ } // policy → stays
}
```

### What to Extract (Mechanics)
- Price/currency formatting
- Date calculations
- String transformations
- Validation logic
- API response normalization
- Pagination math
- Sorting/filtering helpers

### What to Keep (Policy)
- Business rules ("free shipping over $50", discount logic)
- Decision logic ("if user is premium, show feature X")
- Authorization checks
- Route handler structure
- Error messages displayed to users

---

## Phase 3: Pipeline

*Reference: [agentic-engineering-workflow](../agentic-engineering-workflow/SKILL.md)*

Three-level pipeline: **Local → Staging → Main**

### Local
- You architect — own the architecture, I implement
- Feature plans first — write a plan before coding
- Knowledge-first — Phase 1 (context) ran already
- Tests — write them
- Feature first, tidy later — get it working, then clean up

### Staging
- **Every feature PR targets `staging`.** Never `main`.
- **PRs under 1,000 lines.** Quality degrades on larger PRs.
- **Stacked PRs ok** — split big features into sequential PRs.
- **Code review required** — fix any issues, push fixes.
- **Only staging graduates to main.**

### Main
- **Only staging graduates to main.** Nothing else.
- **Main is gold.** Breaks go through the full pipeline again.
- **Hotfix exception:** production-crippling bugs → direct PR to main.

### Tooling

| Command | What it does |
|---------|-------------|
| `./scripts/pr-workflow start "name"` | Creates branch, activates protocol |
| `./scripts/pr-workflow finish "msg"` | Commits, pushes, opens PR to staging |
| `./scripts/pr-workflow check` | Counts lines changed vs 1,000 limit |

---

## Phase 4: Cleanup

*Reference: [code-structure-cleanup](../code-structure-cleanup/SKILL.md)*

**Run *after* the PR is reviewed and merged to staging.** Not during development.

1. **Scan the diff** — what files changed?
2. **Identify duplication** — same logic in 2+ places?
3. **Extract mechanics** → services/ modules
4. **Keep policy** in callers
5. **No behavior changes** — structural only
6. **Verify** — types resolve, tests pass

Good extraction candidates:
- Formatting, date math, string transforms
- Validation schemas
- API response normalization

Do NOT extract:
- Business rules / decision logic
- Authorization checks
- Route structure
- Database query patterns (unless truly identical)

---

## Quick Reference Card

```
"build mode" / pr-workflow
        │
        ▼
╔══════════════════════════════════════╗
║         ENGINEERING PROTOCOL         ║
╠══════════════════════════════════════╣
║  1. READ  ← only what's needed      ║
║  2. SPLIT ← policy vs mechanics     ║
║  3. BUILD ← branch → PR → staging   ║
║  4. CLEAN ← extract dupes after     ║
╚══════════════════════════════════════╝
```
