0. Use repo `tinypulse`
1. Ensure main branch is up-to-date and green
   ○ git checkout main
   ○ git pull origin main
2. Create a release branch from the main branch, follow naming convention
   release-<name>-<time> (e.g. release-security-issue-2026-03-26-17-45)
   ○ git switch main
   ○ git checkout -b release-<name>-<time>
   ○ git push -u origin release-<name>-<time>
