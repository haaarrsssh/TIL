What is .gitignore?
.gitignore is a file that tells Git which files and folders to never track.
You place it in the root of your repo and Git will completely ignore everything listed in it.

Why You Need It
Some files should never go into your repo:

Secret keys and passwords (API_KEY=abc123)
Dependencies folder (node_modules/ — can be 100k+ files)
Build output (dist/, build/)
OS junk files (.DS_Store on Mac, Thumbs.db on Windows)
Editor settings (.vscode/, .idea/)
Log files (*.log)

Creating a .gitignore
Just create a file named exactly .gitignore in your project root:
bashtouch .gitignore

Pattern Syntax (The Important Ones)
Ignore a specific file
secrets.txt
.env
Ignore all files of a type
*.log
*.zip
*.env
Ignore a folder
node_modules/
dist/
build/

Real .gitignore Examples
Node.js Project
node_modules/
dist/
build/
.env
.env.local
*.log
npm-debug.log*
.DS_Store

Python Project
__pycache__/
*.py[cod]
*.egg-info/
.env
venv/
.venv/
dist/
build/
*.log
.DS_Store

General (any project)
.env
.env.*
*.log
*.tmp
.DS_Store
Thumbs.db
.vscode/
.idea/

The ! Exception Pattern

# Ignore all .env files

.env*

# But keep the example file (so teammates know what's needed)

!.env.example
This is a very common real-world pattern — ignore all secrets but commit the template.

What If You Already Committed a File by Mistake?
.gitignore only works on untracked files. If a file is already tracked, adding it to .gitignore won't stop Git from tracking it.
Fix:# Remove from tracking but keep the file on disk
git rm --cached filename

# For a folder

git rm --cached -r foldername/

# Then commit the removal

git add .gitignore
git commit -m "chore: remove secrets from tracking"

Global .gitignore (For Your Whole Machine)
You can set up a global .gitignore for OS/editor files you never want tracked in ANY project:
bash# Create global gitignore
touch ~/.gitignore_global

# Tell Git to use it

git config --global core.excludesfile ~/.gitignore_global

Use gitignore.io
Don't write it from scratch — generate it:
gitignore.io
Type your stack (e.g. Node, Python, macOS, VSCode) and it generates a complete .gitignore for you.

Quick Reference Card
filename.txt          → ignore specific file
*.log                 → ignore all files of this type
folder/               → ignore entire folder
/file.txt             → ignore only in root
**/file.txt           → ignore in any subfolder
!important.txt        → exception — do NOT ignore this
git rm --cached file  → untrack an already tracked file

Key Takeaway

Always set up .gitignore before your first commit.
Once secrets or node_modules are in Git history, cleaning them out is painful.
When in doubt → use gitignore.io to generate one.

🎉 Git 80/20 Series — Complete!
DayTopicDay 1git rebaseDay 2git stashDay 3git reset vs git revertDay 4git cherry-pickDay 5Branching StrategyDay 6git reflogDay 7.gitignore patterns

You now know the 20% of Git that covers 80% of real daily work.
