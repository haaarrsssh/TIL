## Linux Networking: SSH, curl & Ports

1. What is it?

Linux networking commands let your machine talk to other machines — connecting to remote servers (ssh), fetching data from APIs/websites (curl), and inspecting what's listening on your own machine (ports).

If Linux commands are how you talk to your own machine, networking commands are how you talk to every other machine.

1. Why Does It Matter?

Almost no real dev work happens on one isolated machine:

Deploying code → SSH into a remote server
Testing an API → curl instead of opening a browser
Debugging "why won't my app connect" → checking which ports are open/in use
Working with cloud servers (AWS EC2, DigitalOcean, etc.) → SSH is the only way in

This is the bridge between "I can use my own Linux machine" and "I can work with servers in the real world."

1. The 20% That Covers 80% of Real Work

SSH — connect to a remote machine

bashssh username@server_ip
ssh harsh@192.168.1.10
ssh <root@myserver.com> -p 2222    # custom port (-p)

# First time connecting → asks to confirm fingerprint, type "yes"

# Exit the remote session

exit

SSH keys — login without typing a password every time

bash# Generate a key pair (run once on your Mint machine)
ssh-keygen -t ed25519 -C "harsh@mint"

# Press Enter to accept defaults — creates ~/.ssh/id_ed25519 (private)

# and ~/.ssh/id_ed25519.pub (public)

# Copy your public key to the remote server

ssh-copy-id username@server_ip

# Now SSH in without a password

ssh username@server_ip

SSH config — stop typing long commands

bash# Edit ~/.ssh/config
nano ~/.ssh/config

Host myserver
    HostName 192.168.1.10
    User harsh
    Port 22
    IdentityFile ~/.ssh/id_ed25519

bash# Now just type:
ssh myserver

scp — copy files over SSH

bashscp file.txt username@server_ip:/home/username/      # local → remote
scp username@server_ip:/home/username/file.txt .     # remote → local
scp -r myfolder/ username@server_ip:/home/username/   # copy a folder

curl — talk to APIs and websites from the terminal

bashcurl <https://api.github.com>                  # GET request, print response
curl -o output.html <https://example.com>      # save response to a file
curl -I <https://example.com>                  # headers only (no body)

# GET with query params

curl "<https://api.example.com/users?id=5>"

# POST request with JSON body

curl -X POST <https://api.example.com/users> \
  -H "Content-Type: application/json" \
  -d '{"name": "Harsh", "age": 22}'

# Add an auth header (common with APIs)

curl <https://api.example.com/data> \
  -H "Authorization: Bearer YOUR_TOKEN"

# Follow redirects

curl -L <https://short.link/abc>

# Show response code only

curl -o /dev/null -s -w "%{http_code}\n" <https://example.com>

Ports — what's listening, what's open

bash# Check what's listening on your machine
sudo ss -tulpn          # modern tool — shows TCP/UDP listening ports
sudo netstat -tulpn     # older tool, same purpose (may need: sudo apt install net-tools)

# Check if a specific port is in use

sudo lsof -i :3000      # what's using port 3000?
sudo ss -tulpn | grep 3000

# Test if a remote port is open

nc -zv google.com 443   # netcat — check if port 443 is reachable
telnet google.com 80    # alternative way to test a port

# Kill whatever is using a port

sudo lsof -i :3000       # find the PID
kill -9 <PID>            # kill it

Quick network diagnostics

bashping google.com          # check if a host is reachable (Ctrl+C to stop)
ping -c 4 google.com     # send only 4 pings

curl -I <https://google.com>   # quick "is this site up" check

ip addr                  # show your machine's IP addresses
hostname -I               # quick way to get your local IP

traceroute google.com    # see the network hops to reach a host

# (install with: sudo apt install traceroute)

1. Real-Life Mental Model

TaskCommand"Log into a remote server"ssh user@ip"Login without typing a password"ssh-keygen + ssh-copy-id"Copy a file to/from a server"scp"Test an API quickly"curl"Is my app actually running on port X?"sudo lsof -i :PORT"Something's using my port, kill it"lsof -i :PORT → kill -9 <PID>"Is this website even reachable?"ping or curl -I

The debugging flow when "my app won't connect":

bash1. ping the host           → is it reachable at all?
2. curl -I the URL          → is the server responding?
3. sudo ss -tulpn           → is anything actually listening on that port?
4. sudo lsof -i :PORT       → what process owns that port?
5. Check firewall / sudo ufw status  → is the port blocked?

Key Takeaway

ssh gets you onto remote machines, curl lets you talk to APIs and servers without a browser, and port-checking tools (ss, lsof) tell you what's actually running and listening. Together, these three skills are what separate "I can use Linux" from "I can work with real servers and APIs" — which is most of actual backend/devops work.
