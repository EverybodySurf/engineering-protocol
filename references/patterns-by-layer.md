# Common Layer Structures

## Feature-based structure
```
src/features/checkout/
  actions/
    place-order.ts       ← policy: orchestration, business rules
    cancel-order.ts      ← policy
  services/
    pricing.ts           ← mechanics: tax calc, discounts
    order-validation.ts  ← mechanics: field validation
  types.ts               ← shared types
```

## Utility-based structure
```
src/
  lib/
    format.ts            ← mechanics: formatting helpers
    date.ts              ← mechanics: date calculations
    validation.ts        ← mechanics: reusable validators
  features/
    orders/
      actions/
        create.ts        ← policy
        refund.ts        ← policy
```

## When a mechanic becomes policy
If a mechanic starts accumulating conditions and branches, it's becoming policy. Don't fight it — promote it back to the action layer or create a dedicated domain service.

## Test boundaries
- Policy tests: mock mechanics, test decision logic
- Mechanic tests: pure unit tests, no mocks needed
