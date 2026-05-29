# Nmap Enumeration Guide ![Beginner](https://img.shields.io/badge/LEVEL%201-Beginner-brightgreen)

A curated collection of Nmap command-line examples and best practices. This repository serves as a functional reference for common network reconnaissance tasks, including TCP/UDP scanning, timing templates, and output parsing, aimed at optimizing your workflow during security assessments.

## Table of Contents
1. [Installation](#installation)
2. [Basic Scans](#basic-scans)
3. [Port Scanning](#port-scanning)
4. [Service Detection](#service-detection)
5. [OS Detection](#os-detection)
6. [Advanced Techniques](#advanced-techniques)

## Installation
```bash
# Ubuntu/Debian
sudo apt-get install nmap

# macOS
brew install nmap
```
## Basic Scans

### Ping Scan
Discovers live hosts without port scanning.
```bash
nmap -sn 192.168.1.0/24
```
- `-sn`: Ping scan only (no port scan)

### Simple Host Scan
```bash
nmap 192.168.1.1
```
Scans top 1000 most common ports.

## Port Scanning

### TCP Connect Scan
```bash
nmap -sT 192.168.1.1
```
- Completes full TCP handshake
- Slower but more reliable
- Logs appear in system logs

### SYN Scan (Stealth)
```bash
nmap -sS 192.168.1.1
```
- Doesn't complete handshake
- Faster, less likely to be logged
- Requires root/sudo

### UDP Scan
```bash
nmap -sU 192.168.1.1
```
- Scans UDP ports
- Slower than TCP

## Service Detection

### Version Detection
```bash
nmap -sV 192.168.1.1
```
Identifies service versions running on open ports.

### Script Scanning
```bash
nmap -sC 192.168.1.1
```
Runs default nmap scripts for more info.

## OS Detection

```bash
nmap -O 192.168.1.1
```
Attempts to identify the operating system.

## Advanced Techniques

### Combined Scan
```bash
nmap -sS -sV -O -sC 192.168.1.1
```
- `-sS`: SYN scan
- `-sV`: Service version detection
- `-O`: OS detection
- `-sC`: Script scanning

### Scan Specific Ports
```bash
nmap -p 22,80,443 192.168.1.1
```

### Aggressive Scan
```bash
nmap -A 192.168.1.1
```
Includes OS detection, version detection, script scanning, and traceroute.

### Output to File
```bash
nmap -oN output.txt 192.168.1.1
```

## Common Flags
| Flag | Purpose |
|------|---------|
| `-p` | Specify ports |
| `-sS` | SYN scan |
| `-sT` | TCP connect |
| `-sU` | UDP scan |
| `-sV` | Version detection |
| `-O` | OS detection |
| `-sC` | Script scan |
| `-A` | Aggressive scan |
| `-oN` | Output to file |

## Resources
- [Nmap Official Docs](https://nmap.org/book/)
- [Nmap Cheat Sheet](#)

---
*Last updated: May 2026*
