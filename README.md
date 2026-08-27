# Renovate Automerge Schedule Reproducer

This repository is a manual reproducer for renovatebot/renovate#33770.

The checked-in configuration has a closed `schedule`, an open
`automergeSchedule`, and `updateNotScheduled: false`. This is the state that
previously prevented an existing PR from reaching Renovate's PR automerge
logic.

## Reproduction

1. Push this repository to a disposable GitHub repository and enable
   Renovate.
2. Temporarily change `schedule` to `["at any time"]`, then wait for Renovate
   to open the lodash update PR. Do not merge it.
3. Restore `renovate.json` from this repository so `schedule` is closed again.
4. Run the patched Renovate checkout against the repository with
   `RENOVATE_DRY_RUN=full` and debug logging enabled.

Expected behavior with the fix: Renovate reaches `checkAutoMerge` while the
normal schedule is closed. In dry-run mode, a merge-ready PR produces the
"Would merge PR" log instead of returning `update-not-scheduled` first.

The old behavior returns `update-not-scheduled` without attempting PR
automerge.
