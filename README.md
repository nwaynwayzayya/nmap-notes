# Nmap Notes

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue)

Concise, practical notes and cheatsheets for learning Nmap (TryHackMe exercises and hands-on practice).

## Contents

- `cheatsheet.md` - quick command reference for discovery scans
- `01_Live Host Discovery.md` - host-discovery techniques and examples
- `02_Nmap Basic Port Scans.md` - basic port scanning techniques and common flags
- `03_Nmap Advanced Port Scans.md` - advanced port-scan techniques, verbosity and debugging

## Topics Covered

- Host Discovery (ARP, ICMP, ping scans)
- Port Scanning (TCP, UDP scans, common flags)
- Service Enumeration (version detection, banner grabbing)
- Advanced Scans (stealth, timing, evasion techniques)

## Quick Start

Open the notes or run example commands with appropriate privileges. Example:

```bash
# Ping-scan an IPv4 subnet
sudo nmap -sn 10.10.0.0/24
```

## Tips

- Always scan only networks you own or are authorized to test.
- Use `-sn` to discover live hosts before port-scanning.
- Prefer ARP scans for local LAN discovery when possible.

---

_Goal: build strong fundamentals in network scanning and enumeration._