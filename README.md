# cppmd

A portable coding-agent skill that takes your working changes all the way to production:

| Command | Actions |
| --- | --- |
| `cpp` | Commit all changes, push, create/update PR |
| `cppm` | `cpp` plus merge |
| `cppmd` | `cppm` plus deploy |

The skill preserves the repository's existing Git identity and authenticated account. It includes commit-message rules, push-report templates, and a PR template. Push reports are status messages; Git push does not accept a custom message.

## Get started

### 1. Check prerequisites

- `git`, with push access to your repository's remote
- [GitHub CLI](https://cli.github.com/) (`gh`) logged in (`gh auth status`) for PR creation and merging on GitHub
- For `cppmd`: the project's existing deployment tooling (CI workflow, platform CLI, or deploy script)

### 2. Install

One line, for every agent found on your machine (Claude Code in `~/.claude`, Codex in `~/.codex`):

```sh
curl -fsSL https://raw.githubusercontent.com/hoysonics/cppmd/main/install.sh | bash
```

Choose targets explicitly by passing options after `bash -s --`:

```sh
curl -fsSL https://raw.githubusercontent.com/hoysonics/cppmd/main/install.sh | bash -s -- --claude
```

| Option | Installs to |
| --- | --- |
| `--claude` | `~/.claude/skills/cppmd` (personal, all projects) |
| `--codex` | `~/.codex/skills/cppmd` |
| `--all` | Both of the above |
| `--project` | `./.claude/skills/cppmd` in the current repository, as a copy you can commit for your team |

The installer clones this repository to `~/.local/share/cppmd` and symlinks it into each agent's skills folder. An existing `cppmd` folder that the installer did not create is moved to `~/.local/share/cppmd-backups/`, never deleted.

<details>
<summary>Manual install</summary>

```sh
git clone https://github.com/hoysonics/cppmd.git ~/.local/share/cppmd
ln -s ~/.local/share/cppmd ~/.claude/skills/cppmd   # Claude Code
ln -s ~/.local/share/cppmd ~/.codex/skills/cppmd    # Codex
```

Keep `templates/` alongside `SKILL.md`. Running `./install.sh` from your own clone links to that clone instead.

</details>

### 3. Run it

Start a new agent session in a Git repository with changes, then:

| Agent | Invoke |
| --- | --- |
| Claude Code | `/cppmd`, or ask: "run cpp" / "run cppm" |
| Codex | `$cppmd`, or ask for `cpp` / `cppm` |
| Any other agent | "Read `~/.local/share/cppmd/SKILL.md` and run `cpp` in this repository." |

A `cpp` run looks like this:

```text
Pushed a36dfa3 to origin/feat/login-retry.
PR: https://github.com/you/app/pull/42
Validation: npm test (128 passed)
```

When the changes span several concerns, the agent first groups them and asks how to split them:

```text
| Label | Group      | Files | Depends on | Planned commit / PR title             |
|-------|------------|-------|------------|---------------------------------------|
| A     | config     | 1     | -          | chore(build): clarify fallback values |
| B     | auth-retry | 6     | A          | feat(auth): retry expired sessions    |
| C     | docs       | 2     | -          | docs: document session retry          |

1) Single PR, one commit   2) Single PR, grouped commits   3) Split PRs (stacked where dependent)
4) Partial, e.g. "4 A,B"   5) Dry-run   6) Abort   e) Edit
```

Skip the question by stating the choice up front, for example "cpp grouped commits", "cpp split PRs", or "cpp dry-run". A single coherent change proceeds without asking.

The agent stops and asks instead of guessing when the Git account is ambiguous, a deploy target is not defined, required checks or reviews are missing, or a conflict needs a decision. It never force-pushes, bypasses branch protection, or switches your global Git/GitHub account.

### 4. Update or uninstall

```sh
# Update: re-run the installer (pulls the latest version)
curl -fsSL https://raw.githubusercontent.com/hoysonics/cppmd/main/install.sh | bash

# Uninstall: removes only the links/copies the installer created
curl -fsSL https://raw.githubusercontent.com/hoysonics/cppmd/main/install.sh | bash -s -- --all --uninstall
```

## Other agents

Use the direct-file instruction above, or register `SKILL.md` through the agent's supported custom-instruction/skill mechanism. Automatic command discovery depends on the host agent; the workflow itself is portable.

## What's inside

| File | Purpose |
| --- | --- |
| `SKILL.md` | The workflow: account handling, commit/push/PR, merge, deploy, and blockers |
| `templates/commit-message.md` | Commit message formats |
| `templates/grouping-plan.md` | Grouping plan shown before committing multi-concern changes |
| `templates/pull-request.md` | Fallback PR title and body when the repository has no template |
| `templates/push-message.md` | Status report formats after push, merge, and deploy |
| `install.sh` | Installer for Claude Code, Codex, and project-level use |

Installing this skill does not run commit, push, merge, or deploy. Those actions happen only when you invoke the workflow.
