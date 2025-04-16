# home-lab-setup
DNS Server
Objective:

Set up a DNS server on an old iMac 2011 running macOS High Sierra using dnsmasq to manage local domain resolution and ad-blocking for devices on your home network.

Steps Taken:

Installed dnsmasq using Homebrew:
Install Homebrew (if not already installed): /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Install dnsmasq: brew install dnsmasq

Configure dnsmasq:
Edit dnsmasq configuration (/usr/local/etc/dnsmasq.conf): sudo nano /usr/local/etc/dnsmasq.conf


Add local domain settings: address=/myhomelab.local/192.168.1.74
server=8.8.8.8
log-queries
log-facility=/var/log/dnsmasq.log


Save and exit (Ctrl + X, then Y, then Enter).
Start dnsmasq: sudo brew services start dnsmasq


Configured Ad-Blocking:
Add ad-blocking lists in dnsmasq configuration: sudo nano /usr/local/etc/dnsmasq.conf


Add the following to block ads (you can find various blocklist URLs online): address=/adserver.com/0.0.0.0
address=/doubleclick.net/0.0.0.0


Save and exit.
DNS Query Logging:
Enable logging of queries in /usr/local/etc/dnsmasq.conf: sudo nano /usr/local/etc/dnsmasq.conf

Add the line: log-queries
log-facility=/var/log/dnsmasq.log

Save and exit.
Set permissions for the log file: sudo touch /var/log/dnsmasq.log
sudo chmod 644 /var/log/dnsmasq.log

Log Management (Manual Rotation):
Manually delete logs: sudo rm /var/log/dnsmasq.log

Clear logs without deleting the file: sudo truncate -s 0 /var/log/dnsmasq.log

Tested DNS Setup:
Test local domain resolution with nslookup: nslookup server1.myhomelab.local 127.0.0.1


Added DNS Server to Laptop:
Configure laptop to use the DNS server:
Open Network Preferences, select your network, and change the DNS to 192.168.1.74.
Test DNS resolution: nslookup server1.myhomelab.local




Future Considerations:
Plan to revisit automatic log rotation and DNS server restarts at a later stage.
Consider exploring additional DNS features such as caching and enhanced security.
