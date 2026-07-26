# Analyzing Vulnerability Scan Results - Week 6

## Why Analysis Matters
Running a scan is easy. Understanding the results is the skill.
A vulnerability scanner produces raw data — your job is to turn
that data into actionable intelligence.

Bad analyst: copies scanner output into a report
Good analyst: verifies findings, removes false positives,
prioritises by real risk, and provides clear remediation steps

## Understanding Scan Output

### Reading Nmap Output
Example output:
PORT     STATE   SERVICE   VERSION
22/tcp   open    ssh       OpenSSH 7.2p2
80/tcp   open    http      Apache 2.4.18
443/tcp  open    https     Apache 2.4.18
3306/tcp open    mysql     MySQL 5.7.12
8080/tcp filtered http-proxy

What this tells you:
- SSH is running OpenSSH 7.2p2 — search CVEs for this version
- Apache 2.4.18 is old — likely has known vulnerabilities
- MySQL is exposed on port 3306 — should not be publicly accessible
- Port 8080 is filtered — firewall is blocking it

### Reading Nessus or OpenVAS Output
Automated scanners group findings by severity:

Critical (red):
- Immediately exploitable vulnerabilities
- Patch or mitigate urgently
- Example: Remote code execution vulnerability

High (orange):
- Serious vulnerabilities requiring prompt attention
- Example: SQL injection found in web application

Medium (yellow):
- Vulnerabilities that require exploitation conditions
- Example: Cross-site scripting vulnerability

Low (blue):
- Minor issues with limited impact
- Example: Missing security headers

Informational (green):
- Not vulnerabilities — just useful information
- Example: Server software version disclosed

## False Positive Identification

### What is a false positive?
A false positive is when the scanner reports a vulnerability
that does not actually exist on the target.

Scanners are not perfect. They sometimes flag issues based on
version numbers without checking if the patch was backported.

### How to identify false positives
Step 1: Look at the CVE the scanner flagged
Step 2: Check what version is vulnerable
Step 3: Check if the target version matches
Step 4: Manually test to confirm the vulnerability exists
Step 5: If you cannot reproduce it — it is likely a false positive

### Common causes of false positives
- Software version matches but patch was applied
- Service running on non-standard port confusing scanner
- Firewall rules preventing full vulnerability check
- Scanner using outdated signatures

### Why removing false positives matters
- Reporting false positives damages your credibility
- Client wastes time patching things that are not broken
- Real vulnerabilities get ignored while chasing false ones

## Risk Assessment and Prioritisation

### Not all vulnerabilities are equal
A critical vulnerability on an internal test server
is less urgent than a medium vulnerability on a
public-facing web server handling payments.

Consider:
- Exploitability: How easy is it to exploit?
- Impact: What happens if it is exploited?
- Asset value: How important is the affected system?
- Exposure: Is it internal or internet-facing?

### Risk matrix

| Exploitability | Impact | Priority |
|---------------|--------|----------|
| High | High | Fix immediately |
| High | Low | Fix soon |
| Low | High | Fix soon |
| Low | Low | Fix when possible |

### Questions to ask for each finding
1. Is this a real vulnerability or a false positive?
2. How easy is it to exploit with public tools?
3. What is the worst case if exploited?
4. Is this system internet-facing or internal only?
5. Does a patch or fix already exist?
6. How long would remediation take?

## Pattern Recognition

### What patterns tell you
Patterns across multiple findings reveal bigger problems
than individual vulnerabilities.

Common patterns to look for:

Old software everywhere:
- Multiple services running outdated versions
- Indicates poor patch management programme
- Priority: Implement regular patching schedule

Default credentials on multiple systems:
- Admin panels, routers, databases using admin/admin
- Indicates poor security baseline
- Priority: Change all default credentials immediately

Missing security headers across all web applications:
- No Content-Security-Policy, no X-Frame-Options
- Indicates no security standards for web development
- Priority: Implement security header policy

Open database ports on internet-facing servers:
- MySQL or PostgreSQL accessible from internet
- Indicates poor network segmentation
- Priority: Firewall these ports immediately

## Service Correlation

### What is service correlation?
Looking at how different services relate to each other
and what attack paths they create together.

### Example
Finding 1: SSH running old version with CVE
Finding 2: Web application has SQL injection
Finding 3: MySQL running on same server

Correlation:
- SQLi could dump database credentials
- Those credentials might work for SSH login
- SSH access gives full server control
- Single web vulnerability leads to complete server compromise

This is called chaining vulnerabilities — combining multiple
lower-risk findings into a critical attack path.

## Documenting Results

### What to document for every finding

Finding name:
Apache 2.4.18 Multiple Vulnerabilities

CVE reference:
CVE-2017-7679, CVE-2017-7668

Severity:
High — CVSS 7.5

Affected system:
192.168.1.10 — web server — port 80 and 443

Description:
Apache version 2.4.18 is running on the target web server.
This version contains multiple known vulnerabilities including
a buffer overflow that could allow remote code execution.

Evidence:
nmap -sV output showing Apache 2.4.18
Screenshot of response header showing server version

Recommendation:
Update Apache to the latest stable version immediately.
As an interim measure restrict access to the web server
from untrusted networks.

References:
https://httpd.apache.org/security/vulnerabilities_24.html
https://nvd.nist.gov/vuln/detail/CVE-2017-7679

### GitHub documentation format
For each scan you do create a file with:
- Date and target
- Commands run
- Key findings
- Your analysis
- Recommended fixes

## Trend Analysis

### Why track trends over time
A single scan shows a snapshot.
Multiple scans over time show whether security is improving.

Track:
- Total vulnerabilities found each scan
- How quickly vulnerabilities get patched
- New vulnerabilities appearing
- Repeat findings that never get fixed

Repeat findings are the most important:
If the same vulnerability appears in scan after scan
it means remediation is not happening.
This is a process problem not just a technical one.

## Reporting

### Who reads vulnerability reports
Technical report: Goes to the IT team doing the fixing
Executive report: Goes to management who approve budget

### Technical report contents
- Executive summary (one page)
- Methodology used
- Scope of assessment
- Detailed findings with evidence
- Remediation recommendations
- Appendix with raw scan output

### How to write a finding clearly
Bad: "Apache is vulnerable"
Good: "Apache 2.4.18 running on 192.168.1.10 contains
CVE-2017-7679 (CVSS 7.5) which allows a remote attacker
to cause a buffer overflow via a crafted Accept-Charset
header. Update to Apache 2.4.26 or later to remediate."

### Remediation priorities
P1 Critical: Fix within 24 hours
P2 High: Fix within 7 days
P3 Medium: Fix within 30 days
P4 Low: Fix within 90 days
P5 Informational: Address in next review cycle

## Connecting to Your Labs

### After your Nmap home scan
Apply this analysis process:
1. List every open port you found
2. Look up the software version on cve.mitre.org
3. Check CVSS score for any CVEs found
4. Decide: is this a real risk on your home network?
5. Document your findings in nmap-home-scan.md

### After PortSwigger labs
Document each lab as a finding:
- What was the vulnerability
- How you found it
- What the impact would be in real life
- How the developer should fix it

## Summary — Key Points to Remember
- Analysis turns raw scan data into actionable intelligence
- Always verify findings manually before reporting
- False positives are common — remove them before reporting
- Prioritise by exploitability AND impact AND asset value
- Look for patterns across findings not just individual issues
- Chain vulnerabilities together to show real attack paths
- Document every finding with CVE, severity, evidence, and fix
- Track findings over time to measure security improvement
- Critical findings need immediate attention within 24 hours
- The best report is clear, concise, and tells the reader exactly what to fix
