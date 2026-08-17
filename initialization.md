# Human-Validated Initialization

Use this workflow whenever creating or updating repository agent instructions.
It keeps repository inspection efficient while reserving project meaning,
durable policy, and file mutation for explicit maintainer decisions.

Initialization follows this state model:

```text
INSPECT
  → PROPOSE UNDERSTANDING
  → CONFIRM OR REVISE UNDERSTANDING
  → RESOLVE MATERIAL CHOICES
    ├─ CONFIRM NO CHANGE → VERIFY
    └─ PREVIEW EXACT PATCH
       → CONFIRM OR REVISE PATCH
       → WRITE
       → VERIFY
```

Before `WRITE`, remain read-only in the target repository. Silence, filesystem
permission, an automatic approval system, and approval of an earlier checkpoint
do not authorize a later checkpoint.

## One-question interaction rule

Each user-facing message may ask at most one question or request one approval
decision. Provide the evidence, context, and options needed for that single
decision, then stop and wait for the maintainer's answer. Incorporate the answer
into the working understanding before asking the next question.

A compound prompt containing several unknowns, approvals, or adoption choices is
still several questions even if it ends with one question mark. Sequence such
decisions by dependency and value. This rule applies to factual unknowns,
project-understanding review, policy choices, artifact choices, optional
patterns, and exact-patch approval.

## 1. Inspect

Inspect enough repository evidence to account for the proposed instructions:

- existing root, nested, personal, managed, generated, and imported instruction
  sources that may apply;
- product and domain documentation;
- source boundaries, public surfaces, packages, adapters, and delivery targets;
- configuration, scripts, tests, CI, hooks, and executable enforcement;
- relevant history when it explains a durable but otherwise invisible decision;
  and
- harness-specific configuration that demonstrates actual usage.

Use ordinary read-only inspection without asking for approval file by file. Ask
the maintainer to supply a fact only when repository evidence cannot establish
it and the answer would materially change the result. When several material
unknowns remain, ask the highest-dependency question first, wait for its answer,
update the synthesis, and then decide whether the next question is still needed.

**Complete when:** every claim in the proposed understanding can be tied to
repository evidence, marked as inference, or named as a material unknown.

## 2. Propose the project understanding

Present a compact `Project understanding` containing:

1. **Project summary:** what the product, service, library, or other artifact is;
   who or what it serves; and its essential operating model.
2. **Non-negotiable qualities:** product promises, invariants, and boundaries
   that appear durable and materially affect implementation decisions.
3. **System shape:** how important surfaces or components relate, including
   intended differences between them.
4. **Vocabulary:** only domain terms whose meaning would otherwise be ambiguous.
5. **Evidence:** the small set of paths or artifacts supporting the synthesis.
6. **Uncertainty:** every material inference, conflict, or unresolved item,
   expressed as status rather than a bundled request for answers.

Separate verified facts from interpretations. Do not present a directory or
technology inventory as a project summary.

End with one explicit request to approve the understanding or provide an edit.
If the response requests more evidence or rejects the model, address that
response before asking a new question. Do not draft durable instruction policy
from an unconfirmed project model.

**Complete when:** the maintainer explicitly confirms the understanding, or
provides edits that are incorporated and shown back until confirmed.

## 3. Resolve material choices and draft

Using only the confirmed project understanding, identify:

- each durable fact, invariant, hazard, boundary, verification route, or
  workflow trigger proposed for always-loaded context;
- each existing instruction to preserve, reconcile, move, or remove, with a
  reason for every deletion or relocation;
- the proposed canonical, compatibility, and nested files, including why each
  file is warranted and which harness discovers it;
- specialized material that should remain in its natural source or be reached
  through a conditional pointer;
- candidate optional patterns, each presented as a separate adoption decision
  with its benefits and costs; and
- facts deliberately excluded because they are generic, mechanical, volatile,
  speculative, duplicated, or cheap to rediscover.

List every instruction source that influences the draft and identify its
ownership. If an overlapping external initializer or imported configuration is
involved, apply the provenance rules in the main guide before using it.

Ask the maintainer only about choices that would materially change the policy or
artifact set and cannot be resolved from repository evidence or the confirmed
understanding. Resolve those choices one question at a time. Present each
optional pattern as its own adoption decision with its benefit and cost. Once
the material choices are settled, draft the exact patch without adding a
separate approval round for a restatement of the plan.

When no change is warranted, present the evidence and rationale and ask for one
confirmation. After confirmation, skip patch preview and writing, verify that
the repository remains unchanged, and hand off the result.

**Complete when:** every material choice needed to draft the patch is resolved,
or the maintainer has confirmed the no-change conclusion.

## 4. Preview the exact patch

Show the complete proposed content or exact diff before changing files. For
multiple artifacts, present the canonical `AGENTS.md` first, followed by scoped
instructions and small compatibility entries.

The preview must expose every addition, deletion, move, import, and symlink.
Approval of the project understanding does not imply approval of the wording.
When the maintainer edits or rejects part of the patch, revise and show the
affected patch again. Ask only one question: whether the exact current patch is
approved or needs an edit.

**Complete when:** the maintainer explicitly approves the exact current patch.

## 5. Write

Apply only the approved patch. Preserve unrelated working-tree changes and stop
if the target files changed after the preview in a way that could invalidate the
approved diff.

If a material new fact is discovered during writing, return to the earliest
checkpoint it affects. Do not silently add it. Cosmetic formatting that changes
the approved content must also be included in a renewed patch preview.

**Complete when:** the filesystem matches the approved patch and no unapproved
instruction change is present.

## 6. Verify and hand off

Run the verification checklist in the main guide. In addition:

- compare the final diff with the approved patch;
- verify imports and relative symlinks from a fresh-checkout perspective;
- check instruction discovery and precedence from each relevant working
  directory when the harness exposes a safe inspection command;
- search applicable layers for contradictions, duplication, blind references,
  stale generated content, and deterministic rules that belong in tooling;
- report skipped checks and their reason; and
- name every created, changed, preserved, or deliberately omitted artifact.

**Complete when:** the maintainer receives the final diff, verification evidence,
and any remaining uncertainty.

## Corrections, cancellation, and non-interactive use

A maintainer may revise an approved decision at any point. Return to the earliest
affected checkpoint, preserve unaffected decisions, and continue from there.

Cancellation before `WRITE` leaves the target repository unchanged. Cancellation
after a partial write stops further mutation and reports the exact current diff;
it does not assume permission to revert unrelated or pre-existing work.

Non-interactive initialization is opt-in. It may produce a review artifact or
patch, but it must not overwrite or commit durable instruction files unless the
caller supplies an explicit acceptance mechanism for the exact patch. The
interactive workflow remains the default.
