# Nmap Notes - Week 4

## What is Nmap?
Nmap (Network Mapper) is a free tool used to scan networks.
It sends packets to target systems and analyses the responses.
Used by both attackers (to find entry points) and defenders (to see what is exposed).

## How Nmap Works
1. Sends a packet to a target port
2. Analyses the response
3. Determines if the port is open, closed, or filtered

## The TCP Three-Way Handshake
Normal connection between two computers:
- Step 1: Client sends SYN (I want to connect)
- Step 2: Server replies SYN-ACK (okay, connect)
- Step 3: Client sends ACK (connected)

Nmap uses this to detect port states without completing the full connection.

## The 6 Nmap Commands You Need to Know

### 1. Find all live devices on your network
```bash
nmap -sn 192.168.1.0/24
```
Use this first. Shows every device online. No port scanning yet.

### 2. Stealth SYN scan (most common scan)
```bash
nmap -sS 192.168.1.1
```
Sends a SYN packet, gets SYN-ACK back, then sends RST to close.
Never completes the handshake so it is harder to detect.
Requires sudo/root to run.

### 3. Detect service versions on open ports
```bash
nmap -sV 192.168.1.1
```
Tells you what software is running and what version.
Example output: 22/tcp open ssh OpenSSH 8.2

### 4. Detect the operating system
```bash
nmap -O 192.168.1.1
```
Tries to guess what OS the target is running (Windows, Linux, etc).
Requires sudo/root to run.

### 5. Aggressive scan (everything at once)
```bash
nmap -A 192.168.1.1
```
Combines OS detection + version detection + script scanning + traceroute.
Loud and detectable — only use on your own network or with permission.

### 6. Save results to a file
```bash
nmap -sV 192.168.1.1 -oN scan-results.txt
```
Saves your output so you can document it for GitHub.

## Timing Templates (How Fast to Scan)
- **-T0** = Paranoid — extremely slow, hardest to detect
- **-T1** = Sneaky — slow, avoids detection
- **-T2** = Polite — slower than normal
- **-T3** = Normal — default speed
- **-T4** = Aggressive — fast, good for your own network
- **-T5** = Insane — very fast, may miss results

For your home network use -T4. Never use T4 or T5 on a network you don't own.

## Scanning Methodology (Use This Order Every Time)
1. Host Discovery — find who is alive
   nmap -sn 192.168.1.0/24

2. Port Discovery — find open ports on live hosts
   nmap -sS 192.168.1.1

3. Service Identification — find what is running on open ports
   nmap -sV 192.168.1.1

4. OS Detection — find what operating system is running
   nmap -O 192.168.1.1

5. Save results — document everything
   nmap -A 192.168.1.1 -oN results.txt

## Port States
- **Open** — service is running and accepting connections
- **Closed** — port exists but no service is running
- **Filtered** — firewall is blocking Nmap from getting a response

## Common Nmap Scan Types
| Flag | Scan Type | When to Use |
|------|-----------|-------------|
| -sn  | Ping sweep | Find live hosts first |
| -sS  | SYN scan | Default stealth scan |
| -sT  | TCP connect | When you don't have root |
| -sU  | UDP scan | Find UDP services (DNS, SNMP) |
| -sV  | Version detection | Identify software versions |
| -O   | OS detection | Identify operating system |
| -A   | Aggressive | Everything at once |

## Exam Tips
- Always run nmap -sn first before any other scan
- -sS requires root/sudo — if you get a permission error add sudo
- -A is noisy — never use on a network without permission
- Save all scan results with -oN for your GitHub documentation
- Nmap scans only TCP by default — add -sU for UDP services
