# Terminal & Lab Practicals — Week 3 Notes

## Nmap: First Scans
Nmap = network scanner. Tells you if a host is alive, which ports are open, and what service is running behind them. First step of recon in any pentest.

```bash
nmap 127.0.0.1
```
- Scanned my own machine (localhost). Result: all ports closed — normal for a fresh Kali install, nothing running yet.

Started SSH to see an open port in action:
```bash
sudo systemctl start ssh
nmap 127.0.0.1
```
Result:
```
PORT   STATE SERVICE
22/tcp open  ssh
```
- **22/tcp** = port number + protocol
- **open** = something is actively listening
- **ssh** = Nmap recognized the standard service on that port

Turned SSH back off afterward (good practice — don't leave open ports on a lab VM unnecessarily):
```bash
sudo systemctl stop ssh
```

**Takeaway:** turning a service on/off directly changes what Nmap sees. This is the literal mechanic behind "reconnaissance" and "attack surface" from the theory notes.

---

## Terminal Adventure Game — Level Walkthrough

### Level 1 — Basics
Commands: `ls`, `pwd`, `cat`
- Found `password.txt` in home directory, read it with `cat password.txt`.
- Used `su level2` + password to move up a level.

### Level 2 — Dashed Filenames
Files starting with `-` get misread as command flags. Fix:
```bash
cat ./-
# or
cat -- -
```
The `./` or `--` tells the shell "this is a filename, not an option."

### Level 3 — Hidden Files
Hidden files start with a dot and don't show in a plain `ls`. Use:
```bash
ls -a
```
Found `.password.txt`, read with:
```bash
cat .password.txt
```

### Level 4 — Filenames with Spaces
Spaces in filenames split into multiple arguments unless quoted:
```bash
cat "this is the password.txt"
```

### Level 5 — Searching with grep
`grep` searches inside a file for lines containing a specific word.
```bash
grep password listOfPasswords.txt
```
Returned the line containing the actual password, filtered out of a big list.

### Level 6 — Extracting Gzipped Files (no home write permission)
No write permission in home directory, so extracted to `/tmp` instead:
```bash
gunzip -c password.txt.gz > /tmp/password.txt
cat /tmp/password.txt
rm /tmp/password.txt   # cleanup after reading
```
`-c` outputs the decompressed content to stdout instead of overwriting in place, so it can be redirected elsewhere.

### Level 7 — Base64 Decoding
Password file contained a Base64-encoded string. Decoded with:
```bash
echo "bGV2ZWw4dW5oYWNrYWJsZQ==" | base64 -d
```
Other equivalent methods:
```bash
cat password.txt | base64 -d
base64 -d < password.txt
```

---

## Skills Practiced This Session
- Basic navigation (`ls`, `pwd`, `cd`, `cat`)
- Handling special/hidden/spaced filenames
- Privilege switching (`su`)
- Text searching (`grep`)
- Archive extraction (`gunzip`) with permission workarounds
- Encoding/decoding (`base64`)
- Nmap basics: host discovery, port states, service detection

**Next up:** Level 8+ of the terminal game, and continuing OverTheWire Bandit for more realistic OS-level challenges.
