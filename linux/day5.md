## Environment Variables & .bashrc

1. What Are They?

Environment variables are named values stored in your shell session that programs can read. They configure how your system and apps behave — paths, secrets, settings.
.bashrc is a script that runs every time you open a new terminal. It's where you make your environment variables, aliases, and customizations permanent.

Environment variables = global settings for your shell. .bashrc = where those settings live forever.

1. Why Does It Matter?

Every serious project uses environment variables:

DATABASE_URL, API_KEY, SECRET_KEY — never hardcode these in your code
PATH — how Linux knows where to find commands like python3 or git
NODE_ENV=production — tells your app which mode to run in

On your Mint machine, .bashrc is also where you set up aliases (ll instead of ls -la), customize your prompt, and add new tools to your PATH. Every developer personalizes this file — it's your shell's config file.

1. The 20% That Covers 80% of Real Work

Reading environment variables

bashprintenv              # print all environment variables
printenv PATH         # print a specific one
echo $HOME            # → /home/harsh
echo $USER            # → harsh
echo $PATH            # → /usr/local/bin:/usr/bin:/bin:...
echo $SHELL           # → /bin/bash

Setting variables — temporary vs permanent

bash# Temporary — only in current terminal session
export API_KEY="abc123"
echo $API_KEY         # works here

# Opens new terminal → API_KEY is gone

# Permanent — add to ~/.bashrc

echo 'export API_KEY="abc123"' >> ~/.bashrc
source ~/.bashrc      # reload without restarting terminal
echo $API_KEY         # works in every terminal from now on

The .bashrc file — your shell's home base

bash# Open it
nano ~/.bashrc

# or

code ~/.bashrc        # if VS Code is installed

What a well-structured .bashrc looks like:

bash# ~/.bashrc

# ── Environment Variables ──────────────────────────

export EDITOR="nano"
export JAVA_HOME="/usr/lib/jvm/java-17-openjdk-amd64"
export PATH="$HOME/.local/bin:$PATH"   # prepend custom bin to PATH

# ── Aliases ───────────────────────────────────────

alias ll='ls -la'
alias gs='git status'
alias ga='git add .'
alias gc='git commit -m'
alias gp='git push'
alias ..='cd ..'
alias ...='cd ../..'
alias update='sudo apt update && sudo apt upgrade -y'

# ── Custom prompt (optional) ──────────────────────

PS1='\[\e[32m\]\u@\h\[\e[0m\]:\[\e[34m\]\w\[\e[0m\]\$ '

# Shows: harsh@mint:~/projects$  (green user, blue path)

bash# After editing .bashrc, always reload:
source ~/.bashrc

PATH — how Linux finds commands

bashecho $PATH

# → /usr/local/bin:/usr/bin:/bin:/usr/local/sbin:/usr/sbin

# When you type "python3", Linux searches each directory left to right

# First match wins

# Add a new directory to PATH (in .bashrc)

export PATH="$HOME/scripts:$PATH"

# Now scripts in ~/scripts/ run from anywhere without ./

.env files — keep secrets out of code

bash# .env file in your project root
DATABASE_URL="postgresql://localhost/mydb"
API_KEY="super_secret_key_123"
DEBUG="false"

# Load it in a bash script

source .env
echo $DATABASE_URL

# ALWAYS add .env to .gitignore — never commit secrets

echo ".env" >> .gitignore

Key files to know

bash~/.bashrc          # runs on every interactive non-login shell (new terminal tab)
~/.bash_profile    # runs on login shell (SSH login, first terminal on boot)
~/.bash_aliases    # optional — keep aliases separate, sourced from .bashrc
/etc/environment   # system-wide variables — applies to ALL users
/etc/profile       # system-wide, runs on login for all users

On Linux Mint: .bashrc is the one file you'll actually edit. It covers 99% of personal customization.

1. Real-Life Mental Model

ConceptReal Equivalentexport VAR=valueSetting a global app preference$PATHThe list of folders Linux checks for commands.bashrcYour shell's startup config — runs on every terminal open.env fileA secrets locker for your projectsource ~/.bashrc"Reload config without restarting"alias ll='ls -la'A keyboard shortcut for a longer command

The workflow for any new tool you install:

bash# 1. Find where it installed
which newtool         # → /home/harsh/.local/bin/newtool

# 2. If not found, add its directory to PATH in .bashrc

export PATH="$HOME/.local/bin:$PATH"

# 3. Reload

source ~/.bashrc

# 4. Confirm

which newtool         # → /home/harsh/.local/bin/newtool ✓

Key Takeaway

Environment variables are how Linux and your programs talk about configuration without hardcoding values. .bashrc is the file that makes everything permanent — aliases, PATH additions, secrets, prompt customization. The one rule: never commit secrets to git — use .env files and .gitignore. Everything else in .bashrc is fair game to customize.
