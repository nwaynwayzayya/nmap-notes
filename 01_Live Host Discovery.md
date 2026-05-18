# 🛰️ Nmap Host Discovery Notes

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue) ![Category](https://img.shields.io/badge/Category-Network%20Scanning-green) ![Level](https://img.shields.io/badge/Level-Beginner--Intermediate-orange) ![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

---

## 📌 Overview

These notes cover host discovery techniques using Nmap, focused on identifying live systems before running port scans.

**Goal:** Identify live hosts efficiently before enumeration to reduce scan time, noise, and detection risk.

## 🎯 Learning Objectives

- Identify live hosts on a network
- Understand common discovery techniques and when to use them
- Use multiple methods to improve coverage and reliability

---

## 📑 Table of Contents

- [ARP Scans (local)](#arp-scans-local)
- [arp-scan tool](#arp-scan-tool)
- [ICMP: Echo / Timestamp / Mask](#icmp-echo-timestamp-mask)
- [DNS and Reverse Lookups](#dns-and-reverse-lookups)
- [Tips & Best Practices](#tips--best-practices)
- [Quick Reference Commands](#quick-reference-commands)

---

## ARP Scans (local)

### What it is
ARP scanning uses ARP requests to find hosts on the same L2 subnet. It is reliable and fast, but only works on local networks.

### Nmap command
```bash
nmap -PR -sn <target>
```

Example
```bash
sudo nmap -PR -sn 10.10.210.0/24
```

Notes
- Works only on the same broadcast domain (local subnet)
- Returns MAC addresses and vendor info
- Fast and accurate for internal network discovery

## arp-scan tool

### What it is
`arp-scan` is a dedicated ARP discovery utility that sends ARP requests across a network.

### Install
```bash
sudo apt install arp-scan
```

### Usage
```bash
sudo arp-scan -l                     # scan local network
sudo arp-scan -I eth0 10.10.210.0/24 # specify interface
```

Notes
- Often returns similar results to Nmap ARP scans but can be faster for some tasks

## ICMP: Echo / Timestamp / Address Mask

ICMP-based scans send different ICMP types to elicit responses when TCP/UDP probes are filtered.

### ICMP Echo (ping)
```bash
nmap -PE -sn <target>
```
Example
```bash
sudo nmap -PE -sn 10.10.68.0/24
```
Notes: simple and common, but frequently blocked by host- or network-level firewalls.

### ICMP Timestamp
```bash
nmap -PP -sn <target>
```
Example
```bash
sudo nmap -PP -sn 10.10.68.0/24
```
Notes: uses ICMP type 13; sometimes bypasses rules that block echo requests.

### ICMP Address Mask
```bash
nmap -PM -sn <target>
```
Example
```bash
sudo nmap -PM -sn 10.10.68.0/24
```
Notes: uses ICMP type 17; often blocked and less reliable in modern networks.

## DNS and Reverse Lookups

By default, Nmap will attempt to resolve hostnames. You can control DNS behavior:

- Disable DNS resolution: `nmap -n`
- Force reverse-DNS resolution: `nmap -R`
- Use custom DNS servers: `nmap --dns-servers <dns1[,dns2]>`

## Tips & Best Practices

- Use ARP scans for local subnet discovery - they are the most reliable on LANs.
- Combine multiple techniques (ARP + ICMP + TCP/UDP probes) to maximize discovery.
- Be mindful of firewalls and IDS/IPS; ICMP and some probes may be blocked or generate alerts.
- Use `-sn` (ping scan) to discover hosts without doing port scans.
- When scanning remote networks, avoid ARP and prefer ICMP/TCP methods.

## Quick Reference Commands

| Purpose | Command |
| --- | --- |
| Nmap ping (ICMP echo) | `sudo nmap -PE -sn 10.10.0.0/24` |
| Nmap ARP (local) | `sudo nmap -PR -sn 10.10.0.0/24` |
| Nmap timestamp | `sudo nmap -PP -sn 10.10.0.0/24` |
| Nmap address mask | `sudo nmap -PM -sn 10.10.0.0/24` |
| Disable DNS | `nmap -n <target>` |
| Force DNS lookup | `nmap -R <target>` |
| Use custom DNS servers | `nmap --dns-servers 8.8.8.8,1.1.1.1 <target>` |

---


