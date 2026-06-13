## 1. What Are File Permissions?

Every file and folder on Linux has permissions that control who can read it, write to it, or execute it — and for whom. Linux splits "who" into three groups: the owner, the group, and everyone else.

Linux permissions = a lock on every file. You decide who gets the key.

1. Why Does It Matter?

Permissions are the #1 thing that breaks on a Linux server:

Script won't run → missing execute permission
Can't edit a config file → don't own it
Web server can't read your files → wrong group permissions
Security breach → files were world-writable

Every developer who deploys to a server, runs a cron job, or sets up a web app will hit permissions. Understanding them saves hours of debugging.

1. The 20% That Covers 80% of Real Work

Reading permissions — decode ls -la

bash$ ls -la
-rwxr-xr-- 1 harsh developers 4096 Jun 13 10:00 script.sh
drwxr-xr-x 2 harsh harsh      4096 Jun 13 09:00 projects/

- rwx r-x r--
│ │││ │││ │││
│ │││ │││ └── others:  r-- = read only
│ │││ └────── group:   r-x = read + execute
│ └────────── owner:   rwx = read + write + execute
└──────────── type: - = file, d = directory, l = symlink

SymbolMeaningNumericrread4wwrite2xexecute1-none0

chmod — change permissions

bash# Numeric mode (most common in practice)
chmod 755 script.sh     # owner=rwx, group=r-x, others=r-x
chmod 644 config.txt    # owner=rw-, group=r--, others=r--
chmod 600 secret.key    # owner=rw-, group=---, others=--- (private!)
chmod 777 file.txt      # everyone can do everything (avoid in prod)

# Symbolic mode (more readable)

chmod +x script.sh      # add execute for everyone
chmod u+x script.sh     # add execute for owner only
chmod o-r file.txt      # remove read from others
chmod g+w file.txt      # add write for group

The 3 permission numbers you'll use 90% of the time:

bashchmod 755   # scripts, binaries, public directories
chmod 644   # config files, web assets, documents
chmod 600   # private keys, secrets, .env files

chown — change owner

bashchown harsh file.txt              # change owner to harsh
chown harsh:developers file.txt   # change owner + group
chown -R harsh:harsh ./projects/  # recursive — apply to all files inside
sudo chown root:root /etc/config  # system files need sudo

sudo — run as superuser

bashsudo command              # run one command as root
sudo -i                   # switch to root shell (careful)
sudo su - username        # switch to another user
whoami                    # confirm who you're running as

# On Linux Mint — add user to sudoers group

sudo usermod -aG sudo username

User and group management

bash# Users
adduser harsh             # create a new user (interactive)
passwd harsh              # change a user's password
deluser harsh             # delete a user

# Groups

groups harsh              # list groups harsh belongs to
sudo groupadd developers  # create a new group
sudo usermod -aG developers harsh   # add harsh to developers group
id harsh                  # show user ID, group ID, all groups

Real-World Fix Patterns

bash# Script won't run — "Permission denied"
chmod +x myscript.sh
./myscript.sh

# Can't edit a system file

sudo nano /etc/hosts

# Web server (nginx/apache) can't read your files

sudo chown -R www-data:www-data /var/www/myapp/
sudo chmod -R 755 /var/www/myapp/

# .env / private key should never be readable by others

chmod 600 .env
chmod 600 ~/.ssh/id_rsa

1. Real-Life Mental Model

NumberWho can do whatUse for777Everyone: read+write+execNever in production755Owner: all; others: read+execScripts, public dirs644Owner: read+write; others: readConfig files, web files600Owner only: read+writeSSH keys, .env, secrets400Owner only: readRead-only secrets

The mental shortcut for numbers:

r=4, w=2, x=1 → add them up per group
rwx = 4+2+1 = 7
r-x = 4+0+1 = 5
r-- = 4+0+0 = 4
--- = 0+0+0 = 0

chmod 755 = owner(7=rwx) group(5=r-x) others(5=r-x)

Key Takeaway

Linux permissions follow one rule: every file has three groups (owner, group, others) and three actions (read, write, execute). The numbers 7, 5, 4, 6, 0 cover 90% of real cases. When something says "Permission denied" — check ls -la, find what's wrong, fix with chmod or chown. That's the entire debugging loop.
