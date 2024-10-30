Here's a table that lists common network protocols, their associated services, and port numbers:

| **Protocol**   | **Service**            | **Port Number**  |
|----------------|------------------------|------------------|
| **TCP/UDP**    | **Well-Known Ports (0-1023)** |                |
| **TCP**        | FTP (File Transfer Protocol) - Data Transfer | 20 |
| **TCP**        | FTP (File Transfer Protocol) - Control       | 21 |
| **TCP**        | SSH (Secure Shell)                           | 22 |
| **TCP**        | Telnet                                        | 23 |
| **TCP**        | SMTP (Simple Mail Transfer Protocol)         | 25 |
| **TCP**        | DNS (Domain Name System) - Zone Transfer     | 53 |
| **UDP**        | DNS (Domain Name System) - Queries           | 53 |
| **TCP**        | HTTP (Hypertext Transfer Protocol)           | 80 |
| **TCP**        | POP3 (Post Office Protocol version 3)        | 110 |
| **TCP**        | NTP (Network Time Protocol)                  | 123 |
| **UDP**        | NTP (Network Time Protocol)                  | 123 |
| **TCP**        | IMAP (Internet Message Access Protocol)      | 143 |
| **TCP**        | HTTPS (HTTP Secure)                          | 443 |
| **TCP**        | SMTPS (Simple Mail Transfer Protocol Secure) | 465 |
| **TCP**        | FTPS (FTP Secure)                            | 990 |
| **UDP**        | DHCP (Dynamic Host Configuration Protocol) - Client | 68 |
| **UDP**        | DHCP (Dynamic Host Configuration Protocol) - Server | 67 |
| **UDP**        | TFTP (Trivial File Transfer Protocol)        | 69 |
| **TCP**        | LDAP (Lightweight Directory Access Protocol) | 389 |
| **TCP**        | LDAPS (LDAP Secure)                          | 636 |
| **TCP**        | SNMP (Simple Network Management Protocol)    | 161 |
| **UDP**        | SNMP (Simple Network Management Protocol)    | 161 |
| **UDP**        | SNMP Trap                                    | 162 |
| **TCP**        | MySQL Database                               | 3306 |
| **TCP**        | RDP (Remote Desktop Protocol)                | 3389 |
| **TCP**        | SMB (Server Message Block)                   | 445 |
| **TCP**        | NetBIOS (Network Basic Input Output System)  | 139 |
| **UDP**        | NetBIOS Name Service                         | 137 |
| **UDP**        | NetBIOS Datagram Service                     | 138 |
| **TCP**        | Kerberos                                     | 88 |
| **TCP**        | MS SQL (Microsoft SQL Server)                | 1433 |
| **TCP**        | PostgreSQL                                   | 5432 |
| **TCP**        | VNC (Virtual Network Computing)              | 5900 |
| **UDP**        | Syslog (System Logging Protocol)             | 514 |
| **TCP/UDP**    | **Registered Ports (1024-49151)**            |                |
| **TCP**        | HTTP Proxy (Squid Proxy)                     | 3128 |
| **TCP**        | BGP (Border Gateway Protocol)                | 179 |
| **TCP**        | OpenVPN                                      | 1194 |
| **TCP**        | IPsec (Internet Protocol Security)           | 500 |
| **UDP**        | IPsec (Internet Protocol Security)           | 500 |
| **TCP**        | MongoDB Database                             | 27017 |
| **TCP**        | Cassandra Database                           | 9042 |
| **UDP**        | RIP (Routing Information Protocol)           | 520 |
| **TCP**        | HTTPS (Alternative, often used by proxy)     | 8443 |
| **UDP**        | Radius Authentication                        | 1812 |
| **UDP**        | Radius Accounting                            | 1813 |
| **TCP**        | WebSocket Secure (WSS)                       | 443 |
| **TCP/UDP**    | **Dynamic/Private Ports (49152-65535)**      |                |
| **TCP/UDP**    | Dynamic and Ephemeral Ports (Random Ports)   | Random |

### Key:
- **TCP**: Transmission Control Protocol (connection-oriented).
- **UDP**: User Datagram Protocol (connectionless, faster but less reliable than TCP).
- **Well-known ports**: 0-1023 are assigned by IANA for core services.
- **Registered ports**: 1024-49151 are used for applications by vendors and developers.
- **Dynamic/private ports**: 49152-65535 are typically used for temporary or private communications (like client-side connections).

This table provides a list of the most commonly used protocols and their services with corresponding ports.

Here’s a table listing common **ICMP (Internet Control Message Protocol)** message types and codes, which are crucial for diagnostic and control purposes in network operations:

| **ICMP Type** | **Message Type**                     | **Code** | **Description**                                              |
|---------------|--------------------------------------|----------|--------------------------------------------------------------|
| 0             | Echo Reply                           | 0        | Response to a Ping (ICMP Echo Request).                       |
| 3             | Destination Unreachable              | 0        | Network Unreachable.                                          |
|               |                                      | 1        | Host Unreachable.                                             |
|               |                                      | 2        | Protocol Unreachable.                                         |
|               |                                      | 3        | Port Unreachable.                                             |
|               |                                      | 4        | Fragmentation Needed and Don't Fragment (DF) Set.             |
|               |                                      | 5        | Source Route Failed.                                          |
|               |                                      | 6        | Destination Network Unknown.                                  |
|               |                                      | 7        | Destination Host Unknown.                                     |
|               |                                      | 9        | Destination Network Prohibited by Administrative Rules.       |
|               |                                      | 10       | Destination Host Prohibited by Administrative Rules.          |
|               |                                      | 11       | Network Unreachable for Type of Service.                      |
|               |                                      | 12       | Host Unreachable for Type of Service.                         |
|               |                                      | 13       | Communication Administratively Prohibited.                    |
| 4             | Source Quench (Deprecated)           | 0        | Congestion control - packet drop due to congestion (deprecated).|
| 5             | Redirect                             | 0        | Redirect Datagram for the Network (change routing path).      |
|               |                                      | 1        | Redirect Datagram for the Host.                               |
|               |                                      | 2        | Redirect Datagram for the Type of Service and Network.        |
|               |                                      | 3        | Redirect Datagram for the Type of Service and Host.           |
| 8             | Echo Request                         | 0        | Ping request.                                                 |
| 9             | Router Advertisement                 | 0        | Router discovery advertisement message.                       |
| 10            | Router Solicitation                  | 0        | Router discovery/solicitation message.                        |
| 11            | Time Exceeded                        | 0        | TTL (Time to Live) exceeded in transit (usually for traceroute).|
|               |                                      | 1        | Fragment Reassembly Time Exceeded.                            |
| 12            | Parameter Problem                    | 0        | Pointer indicates the error.                                  |
|               |                                      | 1        | Missing a required option.                                    |
|               |                                      | 2        | Bad length.                                                   |
| 13            | Timestamp Request                    | 0        | Request for a timestamp.                                      |
| 14            | Timestamp Reply                      | 0        | Reply with a timestamp.                                       |
| 15            | Information Request (Deprecated)     | 0        | Request for information (deprecated).                         |
| 16            | Information Reply (Deprecated)       | 0        | Reply to information request (deprecated).                    |
| 17            | Address Mask Request                 | 0        | Request for subnet mask.                                      |
| 18            | Address Mask Reply                   | 0        | Reply with subnet mask.                                       |
| 30            | Traceroute (Deprecated)              | 0        | Information for traceroute (not widely supported).            |
| 31            | Datagram Conversion Error            | 0        | Conversion error occurred (not widely used).                  |
| 32            | Mobile Host Redirect                 | 0        | Used for mobile IP operations (not commonly used).            |
| 33            | IPv6 Where-Are-You (Deprecated)      | 0        | Used for early IPv6 operations (deprecated).                  |
| 34            | IPv6 I-Am-Here (Deprecated)          | 0        | Used for early IPv6 operations (deprecated).                  |
| 35            | Mobile Registration Request          | 0        | Request to register a mobile IP node.                         |
| 36            | Mobile Registration Reply            | 0        | Reply to the mobile IP registration request.                  |
| 37            | Domain Name Request (Deprecated)     | 0        | Request for domain name.                                      |
| 38            | Domain Name Reply (Deprecated)       | 0        | Reply with domain name.                                       |
| 39            | SKIP (Security)                      | 0        | Security protocol operations (not widely implemented).        |
| 40            | Photuris                            | 0        | Security failures related to IPsec (authentication failure).  |

### **Key ICMP Types and Use Cases**:
- **Type 0 (Echo Reply)**: Sent in response to an Echo Request (Type 8). Used for Ping.
- **Type 3 (Destination Unreachable)**: Used when a packet cannot reach its destination. Subtypes give more details on the cause.
- **Type 5 (Redirect)**: Used by routers to inform hosts of a better route for a particular destination.
- **Type 8 (Echo Request)**: Sent to a host to request an ICMP Echo Reply (Type 0). Used by the Ping utility.
- **Type 11 (Time Exceeded)**: Used in **traceroute** to show where packets are being dropped when TTL (Time to Live) reaches 0.

### **Deprecated ICMP Messages**:
Some ICMP messages like **Source Quench (Type 4)** or **Information Request (Type 15)** are now deprecated due to their inefficiency or security vulnerabilities.

These ICMP types are critical for troubleshooting and diagnostic tools like **ping** and **traceroute**, helping to ensure network connectivity and performance.