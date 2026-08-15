# Focused Changes

- **Status:** Optional
- **Common best home:** Personal or team guidance; project guidance when its
  maintainers explicitly adopt it

## Intent

Prefer the smallest coherent change that fully achieves the requested outcome.
Limit unrelated churn so the diff is easier to understand, verify, review, and
revert.

This is about scope, not the fewest possible changed lines. A complete fix may
need coordinated changes across several files or surfaces.

## Good fit

- Localized bug fixes and small features
- Work in unfamiliar or high-risk code
- Repositories where drive-by refactors create review noise
- Tasks performed in a working tree that already contains unrelated changes

## Skip or adjust when

- A migration must be atomic across old and new representations
- A narrow patch would preserve incompatible states or duplicate systems
- Broader cleanup or architectural replacement is explicitly in scope
- Correctness requires updating every consumer, platform, or public contract

## Suggested instruction

```md
Keep changes focused on the requested outcome. Do not refactor, reformat, or
remove unrelated code. Make every coordinated change required for correctness,
and remove only artifacts made obsolete by the current work. Preserve unrelated
user changes.
```

## Trade-off

This pattern reduces accidental scope expansion and review noise. Applied too
rigidly, it can encourage incomplete migrations, local patches around a broken
boundary, or preservation of complexity that the authorized task should remove.
