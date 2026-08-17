# Agent Instruction and Initialization Guidance: Primary-Source Review

**Research date:** 2026-08-17
**Scope:** Current first-party guidance related to `AGENTS.md` / `CLAUDE.md`, project initialization, context engineering, human-in-the-loop approval, and onboarding.
**Repository reviewed:** `farhadeidi/agents-md-guide` at commit `5775d8f`.

> Historical, non-normative research snapshot. Some recommendations below were
> implemented after this review. The root `README.md` and its linked guides are
> authoritative.

## Executive summary

The repository's central direction is well aligned with the current primary-source guidance: inspect the repository first, keep always-loaded instructions concise and high-signal, scope specialized guidance narrowly, prefer one canonical instruction source, and avoid replacing enforceable tooling with prose.

The largest gap is not instruction quality but initialization interaction design. The current Quick start asks an agent to inspect and then create or update files. It requires explicit consent for optional patterns and overlapping initialization methodologies, but it does not require the agent to show the maintainer its proposed understanding of the project, reconcile edits, or obtain approval of the exact content before writing. Anthropic's current experimental `/init` flow is the closest first-party precedent: it asks which artifacts to create, explores with a subagent, asks follow-up questions, and presents a reviewable proposal before any files are written. OpenAI's current guidance emphasizes user control, Plan mode, steering, review, and approval boundaries, but its public `/init` documentation does not state that generated `AGENTS.md` content is previewed and approved before it is written.

The strongest improvement would therefore be to make initialization a staged, human-reviewed protocol:

1. inspect without writing;
2. present an evidence-backed project understanding, including the proposed project summary;
3. let the maintainer approve, edit, reject, or answer material unknowns;
4. present the exact proposed artifact set and patch;
5. write only the approved version;
6. verify discovery, links, imports, and conflicts, then report the final result.

This should be a semantic approval process, not merely a filesystem permission prompt. A sandbox approval answers “may the agent write?”; it does not answer “is this project description true?”

This also addresses **Init Fossilization**: the risk that an initializer's first plausible—but incomplete or mistaken—interpretation of the product becomes durable repository policy, is repeatedly loaded by future agents, and gradually gains authority merely because it was written early. The project summary is the highest-leverage checkpoint because later rules, terminology, and routing decisions inherit its framing.

## Method and freshness caveat

Only first-party sources were used. “Latest” below means the latest relevant item visible in the official topic index or changelog on 2026-08-17, not a claim that no newer unrelated product announcement exists. Continuously maintained documentation pages from OpenAI, Anthropic, GitHub, and Gemini do not all expose a publication or revision date. For those pages, this report records the access date and does not infer a release date.

For OpenAI, sources were restricted to `developers.openai.com`, `platform.openai.com`, and `learn.chatgpt.com` as required. The former Codex documentation URLs currently redirect to official ChatGPT Learn pages.

## OpenAI Codex

### Current instruction discovery model

The official Codex `AGENTS.md` guide, accessed 2026-08-17, says Codex loads instruction files once per run/session. It reads global guidance first, then walks from the project root to the current directory; within each directory, `AGENTS.override.md` wins over `AGENTS.md` and configured fallback names. Nearer files appear later in the combined prompt and therefore take precedence. Empty files are skipped, and the combined project-document budget defaults to 32 KiB. The page recommends nested files for genuinely specialized scopes and provides commands for verifying which instruction sources loaded. [OpenAI Docs: Custom instructions with AGENTS.md](https://developers.openai.com/codex/agent-configuration/agents-md)

This supports the guide's existing principles of one root canonical entry, narrow nested scope, explicit compatibility files, and verification from the relevant working directory. It also suggests documenting Codex's exact override semantics and 32 KiB budget in a harness-compatibility note rather than assuming every agent merges files identically.

The Codex CLI page, accessed 2026-08-17, describes `/init` only as creating an `AGENTS.md` file with instructions for Codex. It does not document an inspect → proposal → user revision → write sequence. [OpenAI Docs: Codex CLI](https://developers.openai.com/codex/cli/features)

The official changelog records that on 2026-06-11 the desktop app added `/init` with the same initialization workflow as the CLI. The current public changelog still does not establish a pre-write semantic review gate. [OpenAI Docs: ChatGPT & Codex changelog](https://developers.openai.com/codex/changelog)

### Latest relevant Codex blog and product changes

As of 2026-08-17, the most recent post in the official Codex developer-blog topic index is **“Custom Code Review rules for Codex,” published 2026-07-20**. It says Codex Code Review can use concise, scoped `## Code Review Rules` in the applicable `AGENTS.md`, and recommends consequential, non-obvious invariants with an explicit safe path or exception. It explicitly leaves deterministic formatting and lint checks to CI. OpenAI reports that its primary evaluation suite recovered 98% of required custom findings with rule-guided variants versus 58.3% for the baseline, while also warning that broad rules create noise. [OpenAI Developers: Custom Code Review rules for Codex](https://developers.openai.com/blog/custom-code-review-rules-for-codex)

Implication: the guide should mention Code Review rules as a conditional, evidence-backed use of `AGENTS.md`, not as a default section. This is consistent with the existing “decision value, not volume” principle.

The prior Codex topic post, **“Mastering remote engineering work from your phone,” published 2026-06-23**, frames the human interface as a control plane: select repository, workspace, worktree, and boundaries before starting; use Queue for safe follow-up and Steer for mid-run correction; use Plan mode to inspect the path before implementation; inspect diffs and leave inline feedback; and choose the narrowest permission that keeps work moving. [OpenAI Developers: Mastering remote engineering work from your phone](https://developers.openai.com/blog/mastering-codex-remote-for-engineering)

Implication: initialization should surface a small number of high-value decisions at the moment they matter, support corrections without restarting the whole process, and conclude with an exact diff review.

The latest official Codex changelog visible on 2026-08-17 includes these relevant changes:

- **2026-08-11:** Codex added import/synchronization flows for setup and recent work from Claude Code, Claude Cowork, and Cursor.
- **2026-08-07 (Codex CLI 0.147.0):** automatic approval review became available through `--approve-for-me`; explicit trust is required for unfamiliar local projects.
- **2026-07-23:** multi-folder projects choose one primary folder for automatic `AGENTS.md`, skills, and `config.toml` discovery.
- **2026-07-21 (Codex CLI 0.145.0):** `/import` expanded to settings, MCP servers, plugins, sessions, commands, and project memories.

These changes make cross-harness provenance and the distinction between primary and secondary folders more important. An initializer should state which existing agent configuration it inspected or imported and should never imply that automatic tool approval is equivalent to maintainer approval of durable project policy. [OpenAI Docs: ChatGPT & Codex changelog](https://developers.openai.com/codex/changelog)

### Approval is not content validation

The current security documentation, accessed 2026-08-17, separates two controls: the sandbox determines what Codex can technically do, while the approval policy determines when it must pause before doing it. Auto mode can edit and run commands within a workspace without a prompt; read-only mode is recommended for planning without changes. Side-effecting connectors and destructive MCP calls can trigger approvals as well. The same page now documents automatic approval review, where a reviewer agent may approve eligible requests. [OpenAI Docs: Agent approvals & security](https://learn.chatgpt.com/docs/agent-approvals-security)

Therefore a guide-level approval checkpoint must be expressed as a workflow contract: “show the maintainer this draft and wait for acceptance.” It cannot be delegated to whatever approval policy happens to be active.

## Anthropic and Claude Code

### The most directly relevant current `/init` behavior

The current Claude Code memory documentation, accessed 2026-08-17, says ordinary `/init` analyzes the codebase and creates or improves a starting `CLAUDE.md`. With `CLAUDE_CODE_NEW_INIT=1`, it enables an interactive multi-phase flow that:

- asks which artifacts to set up (`CLAUDE.md`, skills, and hooks);
- explores the repository with a subagent;
- fills gaps with follow-up questions; and
- presents a reviewable proposal before writing any files.

This is the strongest first-party precedent for the requested human-in-the-loop initialization experience. The page does not expose a publication or update date, so this report treats it as current documentation as accessed, not as a dated release announcement. [Claude Code Docs: How Claude remembers your project](https://code.claude.com/docs/en/memory)

The same page offers several additional constraints worth incorporating into this repository's compatibility guidance:

- Treat instruction files as context, not hard enforcement. Use settings or hooks for controls that must execute regardless of model judgment.
- Target fewer than 200 lines per `CLAUDE.md`; prefer specific, concise, structured statements and path-scoped rules.
- Claude Code reads `CLAUDE.md`, not `AGENTS.md`; an official compatible approach is a small `CLAUDE.md` that imports `@AGENTS.md`, with Claude-specific additions only when necessary.
- Running `/init` can read other agents' configuration; the new flow reads `AGENTS.md` and several other tool formats. Cross-agent import therefore needs explicit provenance and review to prevent silent policy blending.
- `/memory` and `/context` expose what is loaded, which supports a post-write verification step.

### Human control and plan approval

Claude Code's permission-mode documentation, accessed 2026-08-17, says Plan mode researches and proposes changes without editing source. When ready, Claude presents the plan and offers approval choices, continued planning with feedback, or direct editing of the plan in the user's editor before proceeding. [Claude Code Docs: Choose a permission mode](https://code.claude.com/docs/en/permission-modes)

Anthropic's **“Trustworthy agents in practice,” published 2026-04-09**, argues that human oversight is most useful at the strategic level rather than as dozens of repetitive per-action prompts. It describes Plan Mode as showing an intended plan up front so the user can review, edit, and approve it before anything happens, while retaining the ability to intervene later. It also argues that agents must distinguish discoverable gaps from preference or intent questions that only the user can settle. [Anthropic Research: Trustworthy agents in practice](https://www.anthropic.com/research/trustworthy-agents)

Implication: do not turn initialization into approval fatigue. Require blocking review for semantic boundaries and durable policy—especially the project summary, canonical file choice, non-obvious invariants, and imported policy—then let the agent handle mechanical formatting inside those approved boundaries. A final exact-patch review catches unintended wording.

### Context engineering

Anthropic's **“Effective context engineering for AI agents,” published 2025-09-29**, treats context as a finite attention budget. It recommends the smallest set of high-signal tokens, clear and direct language at the right level of abstraction, distinct sections, just-in-time retrieval, and structured notes/compaction for long-running work. It warns against both brittle, over-prescriptive logic and vague guidance that assumes shared understanding. [Anthropic Engineering: Effective context engineering for AI agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)

This strongly validates the guide's “maximize decision value,” “preserve judgment,” and “route specialized workflows on demand” sections. The improvement opportunity is to apply the same economy to the initialization conversation: show evidence and decisions, not a transcript of every file inspected.

## Other official ecosystem signals

### Gemini CLI

Gemini CLI's project-context documentation was **last updated 2026-06-18**. It uses hierarchical global, workspace, and just-in-time context files; `/memory show` displays the exact concatenated context; and `context.fileName` can be configured with multiple names, including `AGENTS.md`, `CONTEXT.md`, and `GEMINI.md`. [Gemini CLI: Provide context with GEMINI.md files](https://geminicli.com/docs/cli/gemini-md/)

Implication: the canonical `AGENTS.md` policy is increasingly portable, but compatibility is configuration-dependent. The guide should provide a small, version-neutral compatibility matrix and an instruction-load verification step rather than saying only “compatible entry.”

### GitHub Copilot

GitHub announced `AGENTS.md` support for Copilot coding agent on **2025-08-28**, including nested files for scoped instructions. GitHub's current support table, accessed 2026-08-17, shows that support varies by product surface: the cloud agent accepts `AGENTS.md`, `CLAUDE.md`, and `GEMINI.md`; Copilot code review on GitHub uses `AGENTS.md`; and some IDE/code-review combinations support only GitHub-specific repository or path instructions. [GitHub Changelog: Copilot coding agent now supports AGENTS.md custom instructions](https://github.blog/changelog/2025-08-28-copilot-coding-agent-now-supports-agents-md-custom-instructions/) [GitHub Docs: Custom instruction support](https://docs.github.com/en/copilot/reference/custom-instructions-support)

Implication: “model-agnostic” should not be read as “all files behave identically in every harness.” Canonical content can be shared, but discovery, precedence, path scoping, size limits, and review behavior remain harness-specific.

## Assessment of this repository

### What is already strong

- **Inspect before writing:** matches both Claude's repository exploration and OpenAI's emphasis on scoped context.
- **Verified project explanation before rules:** good semantic onboarding, provided the maintainer can correct the explanation before it becomes durable policy.
- **One canonical instruction entry:** supported by Claude's official `@AGENTS.md` import pattern, Gemini's configurable filename, and GitHub's direct support.
- **Decision value over volume:** directly supported by Anthropic's context-engineering guidance and OpenAI's warning that broad review rules create noise.
- **On-demand routes and natural sources of truth:** aligns with just-in-time context and prevents always-loaded context from becoming an encyclopedia.
- **Explicit consent for optional patterns and overlapping initializers:** a strong provenance boundary, increasingly important now that Codex and Claude can import other agents' configuration.
- **No fixed questionnaire:** consistent with minimizing friction and asking only about preference or intent that repository inspection cannot establish.

### Recommended improvements, ordered by impact

#### 1. Add a mandatory “propose before writing” initialization contract

The core guide should require an initializer to remain read-only until the maintainer approves a proposal. This should apply to both new files and updates.

Minimum proposal:

- evidence-backed project summary;
- durable facts/invariants proposed for always-loaded context;
- material unknowns and why they matter;
- planned artifact set (root/nested files, compatibility imports/symlinks);
- existing guidance to preserve, reconcile, move, or remove;
- optional patterns, if any, presented separately with trade-offs.

The maintainer must be able to approve, edit, reject, or ask for more evidence. Silence is not approval.

#### 2. Make the project summary its own blocking checkpoint

The current guide correctly requires a concise project explanation, but that description is a high-leverage interpretation rather than a mechanical fact. Treating an unreviewed first interpretation as canonical creates the **Init Fossilization** risk described above. Before drafting the rest of the instruction file, show:

- the proposed summary;
- the repository evidence supporting it;
- any uncertain or inferred wording.

Continue only after the maintainer confirms or edits it. This prevents the rest of the guide from being organized around a subtly wrong product model.

#### 3. Separate semantic approval from execution permission

Add a short explicit distinction:

- **Execution permission:** whether the harness allows a file write, command, network request, or connector action.
- **Semantic approval:** whether the maintainer agrees that the durable content is accurate, appropriately scoped, and wanted.

Initialization requires semantic approval even when the harness is in auto, full-access, or automatically reviewed approval mode.

#### 4. Require an exact patch review before the first write

After the project understanding is approved, show the exact proposed content or diff. For multiple files, present the canonical `AGENTS.md` first, then small compatibility files and scoped files. Let the user edit at either level. Do not write any artifact until the patch is accepted.

For existing files, explain each deletion or relocation and preserve provenance. Do not conflate “user approved the project summary” with “user approved every policy change.”

#### 5. Add a compact state model instead of more prose

A small state model would make the workflow testable and portable:

`INSPECT → PROPOSE UNDERSTANDING → CONFIRM/REVISE → PROPOSE PATCH → CONFIRM/REVISE → WRITE → VERIFY`

Only `INSPECT` may run before the first approval. Any new material fact discovered after approval should return to the relevant proposal state rather than being silently added.

#### 6. Add a harness compatibility table

Keep it concise and clearly dated/maintained. At minimum record:

- Codex: direct `AGENTS.md`, `AGENTS.override.md`, root-to-CWD precedence, 32 KiB default combined budget.
- Claude Code: `CLAUDE.md` plus `@AGENTS.md` import; nested context can load on demand; behavioral instructions are not enforcement.
- Gemini CLI: default `GEMINI.md`, configurable `context.fileName` including `AGENTS.md`, hierarchical/JIT loading.
- GitHub Copilot: support varies by cloud agent, chat, code review, IDE, and CLI.

The table should route readers to vendor docs rather than duplicating every compatibility detail.

#### 7. Add conditional guidance for Code Review rules

When a repository actually uses Codex Code Review, allow a scoped `## Code Review Rules` section for consequential, non-obvious invariants that reviewers repeatedly explain. Require an explicit safe path/exception where relevant, and exclude checks already expressed reliably by CI or linters.

#### 8. Strengthen imported-policy provenance

The guide already asks for consent before overlapping initialization methodologies. Extend this to imported agent configuration:

- list every instruction file or imported configuration source;
- identify whether it is project-owned, personal, managed, generated, or external;
- show conflicts and the proposed resolution;
- never silently merge a generated or imported rule into canonical project policy.

#### 9. Add post-write load verification

After writing, verify both the filesystem result and the harness-visible result when a safe command exists. Check symlinks/imports from a fresh-checkout perspective and report which layers were loaded. This operationalizes the guide's existing verification section.

#### 10. Define an explicit non-interactive exception

If the guide ever supports CI or headless initialization, make it opt-in and conservative: generate a review artifact or patch, but do not commit or overwrite durable instruction files without an explicitly supplied acceptance mechanism. The interactive, maintainer-reviewed path should remain the default.

## Bottom line

The guide is already strong on what good instruction content looks like. Its next version should define how maintainers and agents agree on that content. The best design is neither a fully automatic `/init` nor a long questionnaire. It is a short sequence of evidence-backed semantic checkpoints, with the project summary and exact patch always requiring explicit human approval.
