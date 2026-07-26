# Common Ports - Week 4 Notes

## What is a Port?
A port is like a door on a computer. Every computer has 65,535 ports.
When you connect to a website or service, you connect through a specific port number.
Attackers scan ports to find which ones are open and what services are running behind them.

## Port States
- **Open** — something is listening and accepting connections (potential entry point)
- **Closed** — port exists but nothing is running on it
- **Filtered** — a firewall is blocking it, Nmap cannot tell if it is open or closed

## The Most Important Ports to Know

| Port | Protocol | Service | What it does |
|------|----------|---------|--------------|
| 21   | TCP | FTP | File Transfer Protocol — transferring files between computers |
| 22   | TCP | SSH | Secure Shell — remote login to a Linux/Unix system |
| 23   | TCP | Telnet | Old remote login — sends data in plaintext (insecure, replaced by SSH) |
| 25   | TCP | SMTP | Sending emails between mail servers |
| 53   | TCP/UDP | DNS | Translates domain names to IP addresses (google.com → 142.250.x.x) |
| 80   | TCP | HTTP | Unencrypted web traffic |
| 110  | TCP | POP3 | Receiving emails (older method) |
| 143  | TCP | IMAP | Receiving emails (modern method, syncs across devices) |
| 443  | TCP | HTTPS | Encrypted web traffic (the padlock in your browser) |
| 445  | TCP | SMB | Windows file sharing — common attack target (EternalBlue/WannaCry) |
| 3306 | TCP | MySQL | MySQL database connections |
| 3389 | TCP | RDP | Remote Desktop Protocol — graphical remote access to Windows |
| 5432 | TCP | PostgreSQL | PostgreSQL database connections |
| 8080 | TCP | HTTP-Proxy | Alternative web port, often used for development or proxies |

## The 5 You Must Know by Heart
- **22** = SSH (Linux remote login)
- **80** = HTTP (websites unencrypted)
- **443** = HTTPS (websites encrypted)
- **21** = FTP (file transfer)
- **3389** = RDP (Windows remote desktop)

## Why Ports Matter for Cybersecurity
- Open port 22 = you can try SSH brute force attacks
- Open port 80/443 = web application attacks (SQLi, XSS)
- Open port 3389 = Windows RDP attacks
- Open port 445 = SMB attacks (like WannaCry ransomware used this)
- Open port 3306 with no password = direct database access

## How to Scan Ports with Nmap
```bash
# Find all live hosts on your network
nmap -sn 192.168.1.0/24

# Scan the most common 1000 ports on a target
nmap 192.168.1.1

# Scan all 65535 ports
nmap -p- 192.168.1.1

# Scan specific ports
nmap -p 22,80,443,3306,3389 192.168.1.1

# Detect what service and version is running on each open port
nmap -sV 192.168.1.1
```

## Exam Tips
- Port numbers below 1024 are called **well-known ports** — reserved for standard services
- Port numbers
