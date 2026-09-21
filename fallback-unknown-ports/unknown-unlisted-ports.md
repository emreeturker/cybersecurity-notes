# Fallback Methodology — Unknown/Unlisted Ports

## When to Use

The general approach to follow when a known/documented vulnerability (like those in the categories above) can't be found among the ports scanned on a target.

## Method Logic

If you don't know a ready-made exploit for a port/service, match its version — detected via nmap's service version detection — against CVE databases; if a suitable script or exploit module exists, try it.

## Steps

1. `nmap [target IP] -A -p- -T5` → Extracts all ports and service versions
2. When an unknown/suspicious port is found, re-target that service: `nmap [target IP] -p [port] -sV` → Confirms the version
3. Search the web for an exploit/script using the service name + version (e.g. "distccd 4.2.4 exploit", "[service name] [version] nmap script", "[service name] [version] CVE") — reliable sources: **nmap.org** (official NSE script documentation, gives the script's full name and usage example), **exploit-db.com**, **Rapid7/Metasploit module database**
4. Confirm whether the script you found is actually installed on Kali, and get its exact filename: `locate [script name]` or `find / -iname "*[service name]*" 2>/dev/null`
5. Run the script: `nmap -p [port] [target IP] --script [script name] --script-args="[script name].cmd='id'"`
   - If no script is found, search Metasploit instead with `search [service name]`
6. The script/exploit runs a command directly on the target and prints the output (can be verified with commands like `id`, `uname -a`, `ls`) → these are the commands written into the `cmd` field

## Real-World Applications of This Method

- [distcc (Port 3632)](./distcc-port-3632.md)
