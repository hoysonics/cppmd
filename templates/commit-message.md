# Commit message templates

Replace placeholders; omit sections that add no value.

## Small change

```text
<type>(<scope>): <imperative summary>
```

Example:

```text
fix(auth): preserve the repository's existing Git account
```

## Change requiring context

```text
<type>(<scope>): <imperative summary>

<Why this change is needed and the resulting behavior.>

Validation: <checks run and observed results>

Refs: <known issue, if relevant>
```

## Breaking change

```text
feat(<scope>)!: <imperative summary>

<Motivation and migration guidance.>

BREAKING CHANGE: <incompatible behavior and required migration>
```
