# Firewall Rules Documentation

## Overview
These iptables rules secure the Pi-hole DNS server on Oracle Cloud.
The policy follows the principle of default deny — all traffic is
blocked unless explicitly permitted.

## Default Policies
| Chain | Policy |
|---|---|
| INPUT | DROP |
| OUTPUT | ACCEPT |
| FORWARD | ACCEPT |

## INPUT Rules

| Rule | Protocol | Port | Purpose |
|---|---|---|---|
| 1 | ALL | ANY | Allow established/related connections |
| 2 | ALL | ANY | Allow loopback interface |
| 3 | TCP | 22 | Allow SSH access |
| 4 | UDP | 53 | Allow DNS queries |
| 5 | TCP | 53 | Allow DNS queries (TCP) |
| 6 | TCP | 80 | Allow Pi-hole web interface |

## Security Rationale

**Why default DROP on INPUT?**
Any port not explicitly opened is invisible to port scanners and
attack tools. This dramatically reduces the attack surface of the
server.

**Why allow ESTABLISHED,RELATED first?**
Without this rule the server cannot receive replies to its own
outbound connections — package updates, upstream DNS forwarding,
and gravity list downloads would all fail.

**Why allow loopback?**
Pi-hole's web interface and DNS service communicate internally
through the loopback interface. Blocking it would break Pi-hole.

## Commands Used
```bash
sudo iptables -I 4 INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -I 5 INPUT -p udp --dport 53 -j ACCEPT
sudo iptables -I 6 INPUT -p tcp --dport 53 -j ACCEPT
sudo iptables -I 7 INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -P INPUT DROP
```

## Important Note
Oracle Cloud Ubuntu instances come with pre-configured iptables rules
including a default REJECT rule at the end of the INPUT chain. New rules
must be inserted at specific positions using -I INPUT [position] to ensure
they appear above the REJECT rule. Adding rules with -A (append) would
place them after the REJECT rule making them ineffective.

This is why rule ordering in iptables is critical — rules are processed
top to bottom and the first match wins.
