# AGENTS.md Guide

A model-agnostic guide for writing concise, project-specific instructions in
`AGENTS.md`, `CLAUDE.md`, and compatible agent entry files.

This project does not introduce a new instruction format, agent harness, or
project workflow. It defines a small set of rules that any capable coding agent
can apply after inspecting a repository.

## Quick start

From the repository you want to initialize or update, give your preferred
coding agent access to this guide:

```text
Read https://github.com/farhadeidi/agents-md-guide in full.

Then inspect this repository and create or update its agent instruction files
according to that guide. Preserve useful existing guidance, do not invent
project requirements, and keep AGENTS.md as the canonical instruction source.
Inspect before asking questions, and ask only about material facts that cannot
be discovered from the repository. If no change is warranted, explain why and
leave the repository unchanged. If any optional patterns in this guide are
relevant, present only those patterns with their trade-offs and add them only
after explicit user approval.
```

The agent should inspect before writing, ask only necessary questions, and
produce the smallest useful result. Depending on the repository, that may be:

- a concise root `AGENTS.md`;
- a compatible entry such as `CLAUDE.md` pointing to `AGENTS.md`;
- a careful update to useful existing guidance;
- explicitly approved optional patterns adopted as project policy; or
- no change when additional instructions would provide no value.

The result must be adapted to the repository, not copied from this README or a
universal template.

## Purpose

Create the smallest set of durable instructions that helps an agent make better
decisions in the repository in front of it.

The result must be specific to that repository. It must not be a generic
template filled with inferred stack details or broadly applicable engineering
advice.

A valid result may be no change, or no instruction file at all, when the
repository has no durable guidance worth loading on every task.

## Core rules

### Inspect before writing

Read the repository before creating or changing agent instructions. Inspect the
source, configuration, scripts, tests, CI, documentation, existing instruction
files, and relevant history. Scale the investigation to the repository and the
guidance being considered.

Determine the audience and ownership of existing instruction files before
changing them. An `AGENTS.md` may contain coding guidance, contribution policy,
security routing, legal requirements, or another intentionally narrow policy.
Do not silently repurpose it.

Do not invent requirements, risks, conventions, commands, or workflows that the
repository does not support.

### Resolve material unknowns

Inspect the repository before asking questions. Ask the user only when an
answer cannot be discovered from repository evidence and would materially
change the instructions or the authorized scope.

Group related questions, keep them focused, and explain why each answer matters.
Do not ask about facts that can be verified from source, configuration,
documentation, history, or tooling.

When uncertainty affects only optional guidance, omit that guidance and report
the omission instead of blocking initialization. Never turn initialization into
a fixed questionnaire.

Choosing optional behavioral patterns is a legitimate user decision, but ask
only after inspection and present only patterns relevant to the repository.
Explain the trade-off of each option, make no selection by default, and treat
choosing none as a complete answer.

### Keep one canonical instruction entry

When project instructions are warranted, use the root `AGENTS.md` as their
canonical entry. Other agent entry files must point to it rather than maintain
independent copies of the same policy.

Use a relative symlink such as `CLAUDE.md -> AGENTS.md` when the repository,
platform, and target harness support it. Otherwise, use the smallest compatible
pointer or import. Do not duplicate the contents manually.

Create compatibility entry files only for harnesses the project actually uses.

### Maximize decision value, not instruction volume

Every line in an always-loaded instruction file consumes attention on every
task. Include an instruction only when it is:

- specific to the repository;
- not reliably, cheaply, and consistently derivable when the agent needs it;
- relevant across many tasks in its scope;
- durable or important enough to maintain alongside the project;
- actionable by an agent; and
- more valuable than the context and maintenance cost it introduces.

Concise architecture maps, package boundaries, or product-surface lists may be
worth including even when they are technically discoverable, if they prevent
significant search, ambiguity, or changes in the wrong place.

Brevity is a design constraint, not a fixed line limit. Remove content until
removing more would make agents less reliable.

High-value content commonly includes non-obvious terminology, project
invariants, costly risks, boundaries that are easy to violate, concise
orientation that prevents expensive rediscovery, targeted validation commands,
and routes to canonical documentation.

Exclude generic engineering advice, exhaustive maps that do not improve
decisions, formatting rules already enforced by tools, speculative risks,
copied documentation, personal preferences presented as project policy,
unverified capabilities, and temporary state.

Generic behavioral guidance must not be added by default. It becomes project
policy only when maintainers explicitly adopt it, accept its trade-offs, and
want every agent working in that repository to follow it.

### Put information in its natural source of truth

`AGENTS.md` is the canonical entry point for agent-facing policy, not an
encyclopedia and not the source of truth for every project fact.

- Source and configuration define how the system behaves.
- Tests, linters, hooks, and CI enforce mechanical requirements.
- Project documentation explains architecture, decisions, and operations.
- Issue trackers and planning systems hold live work state.
- Agent instructions contain durable constraints and route agents to the right
  source when a task needs more context.

Keep each rule in one canonical location. Link to it elsewhere instead of
maintaining summaries that can drift.

### Route specialized workflows on demand

Route infrequent or detailed review, release, deployment, migration, and testing
procedures out of always-on context. Keep compact preconditions in `AGENTS.md`
when they apply frequently or prevent immediate, expensive, or irreversible
harm. Point to a focused project-owned document or existing skill for the full
procedure and state exactly when it must be read.

Use direct routing language:

```md
Before reviewing changes, read `docs/reviewing.md` in full.
Before publishing a release, read `docs/releasing.md` in full.
When changing the database schema, follow `docs/schema-migrations.md`.
```

Only add a route when the destination exists, is current, and contains
project-specific information that materially changes the workflow.

### Scope instructions narrowly

Keep repository-wide instructions at the root. When a substantial subtree has
different constraints, use a nested instruction file only if the target
harnesses support its scope and precedence semantics.

Do not create nested files merely to describe directory contents. If harness
behavior is uncertain, prefer a root-level route to the relevant project
documentation over relying on unsupported precedence.

### Preserve useful existing guidance

When adopting an existing repository, read all applicable instruction files
before editing them. Preserve their audience, project-specific knowledge, and
authoritative policy. Reconcile conflicts, and remove content only when it is
demonstrably obsolete, duplicated, or in the wrong location.

Do not replace an existing instruction file merely to make it match a preferred
shape.

### Keep requirements proportional

Document only controls the repository actually requires. Do not introduce
mandatory reviews, approval gates, branch strategies, work tiers, hooks, CI, or
specialized skills as initialization ceremony.

Distinguish clearly between requirements enforced by tooling and advice that
depends on agent judgment.

### Keep volatile state out

Do not record current branches, issue counts, active work, deployment state,
temporary incidents, roadmap status, or other rapidly changing facts in durable
agent instructions.

When an agent needs live state, point it to the command or system that provides
the current value instead of copying the value into documentation.

Evolving product contracts such as supported platforms, public surfaces, or
package boundaries may still belong in agent instructions when they directly
affect implementation decisions. Treat them as maintained code: update or
remove the instruction in the same change that makes it inaccurate.

### Keep instructions current

Review agent instructions when changing a workflow, command, path, invariant,
or architectural boundary they describe. Update the canonical instruction and
its routes in the same change instead of leaving cleanup for later.

Do not preserve stale guidance merely because it was once correct. Durability
means clear ownership and change-coupled maintenance, not immutability.

### Make instructions verifiable

Before finishing:

- confirm every referenced path exists;
- validate documented commands non-destructively by inspecting their definitions
  or using a safe help, dry-run, or targeted path;
- verify symlinks and imports from a fresh checkout perspective;
- search for contradictory or duplicated instructions;
- ensure compatibility files do not contain independent policy; and
- reread the final instructions as a complete document.

Never execute a destructive, external, privileged, or costly command merely to
verify documentation.

If a statement cannot be verified, omit it or label the uncertainty instead of
presenting it as project policy.

## Optional patterns

The [`patterns/`](patterns/) catalog contains reusable behavioral guidance that
maintainers may explicitly adopt. Patterns are suggestions, not part of the
core standard and never automatic initialization defaults.

Each pattern explains its intent, suitable and unsuitable contexts, suggested
instruction text, best scope, and trade-off. An initializer must inspect the
repository first, read a candidate pattern in full before recommending it,
recommend only relevant patterns, and obtain explicit user approval before
adding any of them to project instructions.

The initial catalog includes:

- [Focused changes](patterns/focused-changes.md)
- [Simplicity first](patterns/simplicity-first.md)
- [Goal-driven verification](patterns/goal-driven-verification.md)

## Non-goals

This guide does not:

- compete with or replace `AGENTS.md`;
- prescribe an agent harness or model;
- generate files through a CLI or service;
- require a skill, plugin, schema, or manifest;
- define a universal software-development workflow; or
- require additional project structure without repository evidence.

The guide owns only the rules for producing effective project-specific agent
instructions. The repository and its maintainers own everything else.

## License

Released under [CC0 1.0 Universal](LICENSE). You may copy, modify, distribute,
and use this work for any purpose without asking permission.
