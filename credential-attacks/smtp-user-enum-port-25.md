# SMTP User Enumeration (Port 25)

SMTP's `VRFY` command is a legitimate feature designed for server administrators to check whether a username exists on the system. However, if left unrestricted, an attacker can supply a wordlist of usernames and query the server one by one — the server responds with "exists" or "doesn't exist" for each. This isn't a direct access attack; the goal isn't to crack a password, but to **discover which usernames actually exist**. This information can then be used in a follow-up attack like SSH Brute-Force, focusing password-cracking attempts on verified usernames instead of random guesses.

## Why SMTP Enumeration Before SSH Brute-Force?

Feeding a generic wordlist (like `common.txt`) directly into SSH is inefficient and noisy — most of the 478 usernames likely don't exist on the system at all, wasting attempts.

SMTP's `VRFY` command tests the same list much faster (within seconds) and reveals which usernames genuinely exist. Feeding this verified, smaller list into SSH brute-force gives you:

- **Faster** — only a handful of verified usernames are tried instead of 478
- **Stealthier** — far fewer failed login attempts are logged on the SSH server, reducing detection risk
- **More reliable** — the attack targets "confirmed to exist" accounts instead of "might exist" guesses

**Summary:** Gather information first using the cheap/fast/quiet method (SMTP), then apply the expensive/slow/noisy attack (SSH brute-force) only to verified targets — a core principle of real pentest methodology.

## Discovery

| Command | Description |
|---|---|
| `nmap -A -p- -T5 [target IP]` | Detects SMTP (port 25) as open, typically revealing a service like Postfix smtpd |

## Attack

| Command | Description |
|---|---|
| `smtp-user-enum -M VRFY -U /usr/share/wordlists/fern-wifi/common.txt -t [target IP]` | Queries the server with each username in the specified wordlist via the VRFY command, listing the valid ones |

## Verification

*(To be filled in during practice — the tool's own output likely already shows the result, a separate verification step may not be necessary)*

**Next step:** The usernames discovered here can be used directly as the `USER_FILE` in an SSH Brute-Force attack.
