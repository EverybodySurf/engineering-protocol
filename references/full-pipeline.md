# Full Pipeline Visual

```
┌─────────────────────────────────────┐
│            LOCAL                    │
│  You + Agent                        │
│  Plan → Build → Test                │
└──────────┬──────────────────────────┘
           │ push feature → PR to staging
           ▼
┌─────────────────────────────────────┐
│           STAGING                   │
│                                     │
│  Greptile Review → Score < 5/5?    │
│       │                │           │
│       │ No             │ Yes       │
│       ▼                ▼           │
│  Merge to         Grep Loop        │
│  Staging          (auto-fix)       │
│       │                 │          │
│       │                 └→ push    │
│       │                    fix     │
│       ▼                            │
│  Test on Staging                   │
└──────────┬──────────────────────────┘
           │ staging → main merge
           ▼
┌─────────────────────────────────────┐
│            MAIN                     │
│       Production Gold               │
└─────────────────────────────────────┘
```

## When to Skip

- **Hotfix**: direct PR to main with Greptile review, no staging
- **Prototypes/exploratory**: no pipeline needed, formalize when idea graduates
- **PR stuck at 4/5 after 5+ loops**: human call, merge if low risk
