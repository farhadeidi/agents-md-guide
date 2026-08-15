# Optional Instruction Patterns

This directory contains reusable behavioral guidance that a maintainer may
choose to adopt in personal, team, project, or scoped agent instructions.

Patterns are not universal best practices, initialization defaults, or a bundle
to install in full. Each one introduces a bias with a cost. A pattern belongs in
a repository only when its maintainers explicitly want every agent in that
scope to follow it.

## Adoption contract

An agent applying the main guide must:

1. Inspect the target repository before considering patterns.
2. Preserve and account for existing behavioral guidance.
3. Read a candidate pattern in full before recommending it.
4. Recommend only patterns that address a plausible need in that repository.
5. Explain why each pattern may help and where it may be counterproductive.
6. Ask once about the focused set of candidates; never present a fixed catalog
   questionnaire.
7. Add only patterns the user explicitly approves. No response means no
   adoption.
8. Adapt approved wording to the target without weakening its trade-off or
   duplicating existing policy.
9. Make the target instruction self-contained. The adopted policy lives in the
   target repository and must not depend on this catalog at runtime.

Choosing none is always valid and completes pattern selection.

## Catalog

| Pattern | Bias introduced | Common best home |
| --- | --- | --- |
| [Focused changes](focused-changes.md) | Prefer the smallest coherent diff that fully achieves the requested outcome | Personal or team; project when explicitly adopted |
| [Simplicity first](simplicity-first.md) | Prefer the least complex implementation that meets current requirements | Personal, team, or project |
| [Goal-driven verification](goal-driven-verification.md) | Define observable success and verify with relevant evidence | Team or project |

## Adding a pattern

A pattern must describe:

- its intended behavioral effect;
- situations where the bias helps;
- situations where it should be skipped or adjusted;
- concise suggested instruction text;
- the scope where it usually belongs; and
- the trade-off a maintainer accepts by adopting it.

Do not add a pattern that merely restates the main guide or differs from an
existing pattern only in wording.
