# Nmap Home Network Scan - Week 6

## Scan Details
Date performed:
Network range scanned:
Tool used: Nmap on Kali Linux

---

## Step 1 — Find My Network Range
Command run:
ip a

Result:
(write your network range here e.g. 192.168.1.0/24)

---

## Step 2 — Host Discovery
Command run:
nmap -sn 192.168.1.0/24

Devices found:

| IP Address | Status | What I think it is |
|------------|--------|-------------------|
|            | Up     |                   |
|            | Up     |                   |
|            | Up     |                   |
|            | Up     |                   |
|            | Up     |                   |

Total devices found:

---

## Step 3 — Port Scan on Router
Command run:
nmap -sV 192.168.1.1

Open ports found:

| Port | State | Service | Version |
|------|-------|---------|---------|
|      | open  |         |         |
|      | open  |         |         |
|      | open  |         |         |

---

## Step 4 — OS Detection
Command run:
sudo nmap -O 192.168.1.1

OS detected:

---

## Step 5 — Aggressive Scan and Save Results
Command run:
sudo nmap -A 192.168.1.1 -oN home-scan-results.txt

Interesting findings from aggressive scan:

---

## My Analysis

Total devices on my network:

Most interesting open port and why:

What operating system is my router running:

One thing that surprised me:

---

## Attacker Perspective
If an attacker ran this scan on my network they would learn:
-
-
-

What they could potentially do with that information:
-
-

---

## Defender Perspective
To protect my network from this kind of reconnaissance I should:
-
-
-

## What I Learned From This Scan
(fill in after completing the scan)
