# tcp-ip-attack-analysis-lab

## Overview 

This project documents a controlled university cybersecurity lab examining attacks against TCP connections and sessions. The lab investigated TCP reset attacks against Telnet and SSH, ICMP-based connection disruption, and TCP session hijacking.

Wireshark was used to capture and analyze network traffic, while packet-generation tools were used to reproduce the techniques in an isolated lab environment.
The purpose was to understand how these attacks work and identify appropriate defensive measures.

> **Ethical Scope:** The techniques documented in this repository are presented strictly for education and defensive purposes.

## Lab Objectives

- Analyze TCP reset attacks against Telnet and SSH connections.
- Examine TCMP Destination Unreachable and Source Quench messages.
- Inspect TCP flags, sequence numbers, and acknowledgement numbers using Wireshark.
- Demonstrate the security risks of using unencrypted Telnet sessions.
- Compare the security properties of Telnet and SSH.
- Identify defensive controls against connection disruption and session hijacking.

## Tools Used

- Linux virtual machines
- Wireshark
- Netwag
- Telnet
- OpenSSH
- ICMP utilities

## 1. TCP RST Attack Against Telnet

### Technique

A TCP Reset (`RST`) packet tells a device to terminate a TCP connection immediately. If a forged reset packet contains information that matches an active connection, the receiving system may accept it and close the session.

In this experiment, a TCP reset packet was generated against an established Telnet connection to observe its effect on session availability.

### Establishing the Telnet Connection

A Telnet connection was first established between the designated virtual machines. The active session provided a baseline before the reset packet was introduced.

![Established Telnet session](assets/screenshots/01-telnet-session-established.png)

**Observation:** The terminal confirmed that the Telnet connection was active and accepting commands normally.

### Generating the TCP Reset Packet

Netwag was used inside the isolated lab to generate a TCP reset packet associated with the active Telnet connection.

![TCP reset tool interface](assets/screenshots/02-telnet-rst-tool-interface.png)

**Observation:** The packet-generation interface was configured to reproduce a TCP reset scenario against the designated lab connection.

### Connection Termination

After the reset packet was transmitted, the Telnet session was immediately interrupted.

![Closed Telnet connection](assets/screenshots/03-telnet-connection-closed.png)

**Result:** The terminal displayed the message `Connection closed by foreign host`, confirming that the active TCP session had been terminated.

### Security Analysis

This experiment demonstrates that forged TCP reset packets can be used to disrupt established connections, producing a denial-of-service condition. Telnet is especially unsafe because it transmits session data without encryption, making connection information and application content easier to observe.

Potential defensive measures include:

- Replacing Telnet with SSH for remote administration.
- Using stateful firewalls to validate established connections.
- Monitoring unexpected or repeated TCP `RST` packets.
- Applying ingress and egress filtering to reduce IP address spoofing.
- Keeping operating systems and network devices updated.
