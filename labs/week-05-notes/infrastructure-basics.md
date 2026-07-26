# Infrastructure Basics - Week 5

## What is Web Infrastructure?
Web infrastructure is everything that sits behind a website.
When you visit a webpage you only see the frontend.
Behind it is a stack of technologies working together.

## The Web Stack — How It All Connects

Browser → Web Server → Application → Database → Back to Browser

### Step by step
1. Your browser sends an HTTP request
2. The web server receives it (Apache or Nginx)
3. The web server passes it to the application (PHP, Python, Node.js)
4. The application queries the database (MySQL, PostgreSQL)
5. The database returns data to the application
6. The application builds a response
7. The web server sends it back to your browser

## Web Servers

### What a web server does
Receives HTTP requests and sends back responses.
Serves static files (HTML, CSS, images) directly.
Passes dynamic requests to the application behind it.

### Apache
- Most widely used web server in the world
- Config file location: /etc/apache2/apache2.conf
- Site config files: /etc/apache2/sites-available/
- Logs location: /var/log/apache2/
- Start it: sudo systemctl start apache2
- Check status: sudo systemctl status apache2

### Nginx
- Faster than Apache for serving static files
- Often used as a reverse proxy in front of other servers
- Config file location: /etc/nginx/nginx.conf
- Site config files: /etc/nginx/sites-available/
- Logs location: /var/log/nginx/
- Start it: sudo systemctl start nginx

### Why web servers matter for hacking
- Config files reveal directory structure and permissions
- Log files record every request — useful for forensics
- Misconfigured web servers expose sensitive directories
- Version information in headers reveals exploitable vulnerabilities

## Databases

### What a database does
Stores all the dynamic data behind a website.
Users, passwords, posts, orders, settings — all in the database.
The application queries the database using SQL.

### MySQL
- Most common database for web applications
- Default port: 3306
- Connect: mysql -u root -p
- Show databases: SHOW DATABASES;
- Use a database: USE databasename;
- Show tables: SHOW TABLES;

### PostgreSQL
- More advanced than MySQL
- Default port: 5432
- Often used with Python applications

### Why databases matter for hacking
- SQL injection attacks the database directly through the application
- Databases store password hashes — cracking them gives account access
- Default MySQL credentials (root with no password) are a common finding
- Exposed port 3306 with no firewall = direct database access from internet

## Application Languages

### PHP
- Most common language for web applications
- Files end in .php
- Runs on the server — user never sees PHP code
- Common vulnerabilities: file inclusion, SQL injection, code execution

### Python
- Used with frameworks like Django and Flask
- Growing in popularity for web applications

### Node.js
- JavaScript running on the server
- Used for real-time applications

### Why application languages matter for hacking
- Each language has common vulnerability patterns
- PHP file inclusion vulnerabilities let attackers read any file
- Knowing the language helps you craft the right attack payload

## Common Ports for Infrastructure

| Port | Service | Why attackers care |
|------|---------|-------------------|
| 80 | HTTP | Unencrypted web traffic |
| 443 | HTTPS | Encrypted web traffic |
| 3306 | MySQL | Direct database access if exposed |
| 5432 | PostgreSQL | Direct database access if exposed |
| 8080 | HTTP alternate | Development servers, admin panels |
| 8443 | HTTPS alternate | Alternative secure web port |

## Linux File Structure for Web Servers

/var/www/html/ — default web root (where website files live)
/etc/apache2/ — Apache configuration files
/etc/nginx/ — Nginx configuration files
/var/log/apache2/ — Apache log files
/var/log/nginx/ — Nginx log files
/tmp/ — temporary files (sometimes writable by attackers)

## What Attackers Look For in Infrastructure

### Information gathering
- What web server is running and what version
- What application language is being used
- What database is behind the application
- What operating system the server runs

### Common findings
- Default credentials on database or admin panel
- Exposed database port accessible from internet
- Verbose error messages revealing file paths
- Old software versions with known CVEs
- Backup files left on the server (.bak, .old, .zip)

## Connecting Infrastructure to Attacks

| Infrastructure Component | Related Attack |
|--------------------------|---------------|
| Database (MySQL) | SQL Injection |
| Web Server (Apache) | Directory traversal, misconfig |
| Application (PHP) | File inclusion, code execution |
| Session handling | Broken Authentication |
| Input processing | XSS, CSRF |

## Summary — Key Points to Remember
- Web stack order: Browser → Web Server → Application → Database
- Apache and Nginx are the two main web servers
- MySQL default port is 3306 — should never be exposed to internet
- Config files live in /etc/ — log files live in /var/log/
- Web root where site files live is usually /var/www/html/
- Every component of the stack is a potential attack surface
- Version information from headers helps attackers find known CVEs
