# Grouping plan template

Show this before committing when the changes span more than one group. Replace placeholders with observed facts; use the repository's usual language for the prose.

```text
<mode> / Grouping plan

| Label | Group        | Files | Depends on | Planned commit / PR title            |
|-------|--------------|-------|------------|--------------------------------------|
| A     | <group-name> | <n>   | -          | <type>(<scope>): <imperative summary> |
| B     | <group-name> | <n>   | A          | <type>(<scope>): <imperative summary> |

Recommended: <option> (<one-line reason>)

Options:
1) Single PR, one commit     All <total> files in one commit
2) Single PR, grouped commits One commit per group (A → B), one PR
3) Split PRs                 One PR per group; stacked where dependent (A → B)
4) Partial                   Only some groups, e.g. "4 A" or "4 A,B 3"
5) Dry-run                   Print the plan only; change nothing
6) Abort                     Stop; change nothing
e) Edit                      Change groups, branches, or titles

<Note any group that still mixes concerns and how it could be split.>
Choose a number or e.
```

List each group's files when asked, or when there are few enough to read at a glance:

```text
A <group-name>: <path>, <path>
B <group-name>: <path>
```
