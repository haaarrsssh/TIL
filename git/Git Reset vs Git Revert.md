git reset

# You made a bad commit

git log --oneline

# e5f6g7h bad commit with wrong code

# a1b2c3d previous good commit

# Undo the last commit, keep the changes staged

git reset --soft HEAD~1

# Or undo and unstage the changes

git reset --mixed HEAD~1

# Or nuke it completely

git reset --hard HEAD~1

Undoes the commit
Deletes the changes completely ← dangerous
Use when: you want to completely throw away recent work

git revert

# You pushed a bug to main and need to undo it safely

git log --oneline

# e5f6g7h introduced a bug

# a1b2c3d last good commit

git revert e5f6g7h

# Git opens editor for commit message, save and close

# A new revert commit is created and pushed safely

git push

When to Use Which?
SituationUseBad commit, not pushed yetgit resetBad commit, already pushed to shared branchgit revertWorking alone on a personal branchgit reset is fineWorking on main or team branchAlways git revertWant to completely discard local messgit reset --hard

The Golden Rule

If the commit is already pushed and others may have pulled it →
Always use git revert, never git reset

Resetting pushed commits rewrites history and causes serious problems for teammates.

Common Mistakes
MistakeConsequencegit reset --hard without thinkingYour changes are permanently gonegit reset on a pushed branchTeammates' history breaks, force push neededForgetting git revert creates a commitNothing wrong, just know it adds to history

Key Takeaway

git reset = rewrite history (use locally, carefully)
git revert = undo safely (use on shared/pushed branches)
When in doubt → use git revert
