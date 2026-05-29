# Basic Nmap Scans

![Beginner](https://img.shields.io/badge/LEVEL%201-Beginner-brightgreen)

---

## Table of Contents

1. [Before You Start](#before-you-start)
2. [Understanding Port Scanning](#understanding-port-scanning)
3. [Basic Ping Scan](#basic-ping-scan)
4. [Basic Port Scan](#basic-port-scan)
5. [Service Detection](#service-detection)
6. [Operating System Detection](#operating-system-detection)
7. [Combining Scans](#combining-scans)
8. [Common Beginner Mistakes](#common-beginner-mistakes)

---

## Before You Start

### Installation

Before running any Nmap commands, you need to install Nmap on your system.

Linux (Ubuntu/Debian):
```
sudo apt-get update
sudo apt-get install nmap
```

Linux (CentOS/RHEL):
```
sudo yum install nmap
```

macOS:
```
brew install nmap
```

Windows:
Download from https://nmap.org/download.html

### Verify Installation

To confirm Nmap is installed correctly:
```
nmap --version
```

You should see output similar to:
```
Nmap version 7.80
```

### Basic Command Structure

All Nmap commands follow this basic pattern:

```
nmap [OPTIONS] [TARGET]
```

Where:
- nmap: The command to run Nmap
- [OPTIONS]: Flags that change how Nmap behaves
- [TARGET]: The host or network to scan

### Important Safety Note

Only scan networks and hosts that you own or have explicit permission to scan. Scanning without permission is illegal in most jurisdictions.

---

## Understanding Port Scanning

### What is a Port?

A port is a logical endpoint on a computer. Think of it as a channel through which a service communicates.

Ports are numbered from 0 to 65535.

Common ports you should know:

Port 20-21: FTP (File Transfer Protocol)
Purpose: Transfer files between computers
State: Usually CLOSED unless file server is running

Port 22: SSH (Secure Shell)
Purpose: Remote login to systems
State: Usually OPEN on servers and routers

Port 80: HTTP (Hypertext Transfer Protocol)
Purpose: Web browsing (insecure)
State: Usually OPEN if web server is running

Port 443: HTTPS (Secure HTTP)
Purpose: Web browsing (secure)
State: Usually OPEN if web server is running

Port 3306: MySQL
Purpose: Database access
State: Usually CLOSED except on database servers

Port 3389: RDP (Remote Desktop Protocol)
Purpose: Remote access for Windows systems
State: Usually CLOSED unless enabled

### Port States

After scanning, ports will be in one of these states:

OPEN: A service is actively listening on this port and accepting connections. You can connect to it.

CLOSED: The port is not listening, but the host acknowledged your probe. The host is up, but this service is not running.

FILTERED: The port is not accessible. Usually means a firewall is blocking it. The host did not respond to your probe.

---

## Basic Ping Scan

A ping scan is the simplest Nmap operation. It only determines which hosts are alive, without checking ports.

### What It Does

Ping scan sends a simple probe to each host in a range and checks for responses. It answers the question: "Which computers are turned on and reachable?"

### Command Syntax

```
nmap -sn TARGET
```

Where:
- -sn: Ping scan only (no port scanning)
- TARGET: Single host, or network range

### Example 1: Ping Single Host

```
nmap -sn 192.168.1.1
```

This checks if the host at 192.168.1.1 is online.

Output example:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2024-01-15 10:30 WIB
Nmap scan report for router.home (192.168.1.1)
Host is up (0.0023s latency).
Nmap done at 2024-01-15 10:30 WIB; 1 IP address (1 host up) scanned in 0.15s
```

This tells you:
- The host at 192.168.1.1 is UP (online)
- It responded in 0.0023 seconds
- Scan completed in 0.15 seconds total

### Example 2: Ping Network Range

```
nmap -sn 192.168.1.0/24
```

This scans all hosts on the 192.168.1.0 network (192.168.1.1 through 192.168.1.254).

Output example:
```
Nmap scan report for 192.168.1.1
Host is up (0.0023s latency).
Nmap scan report for 192.168.1.5
Host is up (0.0045s latency).
Nmap scan report for 192.168.1.10
Host is up (0.0031s latency).

Nmap done at 2024-01-15 10:32 WIB; 256 IP addresses (3 hosts up) scanned in 2.45s
```

This tells you:
- 3 hosts are online (up) on this network
- Other IPs did not respond (likely offline or don't exist)

### When to Use Ping Scan

Use ping scan when:
- You want to quickly find which hosts are online
- You want to map out your network
- You want to understand network topology
- You do not need detailed port information

---

## Basic Port Scan

A port scan checks which ports are open on a target host. This is more detailed than ping scan.

### What It Does

Port scanning checks each port on a target host and determines its state (OPEN, CLOSED, or FILTERED). It answers the question: "Which services are running on this computer?"

### Command Syntax

```
nmap TARGET
```

With no options, Nmap performs a default scan of the 1000 most commonly used ports.

### Example 1: Scan Default Ports on Single Host

```
nmap 192.168.1.5
```

Output example:
```
Starting Nmap 7.80 ( https://nmap.org ) at 2024-01-15 10:35 WIB
Nmap scan report for workstation.home (192.168.1.5)
Host is up (0.0031s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http

Nmap done at 2024-01-15 10:35 WIB; 1 IP address (1 host up) scanned in 0.42s
```

This tells you:
- Host 192.168.1.5 is UP
- 2 ports are OPEN: port 22 (SSH) and port 80 (HTTP)
- 998 ports are CLOSED
- Scan took 0.42 seconds

### Example 2: Scan Specific Port

```
nmap -p 22 192.168.1.5
```

This scans only port 22 on the target.

Output example:
```
PORT   STATE SERVICE
22/tcp open  ssh
```

This tells you:
- Port 22 is OPEN
- A service (SSH) is listening on this port

### Example 3: Scan Multiple Specific Ports

```
nmap -p 22,80,443 192.168.1.5
```

This scans only ports 22, 80, and 443.

Output example:
```
PORT    STATE SERVICE
22/tcp  open  ssh
80/tcp  open  http
443/tcp closed https
```

This tells you:
- Ports 22 and 80 are OPEN
- Port 443 is CLOSED (host responded but no service on this port)

### Example 4: Scan a Range of Ports

```
nmap -p 1-100 192.168.1.5
```

This scans ports 1 through 100.

Output example:
```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

This tells you:
- In the range of ports 1-100, only ports 22 and 80 are open
- All other ports in this range are closed

### Example 5: Scan All Ports

```
nmap -p- 192.168.1.5
```

The -p- flag means scan all 65535 ports.

Warning: This takes much longer than scanning default ports. Do not use unless necessary.

### Understanding Port Numbers

Different port ranges have different purposes:

Well-known ports (0-1023): Standard services
- Port 22: SSH
- Port 80: HTTP
- Port 443: HTTPS
- Reserved for system administration

Registered ports (1024-49151): Services registered with IANA
- Port 3306: MySQL
- Port 5432: PostgreSQL
- Port 8080: Alternative HTTP

Dynamic/Private ports (49152-65535): Temporary or private services
- Often used by applications
- Less predictable

### Scan Time Explanation

Nmap scans take time because:
1. It sends a probe to each port
2. It waits for a response
3. If no response, it may retry
4. For 1000 ports, this can take several seconds

Basic scans are usually fast (under 1 minute). Full scans of all 65535 ports can take 5-30 minutes depending on network speed and host responsiveness.

---

## Service Detection

Service detection identifies what application is running on each open port. This is more detailed than just knowing the port is open.

### What It Does

Service detection connects to open ports and examines the response to identify the service name and version. It answers the question: "What software is running on this open port?"

### Command Syntax

```
nmap -sV TARGET
```

Where:
- -sV: Enable service/version detection
- TARGET: Host to scan

### Example 1: Basic Service Detection

```
nmap -sV 192.168.1.5
```

Output example:
```
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp  open  http    Apache httpd 2.4.6 (CentOS)
443/tcp open  https   nginx 1.14.1
```

This tells you:
- Port 22 is running OpenSSH version 7.4
- Port 80 is running Apache HTTP server version 2.4.6 on CentOS
- Port 443 is running nginx version 1.14.1

### Example 2: Service Detection with Specific Ports

```
nmap -sV -p 22,80,443 192.168.1.5
```

This performs service detection only on ports 22, 80, and 443.

### Understanding Service Versions

Service versions are important because:

Different versions have different features
Older versions may have known security vulnerabilities
Newer versions may have patches and improvements

For example:
- OpenSSH 7.4 might have certain vulnerabilities
- OpenSSH 7.6 might have patches for those vulnerabilities
- OpenSSH 8.0 might have completely redesigned features

### What Service Detection Reveals

Common services you will see:

SSH (port 22): Remote login service
- Allows secure login to systems
- OpenSSH is the most common implementation

HTTP (port 80): Web server
- Serves web pages
- Common implementations: Apache, nginx, IIS

HTTPS (port 443): Secure web server
- Serves web pages over encrypted connection
- Same implementations as HTTP but with SSL/TLS

MySQL (port 3306): Database
- Stores and manages data
- Used by web applications

PostgreSQL (port 5432): Database
- Alternative to MySQL
- Often used in enterprise environments

FTP (port 21): File transfer service
- Transfers files between systems
- Less secure than modern alternatives

### Why Service Detection is Important

Knowing the exact service and version allows you to:
1. Check if this service should be running on this host
2. Verify the service is up to date
3. Research known vulnerabilities for this version
4. Plan remediation if needed

---

## Operating System Detection

OS detection attempts to identify the operating system running on the target host.

### What It Does

OS detection analyzes network responses to determine what operating system is running. It answers the question: "Is this Windows, Linux, macOS, or something else?"

### Command Syntax

```
nmap -O TARGET
```

Where:
- -O: Enable OS detection
- TARGET: Host to scan

Note: OS detection may require root/sudo privileges on some systems.

### Example 1: Basic OS Detection

```
sudo nmap -O 192.168.1.5
```

Output example:
```
Running: Linux 4.x
OS CPE: cpe:/o:linux:linux_kernel:4
OS details: Linux 4.15 - 5.6
```

This tells you:
- The target is running Linux
- Specifically Linux kernel version 4.x
- Likely running a modern Linux distribution

### Example 2: OS Detection with Service Detection

```
sudo nmap -O -sV 192.168.1.5
```

This provides both OS and service information together.

Output example:
```
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4
80/tcp  open  http    Apache httpd 2.4.6

Running: Linux 4.x
OS details: Linux 4.15 - 5.6, CentOS 7.x - CentOS 8.x
```

This tells you:
- The services running on the host
- The operating system and version
- Likely Linux distribution

### Common Operating Systems You Will See

Linux: Open-source operating system
- Common on servers
- Examples: Ubuntu, CentOS, Debian, Red Hat
- Nmap shows: "Linux X.x"

Windows: Microsoft operating system
- Common on desktop and some servers
- Examples: Windows 10, Windows Server 2019
- Nmap shows: "Windows X (build XXXX)"

macOS: Apple operating system
- Common on Apple computers
- Nmap shows: "Mac OS X X.x"

### OS Detection Accuracy

OS detection is probabilistic and may not always be 100% accurate:

High confidence (>95%): Usually accurate
Medium confidence (70-95%): Likely correct but not guaranteed
Low confidence (<70%): Take with caution

Nmap provides confidence percentages in detailed output.

### Why OS Detection Matters

Knowing the operating system is important because:

Different OS have different vulnerabilities
Exploitation techniques differ between OS
Patch management differs between OS
Configuration and hardening practices differ

For example:
- A vulnerability in Linux kernel 4.x is irrelevant to Windows
- A Windows RDP vulnerability is irrelevant to Linux systems
- Update procedures differ significantly between operating systems

---

## Combining Scans

When you need comprehensive information about a target, you can combine multiple scan options into a single command.

### Option 1: Combining Service and OS Detection

```
nmap -sV -O 192.168.1.5
```

This provides both service detection and OS detection.

Output example:
```
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4
80/tcp  open  http    Apache httpd 2.4.6
443/tcp open  https   nginx 1.14.1

Running: Linux 4.x
OS details: Linux 4.15 - 5.6, CentOS 7.x
```

### Option 2: Aggressive Scan

```
nmap -A 192.168.1.5
```

The -A flag combines multiple options:
- OS detection (-O)
- Service/version detection (-sV)
- Script scanning (basic scripts)
- Traceroute

This provides very comprehensive information but takes longer.

Output example:
```
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
80/tcp  open  http    Apache httpd 2.4.6 (CentOS)
443/tcp open  https   nginx 1.14.1

Running: Linux 4.x
OS details: Linux 4.15 - 5.6, CentOS 7.x - CentOS 8.x

NSE scripts may have additional information below
```

### Option 3: Specific Ports with Full Detection

```
nmap -sV -O -p 22,80,443 192.168.1.5
```

This scans only specific ports but performs both service and OS detection on those ports.

### Option 4: Saving Output to File

```
nmap -sV -O 192.168.1.5 -oN results.txt
```

The -oN flag saves results to a file named results.txt.

This is useful for:
- Keeping records of scans
- Comparing scans over time
- Creating documentation
- Automating analysis

Output file contents:
```
# Nmap 7.80 scan initiated Mon Jan 15 10:40:30 2024
# Nmap command: nmap -sV -O 192.168.1.5 -oN results.txt
Nmap scan report for 192.168.1.5
HOST STATUS: up
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 7.4
80/tcp  open  http    Apache httpd 2.4.6
```

### Option 5: Multiple Hosts

```
nmap -sV -p 22,80,443 192.168.1.5 192.168.1.10 192.168.1.15
```

This scans multiple hosts with the same options.

### Option 6: Network Range

```
nmap -sV 192.168.1.0/24
```

This scans all hosts on the 192.168.1.0/24 network with service detection.

Warning: This can take several minutes depending on network size and responsiveness.

---

## Common Beginner Mistakes

### Mistake 1: Forgetting sudo for OS Detection

Incorrect:
```
nmap -O 192.168.1.5
```

Correct:
```
sudo nmap -O 192.168.1.5
```

OS detection requires root privileges. Without sudo, the scan will run but OS detection will not work properly.

### Mistake 2: Scanning Without Permission

Do not scan networks or hosts that do not belong to you without explicit permission.

This is illegal. Always ensure you have authorization before scanning.

### Mistake 3: Using Too Many Scan Options at Once

Beginner mistake:
```
nmap -A -p- -sS -sV -O 192.168.1.5
```

Better approach:
```
nmap -sV 192.168.1.5
```

Start with basic scans. Add complexity only when you need more information.

### Mistake 4: Expecting Instant Results

Scanning takes time:
- Ping scan: Seconds
- Basic port scan: Seconds to minutes
- Service detection: Minutes
- Full port scan (-p-): 5-30 minutes

Do not interrupt scans prematurely. Allow them to complete.

### Mistake 5: Misinterpreting FILTERED Ports

FILTERED does not mean the port is not a security concern. It means the port is not responding, likely because:
- Firewall is blocking the port
- Host is configured not to respond
- Network latency caused missed response

FILTERED ports are still potentially vulnerable on the other side of the firewall.

### Mistake 6: Ignoring Service Versions

Knowing a port is open is useful. Knowing what service and version is running is much more useful.

Always use -sV when you need security information, not just open port lists.

### Mistake 7: Scanning at Wrong Times

Large scans on production networks:
- Can impact network performance
- Should be scheduled during maintenance windows
- Should be coordinated with network teams

Plan scans appropriately.

---

## Quick Reference

Common basic commands:

Ping scan a single host:
```
nmap -sn 192.168.1.1
```

Ping scan a network:
```
nmap -sn 192.168.1.0/24
```

Port scan single host:
```
nmap 192.168.1.5
```

Port scan with service detection:
```
nmap -sV 192.168.1.5
```

Port scan with OS detection:
```
sudo nmap -O 192.168.1.5
```

Full scan with service and OS detection:
```
sudo nmap -sV -O 192.168.1.5
```

Scan specific ports:
```
nmap -p 22,80,443 192.168.1.5
```

Save results to file:
```
nmap -sV 192.168.1.5 -oN results.txt
```

---

## Next Steps

After mastering basic scans, you can explore:

- Advanced port scanning techniques
- Nmap scripting engine (NSE)
- Vulnerability scanning
- Performance optimization
- Network enumeration techniques
