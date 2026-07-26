# Network Scanning Methodology - Week 6

## What is Network Scanning?
Network scanning is the process of discovering and analysing
devices, services, and vulnerabilities on a network.
It is the second phase of ethical hacking — it comes after
reconnaissance and before exploitation.

Used by:
- Attackers to find entry points into a network
- Defenders to see what is exposed on their own network
- Penetration testers to map a target before attacking

## The 4-Phase Scanning Methodology
Always follow this order. Never skip phases.

### Phase 1 — Host Discovery
Find which devices are actually online and responding.

Why do this first:
- Saves time by only scanning live hosts
- Reduces network noise
- Gives you a clear list of targets

What you get:
- List of IP addresses that are online
- MAC addresses of local devices
- Response times

Command:
nmap -sn 192.168.1.0/24

---

### Phase 2 — Port Discovery
Scan live hosts to find which ports are open.

Why do this second:
- Only scan hosts you know are alive
- Find all potential entry points
- Map the attack surface of each host

What you get:
- List of open ports on each host
- Port states (open, closed, filtered)

Command:
nmap -sS 192.168.1.1

---

### Phase 3 — Service Identification
Find out what software is running on each open port.

Why do this third:
- Knowing the service tells you what attacks to try
- Version numbers help you find known CVEs
- Identifies the technology stack

What you get:
- Service name on each open port
- Software version numbers
- Operating system information

Command:
nmap -sV 192.168.1.1

---

### Phase 4 — Vulnerability Assessment
Research and identify vulnerabilities in the discovered services.

Why do this last:
- You need service versions before you can research CVEs
- Targeted research based on what you actually found
- Guides your exploitation strategy

What you get:
- Known CVEs for discovered software versions
- Misconfigurations and weaknesses
- Prioritised list of attack vectors

Tools used:
- Nmap NSE scripts: nmap --script vuln 192.168.1.1
- Nessus
- OpenVAS
- Searchsploit to find exploits for CVEs

---

## The Flow in One Line
Host Discovery → Port Discovery → Service ID → Vulnerability Assessment

## Active vs Passive Scanning

### Passive Scanning
Gathering information without directly interacting with the target.
You observe traffic or use public sources.

Examples:
- Watching network traffic with Wireshark
- Using Shodan to find exposed services
- Reading WHOIS records
- Google dorking

Advantages:
- Very hard to detect
- No risk of disrupting target systems

Disadvantages:
- Limited information
- Cannot find all open ports

---

### Active Scanning
Directly sending packets to the target and analysing responses.

Examples:
- Nmap port scans
- Banner grabbing
- Vulnerability scanning with Nessus

Advantages:
- Comprehensive information
- Finds open ports and running services

Disadvantages:
- Detectable by IDS and firewalls
- Can disrupt services if too aggressive
- Requires authorisation — illegal without permission

---

## Stealth and Evasion

### Why stealth matters
Security systems like IDS (Intrusion Detection Systems) watch
for scanning activity. As a pentester you need to know how to
scan without triggering alerts.

### Timing control
Slower scans are harder to detect:
- nmap -T0 = paranoid slow (hardest to detect)
- nmap -T1 = sneaky
- nmap -T3 = normal default
- nmap -T4 = aggressive fast
- nmap -T5 = insane (very detectable)

### Packet fragmentation
Breaking scan packets into smaller pieces to evade
simple packet inspection filters:
nmap -f 192.168.1.1

### Source port manipulation
Using a trusted port as your source port to bypass
basic firewall rules:
nmap --source-port 53 192.168.1.1

## Output and Saving Results
Always save your scan results. Never rely on memory.

### Save as text file (human readable)
nmap -sV 192.168.1.1 -oN results.txt

### Save as XML (for tools to parse)
nmap -sV 192.168.1.1 -oX results.xml

### Save in all formats at once
nmap -sV 192.168.1.1 -oA results

### View saved results
cat results.txt

## Analysing Scan Results

### What to look for
- Open ports that should not be open
- Old software versions with known CVEs
- Services running on unexpected ports
- Default service banners revealing version information

### Pattern recognition
- Multiple open ports on one host = high value target
- Port 22 open = SSH brute force possible
- Port 3389 open = RDP attack possible
- Port 445 open = SMB attack possible
- Port 80/443 open = web application attacks possible

### Risk assessment questions to ask
1. Does this port need to be open?
2. Is this software version up to date?
3. Is there a known exploit for this version?
4. What is the impact if this is compromised?

## Manual vs Automated Scanning

### Manual scanning (Nmap)
- Full control over what you scan and how
- Better for stealth and targeted scanning
- Requires more knowledge to interpret results
- Best for penetration testing

### Automated scanning (Nessus, OpenVAS)
- Scans faster and covers more vulnerabilities
- Less control over the scanning process
- Better for compliance and large network assessments
- Results need manual verification to remove false positives

### When to use each
- Use Nmap first to map the network manually
- Use automated tools after to check for known vulnerabilities
- Always manually verify automated findings

## Common Scanning Tools

| Tool | Type | What it does |
|------|------|-------------|
| Nmap | Manual | Port scanning, service detection, OS detection |
| Nessus | Automated | Comprehensive vulnerability scanning |
| OpenVAS | Automated | Free open source vulnerability scanner |
| OWASP ZAP | Automated | Web application vulnerability scanner |
| Burp Suite | Manual | Web application testing and interception |
| Wireshark | Passive | Network traffic capture and analysis |
| Shodan | Passive | Search engine for internet connected devices |

## Summary — Key Points to Remember
- Always scan in order: Host Discovery → Port → Service → Vulnerability
- Passive scanning = no direct contact with target
- Active scanning = sending packets directly to target
- Always get written permission before active scanning
- Save all results with -oN for documentation
- Slower timing templates are harder to detect
- Manual scanning gives control, automated scanning gives coverage
- Always verify automated results manually before reporting
