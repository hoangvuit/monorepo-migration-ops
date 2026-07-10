# monorepo-migration-ops

Claude Code workspace tooling for the TINYpulse SPA → monorepo migration.

These commands orchestrate work **across multiple sibling repos** (not any single
one), so they live at the workspace level (`~/Work/.claude`) rather than inside a
repo. Claude Code loads them because the workspace root (`~/Work`) is the working
directory.

## Layout

```
commands/
  monorepo-sync-latest-spas-code.md     # sync all SPAs into the tinypulse-spa monorepo
  sync-latest-code-to-migrating-branch.md  # merge main -> migrating-to-monorepo across SPA repos
  update-packages-for-staging-testing.md   # central-spa: refresh packages on staging
monorepo-migrating/
  repositories-recap.md                 # inventory of repos + their migrating branches
```

## Expected checkout layout

Commands assume the sibling repos are checked out next to each other under `~/Work`:

```
~/Work/
  .claude/            <- this repo
  tinypulse-spa/      <- the monorepo
  central-spa/  neptune/  engage-spa/  perform-spa/  livepulse-spa/  tps-spa/
```

Relative paths inside `repositories-recap.md` (e.g. `../../neptune`) resolve from
`~/Work`, so keep this repo at `~/Work/.claude`.

## Notes

- Machine-local Claude state (`settings.local.json`, `.claude.json`, scratch) is
  git-ignored — see `.gitignore`.
- The migration is temporary; once complete, syncing stops and this tooling can be retired.
