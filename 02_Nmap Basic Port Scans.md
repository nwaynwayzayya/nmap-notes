# 🛰️ Nmap Basic Port Scanning Notes

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue) ![Category](https://img.shields.io/badge/Category-Network%20Scanning-green) ![Level](https://img.shields.io/badge/Level-Beginner--Intermediate-orange) ![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

---

## 📌 Overview

These notes cover **basic port scanning techniques using Nmap**, focusing on identifying open, closed, and filtered ports on target systems.

**Goal:** Discover accessible services and understand how different scan types behave under various network conditions.

---

## 🎯 Learning Objectives

* Understand different Nmap port scanning techniques
* Identify port states (open, closed, filtered)
* Learn when to use TCP vs UDP scans
* Optimize scans for speed, stealth, and accuracy

---

## 📑 Table of Contents

* [Port States](#port-states)
* [TCP Connect Scan (-sT)](#tcp-connect-scan--st)
* [TCP SYN Scan (-sS)](#tcp-syn-scan--ss)
* [UDP Scan (-sU)](#udp-scan--su)
* [TCP Flags Basics](#tcp-flags-basics)
* [Port Selection Options](#port-selection-options)
* [Timing & Performance Tuning](#timing--performance-tuning)
* [Tips & Best Practices](#tips--best-practices)
* [Quick Reference Commands](#quick-reference-commands)

---

## Port States

Nmap classifies ports into different states:

* **Open** → service is actively accepting connections
* **Closed** → reachable but no service is listening
* **Filtered** → blocked by firewall or filtering rules
* **Open|Filtered** → cannot determine if open or filtered
* **Closed|Filtered** → cannot determine if closed or filtered 

---

## TCP Connect Scan (-sT)

### What it is

A full TCP connection scan that completes the **3-way handshake**.

### Command

```bash
nmap -sT <target>
```

### How it works

* Sends SYN → receives SYN/ACK → sends ACK
* Fully establishes a connection before closing it

### Notes

* Default for **unprivileged users** 
* More detectable (logs full connections)
* Reliable but slower than SYN scan

---

## TCP SYN Scan (-sS)

### What it is

A **half-open (stealth) scan** that does NOT complete the TCP handshake.

### Command

```bash
sudo nmap -sS <target>
```

### How it works

* Sends SYN
* If SYN/ACK received → sends RST (does not complete connection)
* Avoids full handshake

### Notes

* Default scan for **privileged users (root/sudo)** 
* Faster and stealthier than connect scan
* Less likely to be logged
* Very reliable for detecting open ports

---

## UDP Scan (-sU)

### What it is

Scans UDP ports, which are **connectionless** (no handshake).

### Command

```bash
sudo nmap -sU <target>
```

### How it works

* No response → port may be **open**
* ICMP port unreachable → port is **closed** 

### Notes

* Slower than TCP scans
* Results may be ambiguous (open|filtered)
* Useful for discovering services like DNS, DHCP

---

## TCP Flags Basics

To understand scan behavior, you need basic TCP flags:

* **SYN** → initiate connection
* **ACK** → acknowledge data
* **RST** → reset connection
* **FIN** → finish connection
* **PSH** → push data immediately
* **URG** → urgent data processing 

---

## Port Selection Options

Control which ports to scan:

```bash
-p22,80,443        # specific ports
-p1-1023           # range
-p-                # all 65535 ports
-F                 # fast scan (top 100 ports)
--top-ports 10     # top N ports
```

Notes

* Default scan = **top 1000 ports**
* Use `-p-` for full enumeration (slower but thorough) 

---

## Timing & Performance Tuning

### Timing Templates

```bash
-T0  # paranoid (very slow, stealthy)
-T1  # sneaky
-T2  # polite
-T3  # normal (default)
-T4  # aggressive
-T5  # insane (fast but less accurate)
```

Notes

* `-T1` → stealth (real engagements)
* `-T4` → common in labs/CTFs
* `-T5` → fast but may lose accuracy 

---

### Rate Control

```bash
--min-rate 10
--max-rate 50
```

* Controls packets per second
* Helps balance speed vs detection 

---

### Parallelism

```bash
--min-parallelism 100
```

* Controls number of probes sent in parallel
* Higher = faster but noisier 

---

## Tips & Best Practices

* Use **SYN scan (-sS)** whenever possible (stealth + speed)
* Use **connect scan (-sT)** if you don’t have root privileges
* Combine TCP + UDP scans for better coverage
* Start with common ports, then expand (`-p-`)
* Adjust timing based on environment (stealth vs speed)
* Be cautious: aggressive scans can trigger IDS/IPS

---

## Quick Reference Commands

| Purpose             | Command                        |
| ------------------- | ------------------------------ |
| TCP connect scan    | `nmap -sT <target>`            |
| TCP SYN scan        | `sudo nmap -sS <target>`       |
| UDP scan            | `sudo nmap -sU <target>`       |
| Scan specific ports | `nmap -p22,80,443 <target>`    |
| Scan all ports      | `nmap -p- <target>`            |
| Fast scan           | `nmap -F <target>`             |
| Top ports           | `nmap --top-ports 10 <target>` |
| Aggressive timing   | `nmap -T4 <target>`            |
| Stealth timing      | `nmap -T1 <target>`            |

---



