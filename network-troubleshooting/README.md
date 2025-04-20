
# 📡 Network Troubleshooting & Port Check Toolkit (For Cloud Ubuntu VMs)

## 📦 Tools Required
Make sure these are installed (or install via `apt`):

```bash
sudo apt update
sudo apt install -y netcat curl telnet nmap iputils-ping traceroute dnsutils tcpdump mtr socat
```

---

## ✅ 1. Check if **Outbound Port** is Open

### 🔹 Method 1: Using `nc` (Netcat)
```bash
nc -zv <host> <port>
```

**Example:**
```bash
nc -zv google.com 443
```

- `-z`: Zero I/O mode (just checks connection).
- `-v`: Verbose output.

### 🔹 Method 2: Using `telnet`
```bash
telnet <host> <port>
```

**Example:**
```bash
telnet google.com 443
```

> ✅ Success: Connected  
> ❌ Failure: Connection refused or timeout

### 🔹 Method 3: Using `curl`
```bash
curl -v telnet://<host>:<port>
```

---

## ✅ 2. Check if **Inbound Port** is Open

### 🔸 Step 1: On the **receiver VM** (listen on a port)
```bash
nc -lvk <port>
```

**Example:**
```bash
nc -lvk 5000
```

- `-l`: Listen mode  
- `-v`: Verbose  
- `-k`: Keep open for multiple connections

### 🔸 Step 2: On the **sender VM**
```bash
nc <receiver-ip> 5000
```

If it connects, the port is open and reachable.

---

## ✅ 3. DNS Resolution Check

```bash
dig <hostname>
```

**Example:**
```bash
dig google.com
```

or use `nslookup`:

```bash
nslookup google.com
```

---

## ✅ 4. Ping (ICMP) – Basic Reachability

```bash
ping <ip or hostname>
```

Example:
```bash
ping google.com
```

> Note: Some cloud providers block ICMP by default; don't panic if ping fails.

---

## ✅ 5. Traceroute – Trace the path to a host

```bash
traceroute <host>
```

Example:
```bash
traceroute google.com
```

---

## ✅ 6. Port Scanning

### 🔹 Using `nmap`
```bash
nmap -p <port> <ip or domain>
```

**Example:**
```bash
nmap -p 80,443 scanme.nmap.org
```

To scan a range:
```bash
nmap -p 1-1024 <ip>
```

---

## ✅ 7. Capturing Network Traffic

### 🔹 Using `tcpdump`
Capture HTTP traffic on eth0:
```bash
sudo tcpdump -i eth0 port 80
```

To write to a file:
```bash
sudo tcpdump -i eth0 -w output.pcap
```

Then open `.pcap` with Wireshark or analyze using CLI.

---

## ✅ 8. Real-time Network Performance Monitoring

### 🔹 Using `mtr` (better traceroute + ping)
```bash
mtr <hostname>
```

> Example: `mtr google.com`  
Interactive display of packet loss and latency at each hop.

---

## ✅ 9. Quick Socket Test with `socat`

Start a test TCP server on one VM:
```bash
socat -v TCP-LISTEN:12345,fork EXEC:/bin/cat
```

Connect from another VM:
```bash
socat - TCP:<server-ip>:12345
```

You can now type messages and check bidirectional socket communication.

---

## ✅ 10. Check Firewall (UFW) Status

```bash
sudo ufw status verbose
```

To allow a port:
```bash
sudo ufw allow 8080/tcp
```

To allow all outbound:
```bash
sudo ufw default allow outgoing
```

To block all outbound except specific ports:
```bash
sudo ufw default deny outgoing
sudo ufw allow out 443/tcp
```

---

## ✅ 11. Testing Outbound Port using Online Tools (Alternative)

Sometimes helpful to test from a cloud VM:
```bash
curl https://portquiz.net:443
```

Change port as needed:
```bash
curl https://portquiz.net:8080
```

If it fails, the port is likely blocked on outbound.

---

## 🧪 Test Scenarios to Try on Cloud Ubuntu VMs

| Test | Command | Expected Result |
|------|---------|-----------------|
| Outbound Port 80 Open | `nc -zv google.com 80` | Should connect |
| Inbound Port Test | `nc -lvk 8080` on receiver, then `nc <ip> 8080` on sender | Successful connection |
| DNS Test | `dig google.com` | Shows DNS info |
| Traceroute | `traceroute google.com` | Path to Google |
| Port Blocked? | `curl https://portquiz.net:25` | May fail if blocked |
| Firewall Rule Effect | `sudo ufw deny out 443` then `curl https://google.com` | Should fail |

---

## 🛠 Useful Aliases for Quick Checks

Add these to `~/.bashrc` or `~/.zshrc`:

```bash
alias portcheck="nc -zv"
alias pingg="ping google.com"
alias checkdns="dig google.com"
alias traceg="traceroute google.com"
```


