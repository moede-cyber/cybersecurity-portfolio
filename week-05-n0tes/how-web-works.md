# How the Web Works - Week 5

## The Big Picture
When you type a website address and press Enter, a lot happens
before you see the page. Understanding this is essential for
web application hacking — you cannot attack something you do
not understand.

## Step by Step — What Happens When You Visit a Website

### Step 1 — DNS Lookup
Your browser asks: what is the IP address of google.com?
A DNS server answers: 142.250.80.46
DNS = Domain Name System — it is the internet's phone book.

### Step 2 — TCP Connection
Your browser connects to that IP address on port 80 (HTTP)
or port 443 (HTTPS) using the TCP three-way handshake:
- SYN (I want to connect)
- SYN-ACK (okay)
- ACK (connected)

### Step 3 — HTTP Request
Your browser sends a request to the web server:
