# CLAUDE.md

@AGENTS.md

All portable project guidance lives in [`AGENTS.md`](AGENTS.md). Read it first. The notes below
are Claude Code harness deltas only.

- Use `AskUserQuestion` when the required base branch is not already explicit, and prefer
  `EnterWorktree` for the mandatory isolated worktree workflow in `AGENTS.md`.
- Subagents do not inherit this file. Propagate the `AGENTS.md` cross-session rules they need,
  especially the verbatim `git stash` ban, into every subagent prompt that touches Git.
- Override Superpowers paths: plans go to `_tasks/superpowers/plans/`, specs/designs to
  `_tasks/superpowers/specs/`, research to `_tasks/research/`, and hand-offs to
  `_tasks/hands-off/`. Commit durable artifacts inside the separate `_tasks/` repository.
- Override Claude Code's `/tmp/claude-*` scratch location with the repository's `_artifacts/`
  directory. Keep disposable output there; keep durable work in `_tasks/`.
- Before creating a branch or opening a PR, follow the base-green workflow in `AGENTS.md`. If the
  base is red, report inherited failures instead of repairing unrelated drift on the task branch.
