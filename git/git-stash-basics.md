# The Problem It Solves

You're working on a feature, your code is half-done (not ready to commit), and suddenly:

Your team asks you to fix a bug on another branch
You need to pull latest changes but have conflicts
You want to test something on a clean state

## common commands

git stash              → save changes
git stash list         → see all stashes
git stash pop          → restore + delete latest stash
git stash apply        → restore, keep stash
git stash drop         → delete a stash
git stash clear        → delete all stashes
git stash -u           → include untracked files
git stash push -m ""   → save with a name

## real world example

You're mid-feature, changes not ready to commit
git stash                   # save changes, clean working dir

git checkout bugfix-branch  # switch to fix a bug

# fix the bug

git commit -m "fix: navbar crash"

git checkout feature-branch # come back
git stash pop               # restore your half-done work

continue where you left off.
