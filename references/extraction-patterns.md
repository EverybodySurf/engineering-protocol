# Common Extraction Patterns

## Pattern 1: Formatting utilities
```typescript
// Before: inline in 3 components
const formatted = new Intl.NumberFormat('en-US', {
  style: 'currency', currency: 'USD'
}).format(price)

// After: shared utility
// src/lib/format.ts
export function formatPrice(price: number, currency = 'USD') {
  return new Intl.NumberFormat('en-US', {
    style: 'currency', currency
  }).format(price)
}
```

## Pattern 2: Date calculations
```typescript
// Before: same logic in 2 action files
const daysUntil = Math.ceil(
  (new Date(target).getTime() - new Date().getTime())
  / (1000 * 60 * 60 * 24)
)

// After
// src/lib/date.ts
export function daysUntil(date: Date | string): number {
  return Math.ceil(
    (new Date(date).getTime() - Date.now())
    / (1000 * 60 * 60 * 24)
  )
}
```

## Pattern 3: API response normalization
If 2+ endpoints transform API data the same way, extract the transform.

## Anti-patterns (don't extract)
- Single-use helpers (leave them inline)
- Business rules that differ slightly between callers (the differences are the policy)
- Everything into one giant `utils.ts` — extract by domain
