# Active Reconnaissance and Nmap - Week 6

## What is Active Reconnaissance?
Active reconnaissance means directly interacting with the target
to gather information. You send packets and analyse the responses.

Unlike passive recon where you only observe, active recon
involves touching the target system directly.

Important: Active recon is detectable. Always have written
permission before performing active recon on any network.

## Active Recon Techniques

### Port Scanning
Sending packets to ports on a target to find which ones are open.
This is the most common active recon technique.
Tool: Nmap

### Banner Grabbing
Connecting to an open port and reading the service banner.
The banner often reveals software name and version.
Example banner from SSH port:
SSH-2.0-OpenSSH_8.2p1 Ubuntu-4ubuntu0.5

Tool: Netcat
Command:
nc 192.168.1.1 22

### Service Enumeration
Going deeper than just finding open ports.
Finding exactly what software, version, and configuration
is running on each port.
Tool: Nmap -sV

### OS Fingerprinting
Identifying what operating system the target is running
by analysing how it responds to packets.
Different OS handle packets slightly differently.
Tool: Nmap -O

## Nmap Deep Dive

### What Nmap Does
Nmap sends carefully crafted packets to target systems
and analyses how they respond to determine:
- Which hosts are online
- Which ports are open, closed, or filtered
- What services are running on open ports
- What operating system the target is running

### How Nmap Detects Port States
Nmap sends a SYN packet and reads the response:

Open port:
- Nmap sends SYN
- Target replies SYN-ACK
- Nmap sends RST to close without completing handshake
- Result: port is OPEN

Closed port:
- Nmap sends SYN
- Target replies RST
- Result: port is CLOSED

Filtered port:
- Nmap sends SYN
- No response or ICMP unreachable message
- Result: port is FILTERED (firewall blocking it)

## All the Nmap Scan Types You Need to Know

### TCP SYN Scan — Most Common
Command: sudo nmap -sS 192.168.1.1
What it does: Sends SYN, gets SYN-ACK, sends RST. Never completes handshake.
Why use it: Fast, stealthy, reliable. Default scan for most situations.
Requires: sudo or root

### TCP Connect Scan
Command: nmap -sT 192.168.1.1
What it does: Completes the full TCP handshake.
Why use it: When you do not have root/sudo access.
Downside: More detectable than SYN scan, slower.

### UDP Scan
Command: sudo nmap -sU 192.168.1.1
What it does: Scans UDP ports instead of TCP.
Why use it: DNS (53), SNMP (161), DHCP (67) run on UDP.
Downside: Very slow — UDP has no handshake so Nmap waits for timeouts.

### Ping Sweep — Host Discovery
Command: nmap -sn 192.168.1.0/24
What it does: Finds all live hosts on a network. No port scanning.
Why use it: Always run this first before any other scan.

### Version Detection
Command: nmap -sV 192.168.1.1
What it does: Connects to open ports and identifies service versions.
Why use it: Version numbers let you search for known CVEs.
Example output: 22/tcp open ssh OpenSSH 8.2p1

### OS Detection
Command: sudo nmap -O 192.168.1.1
What
