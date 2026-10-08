---
name: cppmd
description: Commit all repository changes, push and open a pull request (cpp), optionally merge (cppm) and deploy (cppmd), preserving the existing Git account and using consistent commit and push reporting templates. Use when the user invokes these workflows.
---

# CPP workflows

These instructions are tool-independent Markdown for Codex, Claude, and other coding agents. Use the available shell, Git client, or hosting tools; no particular agent API is required.

## Modes and authorization

- `cpp`: commit all changes → push → create or update a ready-for-review PR.
- `cppm`: `cpp` → merge.
- `cppmd`: `cppm` → deploy the merged revision.

An invocation authorizes its listed actions in the current repository. Continue without repeated routine approval; honor execution-environment restrictions and ask only for missing decisions or credentials. Do not execute later stages for an earlier mode. Follow applicable repository instructions and established conventions.

## Reuse the previous Git account

Before committing or pushing, inspect `git status`, branch/upstream, `git remote -v`, effective `git config --show-origin --get user.name` and `user.email`, and recent commit authors. Do not print tokens, private keys, or credential-helper output.

Preserve existing repository-local or inherited author identity, signing settings, remote URLs, SSH host aliases, and credential helpers. An SSH alias or URL username can select the repository's account. Commit author identity and the account authenticating a push are distinct; verify both rather than assuming an email selects authentication.

Use existing credentials for the remote host. For GitHub CLI operations, check its active account and repository access with `gh auth status` and read-only repository queries when available. Reuse the previously associated account, including an existing host/account-specific mechanism, without changing the machine's default account. Do not run `gh auth setup-git`, switch global accounts, rewrite remotes, or replace Git configuration merely to make the workflow succeed. Do not choose an account from a hard-coded username.

If author identity is missing, recent human commit history is a hint, not proof of ownership. Confirm the identity if it cannot be established from repository/session context, and use repository-local configuration if needed. If credentials are missing, mismatched, or multiple accounts are ambiguous, stop before the affected mutation and request login or the account choice. Never invent an email or silently use another account. For a new repository with no history or identity, ask for the intended identity.

## Commit, push, and PR

1. Inspect all tracked and untracked changes and the branch diff against the intended PR base. Include pre-existing user changes in the requested all-changes commit; never discard them. Respect ignore rules. Do not force-add generated files, secrets, or credentials. Stop for specific suspected secrets that would be committed.
2. Run relevant tests and required repository checks. Fix failures within scope. Report checks actually run; do not claim unrun checks passed or publish known failing work without explicit direction.
3. Continue on the appropriate feature branch. From a default or protected branch, create a descriptive feature branch. Infer the remote/base from upstream and repository context; ask only if ambiguous.
4. Stage all intended changes, inspect the staged diff, and commit using the rules below. Skip empty commits. If only existing unpublished commits remain, continue with them.
5. Push the feature branch, setting upstream when missing. Use normal pushes. Do not force-push, rewrite shared history, or push directly to a protected/default branch without separate explicit direction. Resolve routine conflicts while preserving both sides; stop for substantive unresolved decisions.
6. Create a PR or update the existing PR for this branch, using the repository template first and [the PR template](templates/pull-request.md) otherwise. Describe the final behavior and real validation. With `gh`, use `--body-file` for multiline descriptions. No base-relative changes means no new PR is needed.

## Commit message rules

Prefer established repository conventions. Otherwise use Conventional Commit style:

`<type>(<optional scope>): <imperative summary>`

Use `feat`, `fix`, `docs`, `refactor`, `test`, `build`, `ci`, `perf`, `style`, or `chore` as appropriate. Omit empty scope parentheses. Keep the subject concise, preferably under 72 characters. Explain what changed and why; avoid vague subjects such as “updates” or “fix stuff.” Use the repository's usual language, otherwise English.

Use a blank line before an optional body. Include motivation, material behavior changes, and actual validation when useful; omit empty sections and placeholders. Use `!` and a `BREAKING CHANGE:` footer only for a real breaking change. Reference issue IDs only when known; use closing keywords only when the change fully resolves that issue. Do not add invented coauthors or agent attribution. Preserve required signing and trailer conventions.

See [commit templates](templates/commit-message.md). Prepare multiline messages in a file and use `git commit -F <file>` rather than shell interpolation.

## Push message rules

Git push has no custom message field. “Push message” means the user-facing status report after a push; PR titles and bodies carry the review description on the host. Do not create tags or extra commits to attach a push message.

Report the actual remote, branch, pushed commit/range, and PR link when available. Clearly distinguish success, failure, or pending work. Include validation and the next requested stage only when relevant. Never expose credentials. Do not claim merge or deployment completion before verifying it. Use [push report templates](templates/push-message.md).

## Merge (`cppm`, `cppmd`)

Check the latest PR head, required CI, reviews, and mergeability. Wait for pending checks with bounded polling; fix actionable failures and recheck the new head. Merge only after required checks/reviews pass, using repository conventions and an allowed method (squash if no convention exists). Guard the expected head when supported. Apply commit-message rules to the squash/merge message as appropriate.

Do not bypass protections, use admin overrides, or fabricate reviews. If blocked, report the PR and reason. Auto-merge is pending until verified as actually merged. If no new PR was needed, inspect the relevant existing PR; do not invent one to merge.

## Deploy (`cppmd`)

Proceed only after the relevant changes actually merged. Determine provider, command, environment, and checks from repository configuration and session context. Use the established destination; ask if none is defined or environments are ambiguous. Do not implicitly create paid infrastructure or change providers.

Deploy the merged revision. Monitor an automatic deployment triggered by merge instead of duplicating it. Verify the completed run and perform an appropriate smoke check of the deployed revision. Retry only with a concrete corrective action; follow the established rollback procedure when applicable. Report URL/run ID, revision, and observed result. If no changes or merge occurred, verify whether the requested revision is already deployed before deciding whether deployment is needed.

## Completion and blockers

Report completed stages, commit/branch, PR and merge state, checks, and deployment result as applicable. Stop at missing authentication/permissions, required reviews, substantive conflicts, or persistent failures; identify the exact incomplete stage rather than repeating unsuccessful mutations. Read-only diagnosis may continue. Never substitute a plan or a queued run for a completed action.
