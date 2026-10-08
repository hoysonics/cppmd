# Push status templates

These are reports, not arguments to `git push`. Replace placeholders with observed facts and omit unused lines.

## Successful push and PR

```text
Pushed <short SHA or range> to <remote>/<branch>.
PR: <URL>
Validation: <checks and results>
```

## Merged

```text
Pushed <short SHA> to <remote>/<branch> and merged PR <URL>.
Merge commit: <SHA>
Validation: <checks and results>
```

## Deployed

```text
Merged PR <URL> and deployed <merged revision> to <environment>.
Deployment: <URL or run ID>
Verification: <observed smoke-check result>
```

## Blocked or failed

```text
Completed: <verified stages, with branch/commit/PR as applicable>.
<Stage> blocked: <specific reason>.
Needed: <missing login, decision, review, or corrective action>.
```

If push itself failed, say “Push failed” and never use a successful-push template. If auto-merge or deployment is queued, report “pending,” not “merged” or “deployed.”
