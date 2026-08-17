# Harness Compatibility

**Last verified:** 2026-08-17

Use this note when deciding how a target harness discovers, scopes, imports, or
overrides repository instructions. Shared support for `AGENTS.md` does not imply
identical behavior. Verify vendor documentation again when compatibility affects
the proposed artifact set.

| Harness | Canonical compatibility approach | Important discovery behavior |
| --- | --- | --- |
| OpenAI Codex | Use `AGENTS.md` directly | Loads global guidance, then project files from root to the current directory. `AGENTS.override.md` has directory-level priority; nearer files appear later. The default combined project-document budget is 32 KiB. |
| Anthropic Claude Code | Use a small `CLAUDE.md` that imports `@AGENTS.md`, or a relative symlink when the repository and platform support it | Reads `CLAUDE.md`; scoped context and imports have Claude-specific loading behavior. Instruction prose supplies model context rather than hard enforcement. |
| Gemini CLI | Configure `context.fileName` to include `AGENTS.md`, or maintain the smallest justified `GEMINI.md` compatibility entry | Supports hierarchical and just-in-time context. `/memory show` exposes concatenated loaded context. |
| GitHub Copilot | Prefer direct `AGENTS.md` where the target surface supports it; use GitHub-specific repository or path instructions only when required | Support differs across the coding agent, code review, IDE, chat, and CLI surfaces. Confirm the exact surface instead of assuming repository-wide parity. |

## Authoring implications

- Keep `AGENTS.md` canonical and avoid copied policy in compatibility files.
- Create compatibility entries only for harnesses evidenced in the repository.
- Verify imports, symlinks, fallback names, scope, precedence, and loaded context
  from the working directories where agents actually run.
- Use settings, hooks, CI, permissions, or executable checks for requirements
  that must be enforced regardless of model judgment.
- Treat imported configuration as input to the human-validated initialization
  workflow, not as automatically accepted project policy.
- Recheck limits and discovery behavior before relying on nested instructions or
  multi-folder workspaces.

## Current official references

- [OpenAI Codex: Custom instructions with AGENTS.md](https://developers.openai.com/codex/agent-configuration/agents-md)
- [Anthropic Claude Code: How Claude remembers your project](https://code.claude.com/docs/en/memory)
- [Gemini CLI: Provide context with GEMINI.md files](https://geminicli.com/docs/cli/gemini-md/)
- [GitHub Copilot: Custom instructions support](https://docs.github.com/en/copilot/reference/custom-instructions-support)

These pages are maintained continuously and may change without a versioned
release. Update the verification date whenever this file's claims are rechecked.
