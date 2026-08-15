# Goal-Driven Verification

- **Status:** Optional
- **Common best home:** Team or project guidance

## Intent

Translate a task into observable success criteria and gather the smallest
relevant evidence that those criteria are met. Verification should follow the
risk and affected surfaces, not a ritual command list.

## Good fit

- Bug fixes with reproducible failures
- Behavior changes that can be demonstrated through focused tests or checks
- Multi-step work where intermediate outcomes can drift from the goal
- Repositories with several surfaces that must remain consistent

## Skip or adjust when

- The task is exploratory and the desired outcome is discovery
- A trivial prose or metadata edit has no meaningful runtime behavior
- Relevant verification is destructive, external, privileged, or costly
- The repository lacks a reliable automated check for the affected behavior

In the last two cases, report the limitation rather than inventing evidence or
running an unsafe check.

## Suggested instruction

```md
Define observable success before implementing. Verify the result with the
smallest relevant checks and affected surfaces. Do not substitute unrelated
repo-wide checks for evidence about the requested behavior. Report what was
verified, what was not, and why.
```

## Trade-off

This pattern improves alignment and makes completion evidence explicit. Applied
to every trivial edit, it adds ceremony; applied mechanically, it can optimize
for passing available checks while missing the user's actual outcome.
