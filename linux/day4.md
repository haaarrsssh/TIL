## Processes, Cron Jobs & Background Tasks

1. What Are They?

A process is any running program on your system — every terminal command, app, or service is a process with a unique PID (Process ID).
A cron job is a scheduled task — you tell Linux when to run a command, and it runs automatically forever.
Background tasks let you run long commands without blocking your terminal.

Cron is your robot assistant. You write the schedule once — it executes forever.

1. Why Does It Matter?

Manual = human error + forgotten steps. Automation = reliability. Real-world use:

Run database backups every night at 2am
Clear temp files every Sunday
Fetch an API and log results every hour
Restart a crashed service automatically

Cron is used in every production Linux server on the planet. Background tasks are essential when running servers, scrapers, or long scripts locally on your Mint machine.

1. The 20% That Covers 80% of Real Work

Managing processes

bashps aux                  # list all running processes
ps aux | grep python    # find a specific process
top                     # live process monitor
htop                    # better live monitor (sudo apt install htop)

kill 1234               # send TERM signal — ask process to stop
kill -9 1234            # send KILL signal — force stop immediately
pkill python            # kill all processes named "python"

# Find PID of a process by name

pgrep python            # prints PID(s)

Background tasks — run without blocking terminal

bash# Run a command in the background with &
python3 server.py &

# Output: [1] 4523   ← job number + PID

# List background jobs

jobs

# Bring background job to foreground

fg %1           # %1 = job number 1

# Send a running foreground job to background

# Press Ctrl+Z to pause it, then

bg %1           # resume it in the background

# nohup — keep running even after terminal closes

nohup python3 server.py &

# Output goes to nohup.out by default

# Redirect output

nohup ./script.sh > logs/output.log 2>&1 &

# 2>&1 = redirect stderr to same place as stdout

Cron jobs — scheduled automation

bash# Open your personal cron table
crontab -e      # opens in nano/vim — edit your jobs
crontab -l      # list current cron jobs
crontab -r      # remove ALL your cron jobs (careful!)

The cron schedule syntax:

* * * * * command
│ │ │ │ │
│ │ │ │ └── Day of week (0=Sun, 6=Sat)
│ │ │ └──── Month (1–12)
│ │ └────── Day of month (1–31)
│ └──────── Hour (0–23)
└────────── Minute (0–59)

Real cron examples:

bash# Run every minute

* * * * * /home/harsh/scripts/check.sh

# Every day at 2:30 AM — perfect for backups

30 2 ** * /home/harsh/scripts/daily_backup.sh

# Every Sunday at midnight

0 0 ** 0 /home/harsh/scripts/weekly_cleanup.sh

# Every hour

0 ** ** /home/harsh/scripts/fetch_data.sh

# Every 15 minutes

*/15* ** * /home/harsh/scripts/ping.sh

# Weekdays (Mon–Fri) at 9 AM

0 9 ** 1-5 /home/harsh/scripts/morning_report.sh

Cron best practices:

bash# Always use absolute paths in cron — cron has no $PATH

# ❌ Bad

* * * * * python3 script.py

# ✅ Good

* * * * * /usr/bin/python3 /home/harsh/scripts/script.py

# Log cron output so you can debug it

30 2 ** * /home/harsh/scripts/backup.sh >> /home/harsh/logs/backup.log 2>&1

# Find the absolute path of a command

which python3     # → /usr/bin/python3
which bash        # → /bin/bash

Automate Day 23's backup script with cron

bash# Step 1 — make sure script has absolute paths and is executable
chmod +x /home/harsh/scripts/daily_backup.sh

# Step 2 — open crontab

crontab -e

# Step 3 — add this line (runs every night at 2 AM)

0 2 ** * /home/harsh/scripts/daily_backup.sh >> /home/harsh/logs/backup.log 2>&1

# Step 4 — verify it's saved

crontab -l

1. Real-Life Mental Model

ToolWhat It DoesWhen to Useps auxSnapshot of all running processesDebugging what's runningkill -9Force-stop a process by PIDWhen a process is frozencommand &Run in background, terminal stays freeStart a server locallynohupRun in background, survives terminal closeLong-running scripts on a servercrontab -eSchedule a command to run automaticallyDaily/hourly/weekly automation

Cron time cheat sheet:

@reboot     → run once at startup
@daily      → same as: 0 0 ** *
@weekly     → same as: 0 0* *0
@monthly    → same as: 0 0 1* *
@hourly     → same as: 0* ** *

bash# Use @reboot to start a script when Mint boots
@reboot /home/harsh/scripts/start_server.sh

Key Takeaway

Processes are just running programs — learn to find, monitor, and stop them. Cron is the Linux alarm clock — write a schedule once, automate forever. The two gotchas: always use absolute paths in cron, and always log output so you know if it worked. Together, background tasks + cron = a personal automation layer running silently on your Mint machine 24/7.
