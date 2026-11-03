1. `tinypulse-spa` is a monorepo. Never base a sync branch on `main`: MRs into `main` are squash-merged, so it has NO subtree history (subtree pulls fail with "was never added").
2. Read `docs/git-subtree-guide.md` within `tinypulse-spa` for the full, authoritative procedure (and `CLAUDE.md` for the sync rules).
3. Refresh upstream migration branches first. For every repo in `repositories-recap.md` that has a `migrating-to-monorepo` branch (engage-spa, perform-spa, and central-spa), merge its `origin/main` into that branch and push it. Resolve conflicts deliberately and never commit `yarn.lock` with conflict markers (check `grep -c '^<<<<<<<' yarn.lock` is 0 before committing).
4. Create a new branch named `sync-spas-YYYY-MM-DD` based on the **latest `sync-base-*` tag** (`git tag -l 'sync-base-*' --sort=-creatordate | head -1`), not on a previous sync branch (those are deleted when their MR merges). Then `git fetch origin` and `git merge origin/main`.
5. Ensure the 7 upstream remotes exist (add + fetch if missing) per the guide's "Upstream remotes" section.
6. Sync central-spa (root) via `git merge central-spa-upstream/migrating-to-monorepo`. The monorepo root was ported by hand, so expect conflicts in `.yarnrc`, `Jenkinsfile`, `package.json` and `yarn.lock`. Resolve by:
   - Keeping the monorepo side (`git checkout --ours`) for `.yarnrc`, `Jenkinsfile`, `package.json` and `yarn.lock`.
   - Re-applying only upstream's real dependency changes to `package.json` (`dependencies` and `resolutions`, e.g. new packages or version bumps such as `@tinypulse/tinykit`). Keep `name: "tinypulse-spa"`, the `apps/*` workspaces and the `setup` script.
   - Regenerating `yarn.lock` with `yarn install` after the subtree pulls. Verify `grep -c 'spa@git+' yarn.lock` is `0`.
7. Sync every SPA with `./scripts/sync-all-spas.sh` (`git subtree pull --squash` using the guide's prefix/branch mapping). If it reports the bad-squash glitch, run `git reset --hard HEAD^1` and re-run.
8. Run `yarn install`, then bump the **patch** version in the root `package.json` (e.g. `1.0.1` -> `1.0.2`) and commit it together with the regenerated `yarn.lock`. Diff `dependencies` against `resolutions` so every package listed in both agrees.
9. Report a summary and confirm before pushing the branch to `origin`. Open the MR into `main` and assign it to the user.
10. After the MR is merged, tag the sync branch tip: `git tag -a sync-base-$(date +%F) -m "monorepo sync state merged via MR !<n>" <branch-tip>` and `git push origin sync-base-$(date +%F)`.
