# Web Attacks - Week 5

## OWASP Top 10
OWASP stands for Open Web Application Security Project.
The OWASP Top 10 is a list of the most common and dangerous
web application vulnerabilities. Every web security professional
must know this list.

## 1. SQL Injection (SQLi)

### What it is
SQL injection is when an attacker inserts SQL code into a form
or URL to manipulate the database behind the website.

### How it works
A normal login query looks like this:
SELECT * FROM users WHERE username='admin' AND password='1234'

An attacker types this in the username field:
admin' OR '1'='1

The query becomes:
SELECT * FROM users WHERE username='admin' OR '1'='1'

Since 1=1 is always true the attacker logs in without a password.

### Real world impact
- Steal all data from the database
- Bypass login pages without a password
- Delete or modify database records

### How to fix it
Use parameterised queries — never put user input directly into SQL.

---

## 2. Cross-Site Scripting (XSS)

### What it is
XSS is when an attacker injects malicious JavaScript into a webpage
that other users then load in their browser.

### Types of XSS

Reflected XSS:
The malicious script is in the URL.
The victim clicks a link and the script runs in their browser.

Stored XSS:
The malicious script is saved in the database.
Every user who loads the page runs the script automatically.
More dangerous than reflected because it affects all visitors.

DOM-based XSS:
The script manipulates the page directly in the browser
without going to the server at all.

### Real world impact
- Steal session cookies and log in as the victim
- Redirect users to fake login pages
- Capture keystrokes

### How to fix it
Encode all user input before displaying it on a page.
Use Content Security Policy headers.

---

## 3. IDOR (Insecure Direct Object Reference)

### What it is
IDOR is when a website uses a predictable reference to an object
like a number in the URL and does not check if you are allowed
to access it.

### How it works
Your profile URL is:
https://example.com/profile?id=1001

You change 1001 to 1002:
https://example.com/profile?id=1002

If the server does not check who owns id=1002
you can see someone elses data.

### Real world impact
- Access other users accounts and personal data
- Download files belonging to other users
- Modify or delete other users records

### How to fix it
Always check on the server that the logged in user owns
the object they are requesting.

---

## 4. Broken Authentication

### What it is
Weaknesses in how a website handles login and sessions
that allow attackers to take over accounts.

### Common examples
- No limit on login attempts — allows brute force attacks
- Weak or predictable session tokens
- Sessions that never expire
- Default credentials never changed (admin/admin)
- Passwords stored in plaintext in the database

### Real world impact
- Attacker brute forces login page and gains access
- Attacker steals session token and hijacks active session
- Attacker uses default credentials on admin panel

### How to fix it
- Lock accounts after failed login attempts
- Use strong random session tokens
- Expire sessions after inactivity
- Enforce strong password policies

---

## 5. CSRF (Cross-Site Request Forgery)

### What it is
CSRF tricks a logged in user into accidentally making a request
to a website they are already authenticated on.

### How it works
You are logged into your bank.
An attacker sends you a link to a malicious page.
That page secretly sends a transfer request to your bank.
Because you are already logged in your browser automatically
sends your session cookie with the request.
The bank thinks you made the transfer yourself.

### Real world impact
- Unauthorised fund transfers
- Password or email changes without victim knowing
- Account settings modified by attacker

### How to fix it
Use CSRF tokens — a secret value included in every form
that the server checks before processing any request.

---

## 6. Security Misconfiguration

### What it is
When a server or application is not configured securely
leaving default settings or sensitive information exposed.

### Common examples
- Default credentials not changed (admin/admin)
- Directory listing enabled — anyone can browse server files
- Verbose error messages revealing file paths and software versions
- Unnecessary services running and exposed to the internet
- Missing security headers in HTTP responses

### Real world impact
- Attacker logs in with default credentials
- Attacker browses server files and finds sensitive documents
- Error messages reveal database type and software versions to attacker

### How to fix it
Follow CIS Benchmarks for server hardening.
Disable everything you do not need.
Change all default credentials immediately after installation.

---

## How These Attacks Connect to Your Labs

| Attack | Where to Practice |
|--------|------------------|
| SQL Injection | PortSwigger SQLi labs |
| XSS | PortSwigger XSS labs |
| IDOR | PortSwigger Access Control labs |
| Broken Authentication | PortSwigger Authentication labs |
| CSRF | PortSwigger CSRF labs |
| Misconfiguration | PortSwigger Information Disclosure labs |

## Summary — Key Points to Remember
- SQL injection attacks the database through user input
- XSS injects JavaScript that runs in other users browsers
- IDOR changes a number in a URL to access someone elses data
- Broken Authentication exploits weak login and session handling
- CSRF tricks a logged in user into making unwanted requests
- Security Misconfiguration means default settings left unchanged
- All of these are in the OWASP Top 10
- PortSwigger Web Security Academy has free labs for every one
