What is git cherry-pick?
git cherry-pick lets you pick a specific commit from one branch and apply it to another — without merging the entire branch.
Think of it as "I want that one commit, not everything else."

The Problem It Solves
You're working on a feature branch and you made a bug fix there.
But main also needs that fix — right now.
You don't want to merge the whole feature branch (it's not ready).
→ Just cherry-pick that one fix commit onto main.

# Step 1 — Find the commit you want

git log --oneline feature-branch

# a1b2c3d fix: navbar crash on mobile   ← you want this one

# e4f5g6h half done feature

# x7y8z9w another incomplete change

# Step 2 — Switch to the branch you want to apply it to

git checkout main

# Step 3 — Cherry-pick that commit

git cherry-pick a1b2c3d

# Step 4 — Push

git push

Visual: What Happens
Before:
main:    A → B → C
feature: A → B → C → D (bug fix) → E → F

After git cherry-pick D onto main:
main:    A → B → C → D'
feature: A → B → C → D → E → F

Handling Conflicts
Sometimes cherry-pick hits a conflict (just like merge/rebase):
bashgit cherry-pick a1b2c3d

# CONFLICT! Git stops and tells you

# Step 1 — Fix the conflict in the file

# Step 2 — Stage the fixed file

git add <filename>

# Step 3 — Continue the cherry-pick

git cherry-pick --continue

# OR — if you want to cancel entirely

git cherry-pick --abort

When to Use Cherry-Pick
SituationUse cherry-pick?Bug fix on feature branch needed on main✅ YesHotfix from one release branch to another✅ YesYou want to bring ONE specific commit, not the whole branch✅ YesYou want to merge an entire completed feature❌ Use git merge insteadYou want to sync two branches fully❌ Use git rebase instead
