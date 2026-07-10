1. `tinypulse-spa` is a monorepo. Its `main` branch is a stale bootstrap branch with NO subtree history — do NOT base the sync on it (subtree pulls fail with "was never added").
2. Read `docs/git-subtree-guide.md` within `tinypulse-spa` for the full, authoritative procedure.
3. Create a new branch named `sync-central-spa-YYYY-MM-DD` based on the **latest existing `sync-central-spa-*` branch** (that branch holds the real subtree lineage). Optionally `git merge main` to fold in main's newest commit.
4. Ensure the 7 upstream remotes exist (add + fetch if missing) per the guide's "Upstream remotes" section.
5. Sync central-spa (root) via `git merge central-spa-upstream/migrating-to-monorepo`, then resolve conflicts:
   - `package.json`: keep monorepo `name: "tinypulse-spa"`, `version: "1.0.0"`.
   - `yarn.lock`: remove any `engage-spa@git+…` / `perform-spa@git+…` entries (SPAs are workspaces, not git deps); keep real deps.
6. Sync every SPA via `git subtree pull --squash` using the prefix/branch mapping in the guide.
7. Report a summary and confirm before pushing the branch to `origin`.
