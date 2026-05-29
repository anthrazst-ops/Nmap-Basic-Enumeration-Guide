# Nmap - Network Mapping and Enumeration

## Table of Contents

1. [Definition](#definition)
2. [Basic Understanding](#basic-understanding)
3. [Fundamental Concepts](#fundamental-concepts)
4. [Uses and Applications](#uses-and-applications)
5. [Logic and How It Works](#logic-and-how-it-works)
6. [Why Nmap Matters](#why-nmap-matters)

---

## Definition

Nmap stands for Network Mapper. It is an open-source tool used for network discovery and security auditing. Nmap is designed to:

- Detect which hosts are alive and active on a network
- Identify which ports are open on a target host
- Determine what services are running on open ports
- Identify the operating system of target hosts

### Simple Explanation

Think of Nmap as a network surveyor that goes through a network and creates a complete map showing:

- What computers exist and are turned on
- What doors (ports) are open on each computer
- What services are operating through those doors
- What operating system each computer is running

---

## Basic Understanding

Before understanding Nmap, you need to know these network basics:

### Host

A host is any device connected to a network.

Examples:
- Personal computers
- Servers
- Printers
- Network switches
- Routers

### IP Address

An IP address is a unique identifier for each device on a network. It follows the format: XXX.XXX.XXX.XXX

Examples:
- 192.168.1.1 (typical home router)
- 192.168.1.5 (computer on home network)
- 10.0.0.100 (server on corporate network)

### Port

A port is a logical endpoint for network communication on a host. Think of it as a channel through which services communicate.

Common ports:
- Port 22: SSH (Secure Shell)
- Port 80: HTTP (Web traffic)
- Port 443: HTTPS (Secure web traffic)
- Port 3306: MySQL (Database service)
- Port 21: FTP (File Transfer)

### Port States

Ports can exist in different states:

Open: A service is actively accepting connections on this port. The host responds to connection attempts.

Closed: No service is listening on this port, but the host acknowledges the connection attempt and responds.

Filtered: The port is not accessible because a firewall or other security device blocks the traffic. No response is received.

### Active vs Inactive Hosts

Active Host: A device that is turned on and connected to the network. It responds to network probes.

Inactive Host: A device that is turned off or not connected to the network. It does not respond to network probes.

---

## Fundamental Concepts

### How Nmap Operates at the Network Layer

Nmap works by sending specially crafted network packets and analyzing responses. Understanding this requires knowledge of the TCP/IP model.

#### Layer 2: Internet Layer

At this layer, Nmap uses IP addresses to target specific devices and ICMP (Internet Control Message Protocol) to send ping probes.

How it works: Nmap sends a ping request to an IP address. If the target responds with a ping reply, the host is active. If there is no response, the host may be inactive, offline, or protected by a firewall.

#### Layer 3: Transport Layer - TCP

TCP (Transmission Control Protocol) is used for reliable, connection-oriented communication.

TCP Three-Way Handshake:
- Client sends SYN (synchronize) packet
- Server responds with SYN-ACK (synchronize-acknowledge)
- Client sends ACK (acknowledge) to complete handshake

Nmap uses variations of this handshake to determine port states:

SYN Scan: Nmap sends a SYN packet to a port and observes the response.
- If the target responds with SYN-ACK, the port is OPEN
- If the target responds with RST (reset), the port is CLOSED
- If no response is received, the port is FILTERED

#### Layer 4: Transport Layer - UDP

UDP (User Datagram Protocol) is a connectionless protocol.

For UDP scanning, Nmap sends UDP packets to target ports and observes responses:
- If the target responds, the port is likely OPEN
- If the target responds with ICMP unreachable, the port is CLOSED
- If no response is received, the port is OPEN or FILTERED

#### Layer 5: Application Layer

At the application layer, Nmap connects to open ports and examines the responses to identify what service is running.

Service Identification: When Nmap connects to an open port, the service often sends a banner or greeting message. Nmap compares this against a database of known service signatures to identify the service and its version.

Example: A connection to port 22 might receive "SSH-2.0-OpenSSH_7.4", indicating the SSH service running OpenSSH version 7.4.

### Scanning vs Enumeration

Scanning and enumeration are related but different concepts:

Scanning: The process of discovering active hosts and open ports. It answers "What is alive and what ports are accessible?"

Enumeration: The deeper process of identifying services, versions, and system information on discovered hosts. It answers "What exactly is running on those ports?"

Nmap can do both scanning and enumeration in a single run, depending on the options used.

---

## Uses and Applications

### Network Reconnaissance

Reconnaissance means gathering information about a target network before any deeper analysis. Nmap enables network administrators and security professionals to:

- Understand the network topology and active devices
- Identify which services are exposed
- Create an inventory of network assets
- Document the current network state

### Vulnerability Assessment

Vulnerability assessment is the process of identifying security weaknesses. Nmap contributes by:

- Identifying outdated or vulnerable service versions
- Detecting unnecessary or risky services that are running
- Finding potentially misconfigured systems
- Creating a baseline for comparison over time

### Network Inventory and Asset Management

Organizations need to know what devices and services exist on their network. Nmap helps by:

- Creating a comprehensive list of all active hosts
- Documenting the services running on each host
- Recording the operating system of each device
- Tracking changes in the network over time

### Security Monitoring

Continuous monitoring of network changes is important for detecting intrusions or unauthorized changes. Nmap helps by:

- Identifying unexpected new hosts on the network
- Detecting ports that were not previously open
- Noticing when services have been started or stopped
- Alerting to unexpected changes in service versions

### Firewall and Access Control Testing

Security policies are only effective if properly implemented. Nmap helps verify this by:

- Testing whether firewall rules are working as intended
- Confirming that blocked ports are actually blocked
- Identifying accidental open ports that should be closed
- Validating that security policies are correctly applied

### Network Baseline and Compliance

For regulatory compliance and security auditing, organizations need documentation. Nmap provides:

- Baseline documentation of the network state
- Historical records of changes
- Evidence for compliance audits
- Asset inventory for security governance

---

## Logic and How It Works

### Sequential Phases of Nmap Operation

Nmap performs scanning in distinct phases, each building on the previous one:

### Phase 1: Host Discovery

Objective: Determine which hosts on the target network are active and reachable.

Logic:

Nmap begins by sending probe packets to the target range. These probes may include:
- ICMP Echo requests (ping)
- TCP SYN packets to common ports
- TCP ACK packets
- UDP probes

When a probe receives a response from a host, Nmap marks that host as alive. Hosts that do not respond are marked as down or unreachable.

Why this matters: Nmap needs to identify which hosts to scan. Scanning a non-existent host wastes time and resources.

Possible outcomes:
- Host responds: Marked as UP
- Host does not respond: Marked as DOWN (or potentially filtered)

### Phase 2: Port Scanning

Objective: For each discovered host, determine which ports are in an open, closed, or filtered state.

Logic for TCP SYN Scan:

Nmap sends a SYN packet to a specific port on the target host. The sequence of events:

Step 1: Nmap sends SYN packet
- Nmap initiates a connection attempt to the target port
- The packet contains the SYN flag set

Step 2: Target responds
- If a service is listening on that port, the target responds with SYN-ACK
- If no service is listening, the target responds with RST (reset)
- If a firewall blocks the port, there may be no response

Step 3: Nmap interprets the response
- SYN-ACK response: Port is OPEN (listening service accepted the connection)
- RST response: Port is CLOSED (host acknowledged but no service is listening)
- No response: Port is FILTERED (likely blocked by firewall)

Step 4: Nmap repeats for all ports
- This process repeats for each port in the scanning range
- Results are accumulated into a list of open, closed, and filtered ports

Why this matters: Nmap needs to know which ports have accessible services for the next phase.

Scanning thousands of ports: A host may have 65,535 possible ports. Nmap must efficiently test each port to create a complete picture.

### Phase 3: Service and Version Detection

Objective: Identify what service is running on each open port and determine its version.

Logic:

Step 1: Connect to open port
- Nmap initiates a connection to a port marked as OPEN from the previous phase

Step 2: Receive service banner
- When the connection is established, the service often sends a greeting or banner
- Example: "SSH-2.0-OpenSSH_7.4" for SSH services
- Example: "Apache/2.4.41 (Ubuntu)" for web servers

Step 3: Match against signature database
- Nmap maintains a large database of service signatures
- The received banner is compared against this database
- The database contains known patterns for different service versions

Step 4: Identify service and version
- Based on the signature match, Nmap identifies the service name and version number
- This information is recorded for the output

Why this matters: Knowing the exact service and version is critical for vulnerability assessment. Different versions of the same service may have different security issues.

Incomplete identification: Some services may not send clear banners, or may be configured to hide version information. In these cases, Nmap may provide a best guess or be unable to identify the service version.

### Phase 4: Operating System Detection

Objective: Determine the operating system running on the target host.

Logic:

Step 1: Send specialized probe packets
- Nmap sends a series of carefully crafted test packets with different characteristics
- These packets test various aspects of the target's network stack
- Variations include: different TCP flags, fragmentation patterns, timing behaviors

Step 2: Analyze target response patterns
- Each operating system implements the network protocol stack slightly differently
- Different OS versions respond differently to edge cases and unusual conditions
- The pattern of responses is unique to each OS and version

Step 3: Compare against OS fingerprint database
- Nmap maintains a database of OS fingerprints
- A fingerprint is a combination of specific response patterns that uniquely identify an OS
- The observed response pattern is compared against known fingerprints

Step 4: Identify operating system
- When a match is found, Nmap identifies the OS and version
- Example output: "Windows Server 2019" or "Ubuntu 20.04 LTS"

Why this matters: The operating system affects what vulnerabilities may exist and what exploitation techniques are relevant. OS identification helps prioritize and target security assessments.

Confidence levels: OS detection is probabilistic. Nmap assigns confidence percentages when multiple OS matches are possible. Higher percentages indicate greater certainty.

### Complete Scanning Workflow

A typical Nmap scan follows this logical flow:

Host Discovery Phase: Identify 192.168.1.5 as UP

Port Scanning Phase: Find ports 22, 80, 443 OPEN

Service Detection Phase: Port 22 = SSH OpenSSH 7.4
                       Port 80 = HTTP Apache 2.4.41
                       Port 443 = HTTPS Apache 2.4.41

OS Detection Phase: Identify Ubuntu 20.04 LTS

Output: Complete picture of host 192.168.1.5 with all information

---

## Why Nmap Matters

### From a Defensive Perspective

Security professionals and network administrators use Nmap to:

Maintain Visibility: Understand exactly what is connected to the network and what services are running. Without this knowledge, unauthorized or misconfigured systems could go undetected.

Identify Vulnerabilities Proactively: By scanning networks regularly, defenders can find outdated software or unnecessary services before attackers do. This provides time to patch or remediate issues.

Verify Security Controls: Nmap can confirm that firewalls, access controls, and security policies are working as intended.

Respond to Incidents: When a security incident occurs, Nmap helps identify compromised systems and understand how an attacker may have gained access.

Maintain Compliance: Many compliance frameworks require documentation and regular assessment of network security. Nmap provides the data and evidence needed.

### From an Attacker's Perspective

Understanding why attackers use Nmap is important for defense:

Reconnaissance: Attackers use Nmap to map target networks and find entry points before launching attacks.

Vulnerability Discovery: By identifying outdated software or unnecessary services, attackers can find systems with known exploits.

Attack Planning: The information from Nmap scans helps attackers choose the most promising targets and select appropriate exploitation techniques.

### The Importance of Proactive Defense

The fundamental principle: Defenders must scan and assess their own networks before attackers do. By continuously using tools like Nmap, organizations can stay ahead of threats.

---

## Legal and Ethical Considerations

### Authorized Use

Nmap is a legitimate tool when used with proper authorization. Legal uses include:

- Scanning networks you own or administrate
- Conducting security assessments with explicit written permission
- Testing in laboratory or training environments
- Authorized penetration testing engagements
- Academic and security research

### Unauthorized Scanning

Scanning networks without authorization is illegal in most jurisdictions and is considered:

- Unauthorized computer access
- Unauthorized network intrusion
- Potentially criminal hacking activity

### Best Practices

- Always obtain written authorization before scanning any network
- Document all scanning activities
- Use Nmap responsibly and ethically
- Respect privacy and confidentiality of data discovered
- Follow organizational security policies

---

## Summary

Nmap is a fundamental tool for network security professionals. It enables:

- Discovery of active hosts and open ports
- Identification of running services and versions
- Detection of operating systems
- Vulnerability assessment and network inventory
- Verification of security controls
- Proactive defense against threats

Understanding how Nmap works, what information it provides, and how to use it responsibly is essential for anyone involved in network security or system administration.
