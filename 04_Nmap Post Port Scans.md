# 🔎 Nmap Scripting Engine (NSE) & Output Formats

![Nmap](https://img.shields.io/badge/Tool-Nmap-blue) ![Category](https://img.shields.io/badge/Category-Network%20Scanning-green) ![Level](https://img.shields.io/badge/Level-Intermediate-yellow) ![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

---

## 📌 Overview

This note covers the Nmap Scripting Engine (NSE) for automation and common output formats (`-oN`, `-oG`, `-oX`, `-oA`) used for saving scan results.

---

## 🎯 Learning Objectives

* Understand NSE purpose and script categories
* Run scripts safely and by pattern
* Save and parse Nmap output in common formats

---

## 📑 Table of Contents

* [NSE Overview](#nse-overview)
* [Common Scripts](#common-scripts)
* [Running Scripts](#running-scripts)
* [Script Categories](#script-categories)
* [Output Formats](#output-formats)
* [Examples](#examples)
* [Tips & Best Practices](#tips--best-practices)
* [Quick Reference Commands](#quick-reference-commands)

---

## NSE Overview

NSE allows automation of tasks such as service detection, vulnerability scanning, brute-force checks, and information gathering. Scripts are written in **Lua** and can extend Nmap’s capabilities considerably.

---

## Common Scripts

Examples of useful built-in scripts:

- `http-enum`
- `http-wordpress-brute`
- `http-wordpress-users`
- `http-vuln-*`
- `ftp-brute`
- `http-waf-detect`
- `http-methods`

You can use existing scripts, write your own, or download community scripts (exercise caution with untrusted sources).

---

## Running Scripts

### Default scripts

```bash
nmap -sC <target>
```

or

```bash
nmap --script=default <target>
```

Runs a safe set of commonly useful scripts.

### Run a specific script

```bash
nmap --script=<script-name> <target>
```

Example:

```bash
nmap --script=http-date <target>
```

### Run scripts by pattern

```bash
nmap --script="ftp*" <target>
```

Runs all scripts matching the pattern.

Use `--script-help <script>` to view script options.

---

## Script Categories

| Category  | Description                                 |
| --------- | ------------------------------------------- |
| auth      | Authentication-related scripts              |
| broadcast | Discover hosts via broadcast                |
| brute     | Brute-force login attempts                  |
| default   | Default safe scripts (`-sC`)                |
| discovery | Gather info (DNS, databases, etc.)          |
| dos       | Detect DoS vulnerabilities                  |
| exploit   | Attempt exploitation                        |
| external  | Use third-party services (e.g., VirusTotal) |
| fuzzer    | Fuzzing attacks                             |
| intrusive | Aggressive scripts (brute-force, exploit)   |
| malware   | Detect backdoors                            |
| safe      | Non-intrusive scripts                       |
| version   | Service version detection                   |
| vuln      | Vulnerability scanning                      |

Some scripts belong to multiple categories; `intrusive` and `vuln` may be noisy or destructive.

---

## Output Formats

Saving scan results is important for documentation, analysis, and automation. Common output options:

### Normal format (`-oN`)

```bash
nmap -oN scan.nmap <target>
```

Human-readable; mirrors terminal output.

### Grepable format (`-oG`)

```bash
nmap -oG scan.gnmap <target>
```

Compact, line-oriented format optimized for `grep` and quick parsing.

### XML format (`-oX`)

```bash
nmap -oX scan.xml <target>
```

Structured XML useful for automation and tool integration.

### All formats (`-oA`)

```bash
nmap -oA scan <target>
```

Creates `.nmap`, `.gnmap`, and `.xml` files with the given base name.

### Script-kiddie format (`-oS`)

```bash
nmap -oS scan.kiddie <target>
```

Stylized/obfuscated output — not recommended for analysis.

---

## Examples

```bash
sudo nmap -sS -sC -oA scan 10.10.252.27
nmap -sV --script=vuln -oX results.xml 10.10.252.27
grep http scan.gnmap
```

---

## Tips & Best Practices

- Use `-sC` for safe, quick script-based enumeration; prefer `safe`/`default` categories for discovery.
- Inspect scripts before running with `--script-help <script>` and test scripts on non-production hosts.
- Combine `-sV` with targeted NSE scripts to get version-aware results.
- Save full outputs with `-oA` for records and use `-oG` for fast grepping and filtering.
- Avoid running `intrusive` or `vuln` scripts against production systems without explicit permission.
- Be cautious with third-party scripts — they may be unmaintained or malicious.

---

## Quick Reference Commands

| Purpose | Command |
| --- | --- |
| Default scripts | `nmap -sC <target>` |
| Specific script | `nmap --script=<script-name> <target>` |
| Script by pattern | `nmap --script="pattern*" <target>` |
| Service/version detection | `nmap -sV <target>` |
| Save all formats | `nmap -oA scan <target>` |
| Save grepable output | `nmap -oG scan.gnmap` |
| Save XML output | `nmap -oX scan.xml` |
| Quick vuln checks | `nmap --script vuln <target>` (use with caution) |
| Traceroute | `nmap --traceroute <target>` |


---




