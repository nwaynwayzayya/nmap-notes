# 🚀 Nmap Advanced Port Scans Notes

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue) ![Category](https://img.shields.io/badge/Category-Network%20Scanning-green) ![Level](https://img.shields.io/badge/Level-Intermediate--Advanced-red) ![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

---

## 📌 Overview

These notes cover advanced Nmap port scanning techniques and flags used to gain deeper insights into network services and scan behavior.

**Goal:** Perform efficient, stealthy, and informative scans while understanding how Nmap interprets results.

---

## 🎯 Learning Objectives

* Understand advanced scanning techniques and their behavior
* Interpret scan results using reasoning flags
* Control verbosity and debugging for deeper analysis
* Optimize scans for stealth and information gathering

---

## 📑 Table of Contents

* [SYN Stealth Scan (-sS)](#syn-stealth-scan--ss)
* [Understanding Scan Results](#understanding-scan-results)
* [Reason Flag (--reason)](#reason-flag---reason)
* [Verbosity Levels (-v / -vv)](#verbosity-levels--v--vv)
* [Debugging Output (-d / -dd)](#debugging-output--d--dd)
* [Tips & Best Practices](#tips--best-practices)
* [Quick Reference Commands](#quick-reference-commands)

---

## SYN Stealth Scan (-sS)

### What it is

SYN scan (half-open scan) sends a SYN packet and analyzes the response without completing the full TCP handshake.

### Nmap command

```bash
nmap -sS <target>
```

Example

```bash
sudo nmap -sS 10.10.252.27
```

### Notes

* Known as **stealth scan** because it avoids full connection
* Faster and less detectable than full TCP connect scans
* Requires root privileges
* Common responses:

  * **SYN-ACK → port is open**
  * **RST → port is closed**

---

## Understanding Scan Results

Nmap determines port states based on responses from the target.

### Common States

| State    | Meaning                                       |
| -------- | --------------------------------------------- |
| Open     | Service is actively accepting connections     |
| Closed   | Port is reachable but no service is listening |
| Filtered | Packet filtered (firewall/IDS interference)   |

### Example Insight

* Open ports return **SYN-ACK packets**
* Closed ports return **RST packets**
* Large numbers of resets indicate closed ports 

---

## Reason Flag (--reason)

### What it is

Displays the exact reason why Nmap classified a host or port in a certain state.

### Command

```bash
nmap --reason <scan_type> <target>
```

Example

```bash
sudo nmap -sS --reason 10.10.252.27
```

### Notes

* Shows packet-level reasoning (e.g., `syn-ack`, `arp-response`)
* Helps understand how Nmap reaches conclusions
* Useful for debugging and learning

Example output insight:

* Host is up → **received arp-response**
* Port is open → **received syn-ack** 

---

## Verbosity Levels (-v / -vv)

### What it is

Controls how much information Nmap prints during a scan.

### Commands

```bash
nmap -v <target>
nmap -vv <target>
```

Example

```bash
sudo nmap -sS -vv 10.10.252.27
```

### Notes

* `-v` → more details
* `-vv` → very detailed (shows scan progress and discovered ports)
* Useful for tracking long scans in real-time

---

## Debugging Output (-d / -dd)

### What it is

Provides deep debugging information about scan execution.

### Commands

```bash
nmap -d <target>
nmap -dd <target>
```

### Notes

* Shows internal workings of Nmap
* Extremely verbose output
* Can exceed one screen easily 
* Best used for troubleshooting or advanced learning

---

## Tips & Best Practices

* Use **SYN scans (-sS)** for stealth and speed
* Add `--reason` to understand why ports are classified
* Use `-vv` when you need live feedback during scans
* Avoid excessive verbosity in production environments (can be noisy)
* Combine flags (e.g., `-sS -vv --reason`) for deeper insights
* Use debugging (`-d`) only when necessary due to output size

---

## Quick Reference Commands

| Purpose              | Command                           |
| -------------------- | --------------------------------- |
| SYN stealth scan     | `sudo nmap -sS <target>`          |
| SYN scan with reason | `sudo nmap -sS --reason <target>` |
| Verbose scan         | `nmap -v <target>`                |
| Very verbose scan    | `nmap -vv <target>`               |
| Debug mode           | `nmap -d <target>`                |
| Advanced debug       | `nmap -dd <target>`               |

---

