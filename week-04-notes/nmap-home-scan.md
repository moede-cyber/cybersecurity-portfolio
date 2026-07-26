# Nmap Home Network Scan - Week 4

## Scan Details
- Date performed:
- Network range scanned:
- Tool used: Nmap on Kali Linux

## Step 1 - Find My Network Range
Command run:
```bash
ip a
```
Result: (write what your network range was here e.g. 192.168.1.0/24)

## Step 2 - Host Discovery (Find All Live Devices)
Command run:
```bash
nmap -sn 192.168.1.0/24
```
### Devices Found
| IP Address | Status | Device (guess what it is) |
|------------|--------|--------------------------|
|            | Up     |                          |
|            | Up     |                          |
|            | Up     |                          |
|            | Up     |                          |

Total devices found:

## Step 3 - Port Scan on Router
Command run:
```bash
nmap -sV 192.168.1.1
```
(Replace 192.168.1.1 with your actual router IP)

### Open Ports Found
| Port | State | Service | Version |
|------|-------|---------|---------|
|      | open  |         |         |
|      | open  |         |         |
|      | open  |         |         |

## Step 4 - OS Detection on Router
Command run:
```bash
sudo nmap -O 192.168.1.1
```
OS detected:

## Step 5 - Save Results to File
Command run:
```bash
nmap -A 192.168.1.1 -oN home-scan-results.txt
```

## What I Found (Analysis)
- Total devices on my network:
- Most interesting open port:
- What operating system is my router running:
- One thing that surprised me:

## Attacker Perspective
If an attacker ran this scan on my network, here is what they would learn:
-
-
-

## Defender Perspective
To protect my network from this kind of scan I should:
-
-
-
