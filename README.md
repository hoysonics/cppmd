# cppmd

A portable coding-agent skill for these commands:

| Command | Actions |
| --- | --- |
| `cpp` | Commit all changes, push, create/update PR |
| `cppm` | `cpp` plus merge |
| `cppmd` | `cppm` plus deploy |

The skill preserves the repository's existing Git identity and authenticated account. It includes commit-message rules, push-report templates, and a PR template. Push reports are status messages; Git push does not accept a custom message.

## Use with any agent

Ask the agent: “Read `<path>/SKILL.md` and run `cpp`, `cppm`, or `cppmd` in this repository.” The instructions do not depend on an agent-specific API. Git, repository-host authentication, and the project's deployment tooling must already be available for the corresponding stages.

## Codex

Place or symlink the repository folder at `~/.codex/skills/cppmd` and invoke `$cppmd`, specifying a shorter mode if wanted. If that destination already exists, back it up or choose a separate install location before replacing it. Keep `templates/` alongside `SKILL.md`.

## Claude

Place or symlink the repository folder at `~/.claude/skills/cppmd` for personal use, or `.claude/skills/cppmd` for a project, and invoke `/cppmd`. Specify `cpp` or `cppm` in the request for a shorter workflow. Keep `templates/` alongside `SKILL.md`.

## Other agents

Use the direct-file instruction above, or register `SKILL.md` through the agent's supported custom-instruction/skill mechanism. Automatic command discovery depends on the host agent; the workflow itself is portable.

Creating or installing this skill does not run commit, push, merge, or deploy. Those actions occur when the workflow is invoked.
