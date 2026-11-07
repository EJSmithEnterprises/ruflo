# Disabled workflows

This fork (`EJSmithEnterprises/ruflo`) is kept as a pure pass-through mirror
of `ruvnet/ruflo`, synced forward with a plain fast-forward on a schedule.
GitHub only treats files directly under `.github/workflows/` as active
workflows, so moving a file here is enough to disable it — no file content
was changed.

The workflows below were moved out because each one commits/pushes to
`main` on its own (via a schedule, a push trigger, or both), which puts
commits on this fork that don't exist upstream. That breaks the
fast-forward assumption the sync relies on and forces a manual
reconciliation every time one of them fires.

| File | Why it's here |
|---|---|
| `clone-tracker.yml` | Runs on a 13-day cron, commits a clone-count snapshot to `main`. Tracks *this fork's* clone count (not upstream's), which this fork has no use for. Already caused two unplanned drift commits (2026-10-01, 2026-10-14) before being disabled. |
| `status-badges.yml` | Runs daily (cron) and on every push to `main`; commits README badge updates back to `main`. |
| `rollback-manager.yml` | Runs automatically on every push to `main` and on completion of CI pipelines (not just manual `workflow_dispatch`); can auto-commit and push a "rollback" to `main` based on failure-detection heuristics that may misfire in a fork environment missing upstream's secrets/targets. |

If this fork ever needs one of these for real, move the file back under
`.github/workflows/` and review whether its trigger conditions still make
sense running against this fork specifically (as opposed to upstream).
