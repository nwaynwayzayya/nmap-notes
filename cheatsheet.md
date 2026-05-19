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

## Port Scanning Cheatsheet

A compact reference for common Nmap port-scanning commands and useful flags.

| Purpose | Command | Notes |
| --- | --- | --- |
| TCP SYN (stealth) | `sudo nmap -sS <target>` | Fast; requires root/sudo |
| TCP connect | `nmap -sT <target>` | Default for unprivileged users |
| UDP scan | `sudo nmap -sU <target>` | Slow; results often `open|filtered` |
| Scan specific ports | `nmap -p22,80,443 <target>` | Scan listed ports only |
| Scan all ports | `nmap -p- <target>` | Enumerate all 65535 ports |
| Top common ports | `nmap --top-ports 100 <target>` | Scan most likely ports quickly |
| Verbosity | `-v` / `-vv` | More runtime detail/progress |
| Reason | `--reason` | Show why Nmap classified results |
| Debug | `-d` / `-dd` | Very verbose internal debug output |
| Timing templates | `-T1` ... `-T5` | Stealthy (`-T1`) → aggressive (`-T5`)

## Examples

```bash
sudo nmap -sS --reason -vv 10.10.252.27
nmap -p22,80,443 --top-ports 100 10.10.252.27
sudo nmap -sU -p53 10.10.252.27
```

## Quick tips

- Combine `-sS -vv --reason` for informative, stealthy scans.
- Use `-d` only when debugging scan behavior — output is very large.
- Always scan only networks you are authorized to test.

## Post-Port-Scan Cheatsheet

| Purpose | Command | Notes |
| --- | --- | --- |
| Service/version detection | `nmap -sV <target>` | Fingerprint services and versions |
| OS detection | `nmap -O <target>` | TCP/IP stack fingerprinting (requires privileges) |
| Aggressive scan | `nmap -A <target>` | Combines `-sV -O -sC` and traceroute; noisy |
| Default NSE scripts | `nmap -sC <target>` | Run safe, common scripts |
| Run specific NSE script/category | `nmap --script <script-name> <target>` | e.g., `--script http-enum` or `--script vuln` |
| Quick vuln checks | `nmap --script vuln <target>` | Rapid triage for known issues |
| Traceroute | `nmap --traceroute <target>` | Map network path to target |

## Examples

```bash
nmap -sV --script=vuln 10.10.252.27
nmap -A 10.10.252.27
nmap -sC --script=default 10.10.252.27
```

## Quick tips

- Use `--script-help <script>` to inspect script options before running.
- Prefer targeted NSE scripts for enumeration; avoid `vuln` or `intrusive` categories on production without permission.
- Combine `-sV` with `--script` to gather version-aware script results.

## Output Formats (quick reference)

| Format | Command | Use |
| --- | --- | --- |
| Normal | `-oN scan.nmap` | Human-readable output (same as terminal) |
| Grepable | `-oG scan.gnmap` | Line-oriented for `grep`/quick filtering |
| XML | `-oX scan.xml` | Structured output for automation/tools |
| All formats | `-oA scan` | Saves `.nmap`, `.gnmap`, and `.xml` |
| Script-kiddie | `-oS scan.kiddie` | Stylized/obfuscated output (not recommended) |

## Example

```bash
nmap -sV --script=vuln -oA scan 10.10.252.27
grep http scan.gnmap
```
