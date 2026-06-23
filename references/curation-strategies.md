# Curation Strategies by Task Type

## Adding a new component
1. Read 1-2 existing sibling components (pattern reference)
2. Read the page/layout that will use it
3. Read the design system primitives (Button, Card, etc.)
4. Skip: pages unrelated, global styles, API routes

## Refactoring a feature
1. Read the feature file(s) completely
2. Read test files for the feature
3. Read dependent components (callers)
4. Skip: other features, global config, unrelated utilities

## Adding an API endpoint
1. Read the route handler pattern (find an existing handler)
2. Read the database schema / type definitions
3. Read the service layer the handler delegates to
4. Skip: UI components, client code, unrelated routes

## Fixing a bug
1. Read the file where the bug manifests
2. Read the immediate callers / data flow
3. Read related test files
4. Skip: anything not on the code path

## When in doubt
Less is more with agent context. Start with the minimum viable set and let the agent ask if it needs more.
