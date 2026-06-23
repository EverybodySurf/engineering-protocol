# Engineering Protocol 🏄

> A unified four-phase build protocol for agentic software engineering.

## What This Is

The Engineering Protocol combines four proven practices into one linear workflow for building software with AI agents. It was born from the [SurfHub](https://github.com/EverybodySurf/SurfHub) project and extracted as a standalone open-source methodology.

## The Four Phases

```
┌─────────────────────────────────────────────┐
│  1. CONTEXT  → Read only what you need     │
│  2. ARCHITECTURE → Policy vs mechanics     │
│  3. PIPELINE → Branch → PR → staging       │
│  4. CLEANUP → Extract dupes after merge    │
└─────────────────────────────────────────────┘
```

### Phase 1: Context

Before writing a single line of code, read only the files that matter.

- Map the scope — which components/files/APIs will be touched?
- Read selectively: main files first, then types, then imports
- Skip noise: `node_modules`, unrelated configs, whole routing trees
- Verify understanding before writing

### Phase 2: Architecture

Separate **policy** from **mechanics** from day one.

| Policy (stays in actions/routes) | Mechanics (extract to services) |
|---------------------------------|--------------------------------|
| Business rules | Formatting, calculations |
| Decision logic | String transforms |
| Authorization | API normalization |
| Error messages | Pagination math |

**Golden rule:** Policy calls mechanics. Mechanics never call policy.

```typescript
// ❌ BAD: mixed
function handleCheckout(req) {
  const tax = req.amount * 0.08;              // mechanic
  const formatted = '$' + tax.toFixed(2);     // mechanic
  if (req.user.tier === 'premium') { /* */ }  // policy
}

// ✅ GOOD: separated
function handleCheckout(req) {
  const tax = calculateTax(req.amount);        // mechanic → service
  if (req.user.tier === 'premium') { /* */ }  // policy → stays
}
```

### Phase 3: Pipeline

Three-level pipeline with no skipping:

```
LOCAL → STAGING → MAIN
```

**Local:** You architect. The agent builds. Write tests. Feature first, tidy later.

**Staging:** Every PR targets `staging`. Under 1,000 lines. Code review required. Only staging graduates.

**Main:** Gold standard. Only staging graduates here.

### Phase 4: Cleanup

Run *after* the PR merges to staging — not during development.

1. Scan the diff
2. Identify duplication (same logic in 2+ places?)
3. Extract mechanics → service modules
4. Keep policy in callers
5. No behavior changes — structural only

## Activation

### Chat Trigger
Say `"build mode"` to your AI assistant → the full protocol activates.

### Script Trigger
```bash
./scripts/engineering-protocol "Add user authentication"
```

This creates a feature branch and activates the protocol banner.

## Tooling

| Tool | Purpose |
|------|---------|
| `scripts/engineering-protocol` | Create feature branch, activate protocol |
| `scripts/finish-feature.sh` | Commit, push, open PR to staging |
| `scripts/check-pr-size.sh` | Validate PR stays under 1,000 lines |

## Quick Reference Card

```
"build mode"
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

## License

MIT © Luis & Surf
