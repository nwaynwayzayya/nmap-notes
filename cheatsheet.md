# Nmap Host Discovery Cheatsheet

A compact reference for Nmap host-discovery options. Use these commands to determine which hosts are up before running port/service scans.

| Purpose | Command | Notes |
| --- | --- | --- |
| Ping scan (no port scan) | `nmap -sn <target>` | Default host discovery (ICMP/TCP/ARP depending on privileges and network) |
| ARP scan (local LAN) | `nmap -PR -sn <target>` | Fast and reliable on the same L2 subnet; returns MAC addresses |
| ICMP Echo (ping) | `nmap -PE -sn <target>` | Explicit ICMP echo requests (may be blocked by firewalls) |
| ICMP Timestamp | `nmap -PP -sn <target>` | Uses ICMP type 13; alternative if echo is filtered |
| ICMP Address Mask | `nmap -PM -sn <target>` | Uses ICMP type 17; often blocked on modern networks |
| Disable DNS resolution | `nmap -n <target>` | Avoids reverse DNS lookups to speed scans and reduce noise |
| Force DNS resolution | `nmap -R <target>` | Always resolve hostnames (can be slower) |

## Examples

```bash
# Ping-scan an IPv4 subnet
sudo nmap -sn 10.10.0.0/24

# ARP-scan the local subnet (very fast, LAN only)
sudo nmap -PR -sn 10.10.0.0/24

# Force ICMP timestamp scan
sudo nmap -PP -sn 10.10.0.0/24

# Disable reverse DNS lookups for speed
nmap -n 10.10.0.0/24
```

## Quick tips

- Use ARP scans for local discovery whenever possible - they are the most accurate on LANs.
- Combine methods if results are sparse (ARP + ICMP + TCP probes).
- Run scans with appropriate authorization and be aware of IDS/IPS alerts.

---
