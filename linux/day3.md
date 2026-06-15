## What is Bash Scripting?

A Bash script is a plain text file containing a sequence of terminal commands that run together as a single program. Instead of typing the same 10 commands every time, you write them once, make the file executable, and run it with one command.

A bash script is just your terminal commands — saved, automated, and repeatable.

1. Why Does It Matter?

Every developer eventually has tasks they repeat daily:

Pull latest code, run tests, restart a server
Backup a folder at midnight
Rename 500 files in a pattern
Set up a fresh Linux machine from scratch

Doing these manually = wasted time + human error. A bash script does it in one shot, every time, perfectly. It's the foundation of DevOps, CI/CD pipelines, and Linux automation.

1. The 20% That Covers 80% of Real Scripting

The anatomy of a script

bash#!/bin/bash

# This line (shebang) tells Linux: run this with bash

echo "Hello, Harsh!"   # print to terminal

bash# Save as hello.sh, then:
chmod +x hello.sh    # make it executable
./hello.sh           # run it

Variables

bash#!/bin/bash

name="Harsh"
day=23

echo "Day $day — written by $name"
echo "Home directory: $HOME"      # built-in variable
echo "Script name: $0"            # $0 = the script itself
echo "First argument: $1"         # $1 = first arg passed in

bash./script.sh Linux    # $1 = "Linux"

User input

bash#!/bin/bash

echo "Enter your name:"
read username
echo "Welcome, $username!"

if / else — conditionals

bash#!/bin/bash

age=20

if [ $age -ge 18 ]; then
  echo "Adult"
elif [ $age -ge 13 ]; then
  echo "Teenager"
else
  echo "Child"
fi

bash# Common comparison operators
-eq   # equal
-ne   # not equal
-gt   # greater than
-lt   # less than
-ge   # greater than or equal
-le   # less than or equal
-z    # string is empty
-f    # file exists
-d    # directory exists

Loops

bash#!/bin/bash

# for loop — iterate over a list

for name in Harsh Riya Arjun; do
  echo "Hello, $name!"
done

# for loop — iterate over a range

for i in {1..5}; do
  echo "Day $i"
done

# while loop

count=1
while [ $count -le 5 ]; do
  echo "Count: $count"
  count=$((count + 1))
done

Functions

bash#!/bin/bash

greet() {
  echo "Hello, $1!"         # $1 = first argument to the function
}

backup() {
  local src=$1
  local dest=$2
  cp -r "$src" "$dest"
  echo "Backed up $src → $dest"
}

greet "Harsh"
backup ./projects ./projects_backup

A real script — daily backup automation

bash#!/bin/bash

# daily_backup.sh — backs up a folder with a timestamp

SOURCE="$HOME/projects"
DEST="$HOME/backups"
DATE=$(date +%Y-%m-%d)
BACKUP_NAME="projects_$DATE"

mkdir -p "$DEST"

cp -r "$SOURCE" "$DEST/$BACKUP_NAME"

echo "✓ Backup complete: $DEST/$BACKUP_NAME"

bashchmod +x daily_backup.sh
./daily_backup.sh

# Output: ✓ Backup complete: /home/harsh/backups/projects_2024-06-14

1. Real-Life Mental Model

ConceptWhat It Maps To#!/bin/bash"Run this file with bash"VariablesStore values, reuse with $varname$1, $2Arguments passed when running the scriptif [ ]Decision point — spaces inside [ ] matterfor loopRepeat for each item in a listwhile loopRepeat until a condition is falseFunctionsNamed reusable blocks, like Python def$(command)Run a command and capture its output

The 3 rules that trip everyone up:

bash# Rule 1: spaces inside [ ] are required
if [ $x -eq 5 ]    # ✅ correct
if [$x -eq 5]      # ❌ breaks

# Rule 2: no spaces around = in variable assignment

name="Harsh"       # ✅ correct
name = "Harsh"     # ❌ breaks

# Rule 3: use $(command) to capture command output

today=$(date +%Y-%m-%d)    # ✅ captures today's date
today=`date +%Y-%m-%d`     # works but old-style, avoid

Key Takeaway

Bash scripts follow a simple structure: shebang → variables → logic (if/loops/functions) → commands. The moment you find yourself repeating more than 3 terminal commands regularly, that's a script waiting to be written. Start small — automate one thing today on your Mint machine.
ate
