## Linux Command Line Basics

1. What is the Linux Command Line?

The command line (terminal/shell) is a text interface to your operating system. Instead of clicking through folders and menus, you type commands — and the OS executes them instantly. On Linux Mint, your default shell is Bash.

The terminal is not a developer tool. It IS the developer's operating system.

1. Why Does It Matter?

Every server in the world runs Linux. Every cloud instance (AWS, GCP, Azure), every Docker container, every CI/CD pipeline — Linux under the hood. If you can navigate the terminal, you can:

SSH into any remote server and feel at home
Automate repetitive tasks with scripts
Debug production issues that GUI tools can't reach
Work 10x faster than clicking through a file manager

Learning Linux commands on your Mint machine = directly transferable to production servers.

1. The 20% of Commands That Cover 80% of Real Work

Navigate the filesystem

bashpwd               # where am I right now?
ls                # list files in current directory
ls -la            # list all files (including hidden) with details
cd projects       # move into a folder
cd ..             # go up one level
cd ~              # go to home directory (/home/yourname)
cd -              # go back to previous directory

Work with files and folders

bashmkdir notes           # create a folder
mkdir -p a/b/c        # create nested folders in one shot
touch file.txt        # create an empty file
cp file.txt backup.txt        # copy a file
cp -r folder/ backup_folder/  # copy a folder (recursive)
mv file.txt docs/file.txt     # move a file
mv old_name.txt new_name.txt  # rename a file
rm file.txt           # delete a file
rm -rf folder/        # delete a folder and everything inside (careful!)

Read file contents

bashcat file.txt          # print entire file to terminal
less file.txt         # scroll through file (q to quit)
head -n 20 file.txt   # first 20 lines
tail -n 20 file.txt   # last 20 lines
tail -f app.log       # live-follow a log file (Ctrl+C to stop)

Find things

bashfind . -name "*.txt"              # find all .txt files from here
find /home -name "config.json"    # find a specific file
grep "error" app.log              # search for "error" inside a file
grep -r "TODO" ./projects/        # search recursively in a folder
grep -i "error" app.log           # case-insensitive search

System and process info

bashwhoami            # your username
uname -a          # OS and kernel info
df -h             # disk space usage (human readable)
free -h           # RAM usage
top               # live process monitor (q to quit)
htop              # better top — install with: sudo apt install htop
ps aux            # list all running processes
kill 1234         # kill process by PID

Package management on Linux Mint (apt)

bashsudo apt update               # refresh package list
sudo apt upgrade              # upgrade all installed packages
sudo apt install htop         # install a package
sudo apt remove htop          # uninstall a package
sudo apt autoremove           # clean up unused packages

1. Real-Life Mental Model

TaskCommand Pattern"Where am I?"pwd"What's in this folder?"ls -la"Find a file I lost"find . -name "filename""Search text inside files"grep -r "keyword" ./folder"Watch logs in real time"tail -f logfile.log"Check disk/RAM"df -h / free -h"Install something"sudo apt install packagename"Kill a frozen process"ps aux | grep appname → kill PID

3 shortcuts that save the most time:

bashTab        # autocomplete file/folder names — use this constantly
Ctrl+C     # cancel a running command
Ctrl+L     # clear the terminal (same as: clear)
!!         # re-run the last command
sudo !!    # re-run last command as sudo (forgot sudo? this fixes it)

Key Takeaway

The Linux terminal looks intimidating but has a simple logic: verb + target + options. ls -la /home = list (ls) the /home directory (target) with all details (-la). Learn the 20 commands above and you can navigate, manage files, search, monitor, and install anything on any Linux machine — including production servers.
