Scapy is a powerful Python library used for network packet manipulation, which provides numerous methods for packet crafting, manipulation, and network analysis. Below is a list of commonly used methods and functions in Scapy:

### General Methods
1. **`sr()`** - Send and receive packets at the network layer.
2. **`sr1()`** - Send packets and receive the first response.
3. **`srp()`** - Send and receive packets at the data link layer.
4. **`srp1()`** - Send packets at the data link layer and receive the first response.
5. **`send()`** - Send packets at the network layer without waiting for responses.
6. **`sendp()`** - Send packets at the data link layer without waiting for responses.
7. **`sniff()`** - Capture packets from the network.
8. **`ls()`** - List all fields of a protocol.
9. **`lsc()`** - List all Scapy commands.
10. **`lss()`** - List all supported socket fields.
11. **`hexdump()`** - Display the hex dump of a packet.
12. **`wireshark()`** - Open captured packets in Wireshark.
13. **`promiscping()`** - Test if hosts respond to ARP in promiscuous mode.

### Packet Manipulation
1. **`Ether()`** - Create Ethernet packets.
2. **`IP()`** - Create IP packets.
3. **`TCP()`** - Create TCP packets.
4. **`UDP()`** - Create UDP packets.
5. **`ICMP()`** - Create ICMP packets.
6. **`Dot11()`** - Create 802.11 (Wi-Fi) packets.
7. **`ARP()`** - Create ARP packets.
8. **`fragment()`** - Fragment IP packets.
9. **`fragments()`** - Fragment packets with configurable size.
10. **`conf.route.add()`** - Add a new routing entry.
11. **`conf.iface`** - Set the interface for sending packets.

### Utility Methods
1. **`rdpcap()`** - Read packets from a pcap file.
2. **`wrpcap()`** - Write packets to a pcap file.
3. **`IPID_count()`** - Analyze IPID field values in packets.
4. **`traceroute()`** - Perform traceroute.
5. **`arping()`** - Send ARP requests to find active hosts on the network.
6. **`nmap_fp()`** - Perform OS fingerprinting using the Nmap fingerprint.
7. **`checksum()`** - Calculate the checksum for a packet.
8. **`linehexdump()`** - Create a hex dump of a packet, line by line.
9. **`split_layers()`** - Split a layer in a packet.

### Interactive Commands
1. **`interact()`** - Start interactive Scapy session.
2. **`is_promisc()`** - Check if a host is in promiscuous mode.
3. **`get_if_list()`** - List all network interfaces.
4. **`get_if_hwaddr()`** - Get the hardware address of an interface.

These methods allow users to craft, send, receive, and manipulate packets for a variety of purposes, such as network testing, security research, and network forensics. The power of Scapy lies in its flexibility and ability to work with many layers of network protocols, which makes it a valuable tool for network engineers and security analysts.

Certainly! The syntax of Scapy is quite intuitive and Pythonic, designed for ease of use in creating, manipulating, and analyzing network packets. Below is an overview of the key components of Scapy's syntax:

### 1. Importing Scapy
Before using Scapy, you need to import it. You can import all functions and classes using:
```python
from scapy.all import *
```
Alternatively, for specific functionalities:
```python
from scapy.all import IP, TCP, send, sniff
```

### 2. Packet Creation
Scapy uses classes to define different types of packets. The syntax typically follows the pattern:
```python
packet = PacketType(field1=value1, field2=value2, ...)
```
For example:
```python
# Creating an IP packet
ip_packet = IP(dst="8.8.8.8", src="192.168.1.1")

# Creating a TCP packet
tcp_packet = TCP(sport=12345, dport=80, flags="S")
```

### 3. Packet Stacking
You can stack multiple protocols in a single packet using the `/` operator:
```python
packet = IP(dst="8.8.8.8") / TCP(sport=12345, dport=80, flags="S")
```
This creates a packet that contains both an IP layer and a TCP layer.

### 4. Sending Packets
To send packets, you can use `send()` for layer 3 (network) or `sendp()` for layer 2 (data link):
```python
send(packet)     # Sends at the network layer
sendp(packet)    # Sends at the data link layer
```

### 5. Receiving Packets
Scapy provides several methods for receiving packets, including:
- `sniff()`: Captures packets.
- `sr()`: Sends packets and receives responses.
- `sr1()`: Sends packets and receives the first response.

For example:
```python
# Sniffing 10 packets
packets = sniff(count=10)

# Sending and receiving packets
ans, unans = sr(IP(dst="8.8.8.8")/ICMP())
```

### 6. Analyzing Packets
Once you have a packet or a list of packets, you can analyze them using methods like:
- `show()`: Displays the contents of a packet.
- `hexdump()`: Displays the hex dump of a packet.

For example:
```python
packets.show()      # Show details of the captured packets
packet.show()       # Show details of a specific packet
hexdump(packet)     # Show the hex dump of a specific packet
```

### 7. Filtering Packets
You can filter packets during sniffing using the `filter` parameter. This accepts a BPF (Berkeley Packet Filter) syntax:
```python
# Sniffing only TCP packets
packets = sniff(filter="tcp", count=10)
```

### 8. Writing and Reading PCAP Files
Scapy provides methods for reading from and writing to PCAP files:
```python
# Read packets from a PCAP file
packets = rdpcap("example.pcap")

# Write packets to a PCAP file
wrpcap("output.pcap", packets)
```

### 9. Using Layers and Fields
Scapy allows you to access specific fields in packets using dot notation:
```python
# Accessing fields in an IP packet
dst_ip = ip_packet.dst   # Get destination IP
src_ip = ip_packet.src    # Get source IP

# Modify fields
ip_packet.ttl = 64        # Change Time-To-Live value
```

### 10. Layer Modification
You can modify packet layers easily:
```python
ip_packet[IP].ttl = 64      # Set the TTL field of the IP layer
```

### Example Summary
Here’s a complete example putting it all together:
```python
from scapy.all import *

# Create a packet
ip_packet = IP(dst="8.8.8.8") / ICMP()

# Send the packet and wait for a response
response = sr1(ip_packet)

# Analyze the response
if response:
    response.show()  # Display response details
```

### Conclusion
Scapy’s syntax is designed to be flexible and straightforward, making it easier to work with complex network protocols. The ability to stack protocols, filter packets, and access fields makes it a powerful tool for network analysis and packet manipulation. As you gain more experience with Scapy, you’ll find its syntax quite intuitive and efficient for a variety of networking tasks.