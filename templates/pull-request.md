# Pull request template

Use the repository's own PR template when one exists. Otherwise, replace placeholders and omit optional sections that add no value. Describe the final state of the branch, not the history of attempts.

## Title

```text
<type>(<scope>): <imperative summary>
```

Follow the commit-message conventions. For a single-commit PR, reuse the commit subject. Keep it under about 72 characters, and add `!` for breaking changes.

## Body

```markdown
## Summary

<Concrete problem and resulting behavior, in one to three sentences.>

Closes #<issue>  <!-- or Refs #<issue>; omit when there is no known issue -->

## Changes

- <Meaningful final change, grouped by area when there are several>

## Validation

- `<command or check>`: <observed result>
- <Manual check and what was observed, or a clearly stated limitation such as "not run: requires production credentials">

## Screenshots

<Before/after for user-visible UI changes; omit otherwise.>

## Risks or migration

<Material risks, breaking changes, required config/env/schema changes, and migration steps; omit when unnecessary.>

## Deployment and rollback

<Deploy-order constraints, feature flags, and how to roll back; omit when a normal deploy and revert suffice.>

## Reviewer notes

<Where to focus, intentional non-changes, follow-up work, or open questions; omit when unnecessary.>
```

## Rules

- Report only validation that actually ran, with its real result. Never imply that checks passed when they were skipped or are still pending.
- Link issues only when they are known. Do not invent issue numbers.
- Do not include secrets, tokens, internal hostnames, or customer data.
- When updating an existing PR, rewrite the body to match the current branch, not appending a change log, and keep any human-written sections intact.
- Add the attribution lines required by the host or user instructions at the end, when present.
