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
DNS = Domain Name System — it is the internets phone book.

### Step 2 — TCP Connection
Your browser connects to that IP address on port 80 (HTTP)
or port 443 (HTTPS) using the TCP three-way handshake:
- SYN (I want to connect)
- SYN-ACK (okay)
- ACK (connected)

### Step 3 — HTTP Request
Your browser sends a request to the web server:
GET /index.html HTTP/1.1
Host: google.com
User-Agent: Chrome/114
This is asking the server: please give me the homepage.

### Step 4 — Server Response
The web server sends back:
HTTP/1.1 200 OK
Content-Type: text/html
Then the page content follows.

### Step 5 — Browser Renders the Page
Your browser reads the HTML and displays the page you see.

## HTTP Methods
These are the types of requests a browser can make:

| Method | What it does | Example |
|--------|-------------|---------|
| GET | Fetch a page or resource | Loading a webpage |
| POST | Send data to the server | Submitting a login form |
| PUT | Update existing data | Editing your profile |
| DELETE | Remove data | Deleting a post |
| PATCH | Partially update data | Changing just your email |

Why this matters for hacking:
- GET requests put data in the URL — easy to see and modify
- POST requests put data in the body — still interceptable with Burp Suite
- Burp Suite intercepts both GET and POST requests

## HTTP Status Codes

| Code | Meaning | What it means for hackers |
|------|---------|--------------------------|
| 200 | OK | Page loaded successfully |
| 301 | Moved Permanently | Redirected to another page |
| 302 | Found | Redirected temporarily |
| 403 | Forbidden | Page exists but you are blocked |
| 404 | Not Found | Page does not exist |
| 500 | Internal Server Error | Something broke on the server |

Hacker tips:
- 403 means the page exists but you are blocked — worth investigating
- 500 errors can reveal server information
- 302 redirects after login can sometimes be bypassed

## HTTP Headers
Headers are extra information sent with every request and response.

### Request Headers (sent by your browser)
- Host: example.com
- User-Agent: tells the server what browser you are using
- Cookie: sends your session cookie with every request
- Content-Type: tells server what format the data is in

### Response Headers (sent by the server)
- Server: Apache/2.4.41 — reveals server software
- Set-Cookie: gives you a cookie to store
- X-Powered-By: PHP/7.4.3 — reveals backend technology

Why headers matter for hacking:
- Server and X-Powered-By headers reveal technology versions
- Attackers search for known vulnerabilities for those versions
- Missing security headers are findings in a pentest report

## Cookies and Sessions

### What is a Cookie?
A small piece of data the server stores in your browser.
Used to remember who you are between requests.
HTTP has no memory — cookies solve this.

### What is a Session?
When you log in the server creates a session and gives you
a session ID stored in a cookie.
Every request you make sends that cookie so the server
knows it is you.

Why this matters for hacking:
- Stealing a session cookie = stealing someones login
- XSS attacks are used to steal cookies
- Weak session IDs can be predicted or brute forced
- Cookies without the HttpOnly flag can be read by JavaScript

## HTTPS vs HTTP
- HTTP = unencrypted — anyone on the network can read your traffic
- HTTPS = encrypted using TLS — traffic is scrambled in transit
- The padlock in your browser means HTTPS is working
- HTTPS protects data in transit — not the application itself
- A site can use HTTPS and still be vulnerable to SQLi and XSS

## How Forms Work
When you fill in a login form and click Submit:
1. Browser sends a POST request
2. Data goes in the request body: username=admin&password=1234
3. Server checks the credentials against the database
4. Server creates a session and sends back a cookie
5. Browser stores the cookie and uses it for all future requests

Why this matters for hacking:
- The username and password are user input going into a database
- User input without checking = SQL injection
- User input displayed back on a page = XSS

## Developer Tools — Your Free Built-In Hacking Tool
Right-click any webpage and click Inspect

| Tab | What it shows | Why it matters |
|-----|--------------|----------------|
| Elements | HTML structure of the page | Find hidden fields and comments |
| Network | Every HTTP request and response | See exactly what data is sent |
| Console | JavaScript errors | Find JavaScript vulnerabilities |
| Application | Cookies and local storage | See and modify your cookies |

How to use the Network tab:
1. Open Developer Tools with F12
2. Click the Network tab
3. Reload the page
4. Click any request to see headers, body, and response
5. This is exactly what Burp Suite shows — but free in your browser

## Summary — Key Points to Remember
- Every website visit is an HTTP request and response
- GET fetches data, POST sends data
- Status codes tell you what happened — 200 OK, 403 Forbidden, 500 Error
- Headers reveal server technology and control security settings
- Cookies maintain your session between requests
- HTTPS encrypts traffic but does not protect the application from attacks
- Developer Tools lets you see all HTTP traffic in your browser right now
