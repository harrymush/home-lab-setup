```markdown
# DNS Server Setup on macOS High Sierra (iMac 2011)

## Objective

Set up a DNS server on an old iMac (2011) running macOS High Sierra using `dnsmasq` to manage local domain resolution and ad-blocking for devices on your home network.

---

## Steps Taken

### 1. Install `dnsmasq` Using Homebrew

#### Install Homebrew (if not already installed):

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

#### Install `dnsmasq`:

```bash
brew install dnsmasq
```

---

### 2. Configure `dnsmasq`

#### Edit the configuration file:

```bash
sudo nano /usr/local/etc/dnsmasq.conf
```

#### Add local domain and upstream DNS settings:

```
address=/myhomelab.local/192.168.1.74
server=8.8.8.8
log-queries
log-facility=/var/log/dnsmasq.log
```

Save and exit (`Ctrl + X`, then `Y`, then `Enter`).

#### Start `dnsmasq`:

```bash
sudo brew services start dnsmasq
```

---

### 3. Configure Ad-Blocking

#### Edit `dnsmasq` config:

```bash
sudo nano /usr/local/etc/dnsmasq.conf
```

#### Add ad-blocking entries (add as many as needed):

```
address=/adserver.com/0.0.0.0
address=/doubleclick.net/0.0.0.0
```

Save and exit.

---

### 4. Enable DNS Query Logging

Ensure these lines are present in `/usr/local/etc/dnsmasq.conf`:

```
log-queries
log-facility=/var/log/dnsmasq.log
```

#### Set up the log file:

```bash
sudo touch /var/log/dnsmasq.log
sudo chmod 644 /var/log/dnsmasq.log
```

---

### 5. Log Management (Manual Rotation)

#### Option A: Delete the log file

```bash
sudo rm /var/log/dnsmasq.log
```

#### Option B: Clear the log contents without deleting the file

```bash
sudo truncate -s 0 /var/log/dnsmasq.log
```

---

### 6. Test the DNS Setup

#### Use `nslookup` to test local resolution:

```bash
nslookup server1.myhomelab.local 127.0.0.1
```

---

### 7. Configure Client Device (Laptop)

#### Set DNS server:

- Open **Network Preferences**
- Select your network interface
- Click **Advanced > DNS**
- Set DNS server to: `192.168.1.74`

#### Verify with:

```bash
nslookup server1.myhomelab.local
```

---

## Future Considerations

- Automate log rotation (e.g. using a `cron` job or `logrotate`)
- Restart `dnsmasq` periodically or on failure
- Explore features like DNS caching, blacklisting, or enhanced DNS security (e.g. DNSSEC, DoH)

---

## Notes

- Configuration tested and running successfully on macOS High Sierra (iMac 2011)
- `dnsmasq` version installed via Homebrew

---
```
