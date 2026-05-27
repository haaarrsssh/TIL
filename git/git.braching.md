What is git reflog?
git reflog is a log of every single thing your HEAD has pointed to — every commit, reset, merge, rebase, checkout. Everything.
It's Git's undo history for your undo history.

If you ever think "I just lost my work forever" — git reflog is probably your way out.

git checkout -b feature/name     → create feature branch
git rebase main                  → rebase before merging
git merge feature/name           → merge with fast-forward
git merge --no-ff feature/name   → merge with commit
git branch -d feature/name       → delete merged branch
git branch -D feature/name       → force delete branch
git fetch origin                 → get latest remote state

# See full reflog

git reflog

# See reflog for a specific branch

git reflog show feature/login

# See reflog with timestamps

git reflog --date=iso

# Restore to a specific reflog entry

git reset --hard HEAD@{3}

# Create a branch from a reflog entry

git checkout -b recovered-branch HEAD@{5}

# You accidentally ran this

git reset --hard HEAD~2

# Oh no — 2 commits are gone

# Step 1 — Open reflog

git reflog

# e4f5g6h HEAD@{1}: commit: feat: add payment form  ← this is what you lost

# Step 2 — Restore to that point

git reset --hard e4f5g6h

# Your commits are back ✅
