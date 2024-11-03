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


---


In `scapy`, `sr`, `sr1`, and `srp` are different functions used to send and receive packets, each with its specific use case and behavior. Let’s look at each one and the differences between them.

### 1. `sr`
The `sr` function stands for **"send and receive"** and is used to send a packet and receive all responses. 

- **Usage**: `sr(packet, timeout=2)`
- **Returns**: Two lists: one for answered packets (packets that received a response) and one for unanswered packets.
- **When to Use**: This is ideal when you want to send a packet and receive multiple replies. For example, if you’re performing a scan and expect responses from multiple devices, `sr` is useful.

**Example**:
```python
from scapy.all import *

# Sending an ICMP packet and expecting multiple responses
answered, unanswered = sr(IP(dst="192.168.1.0/24")/ICMP())
answered.summary()
```

In this example, `sr` sends the ICMP packet to each IP in the `192.168.1.0/24` subnet and collects any replies.

### 2. `sr1`
The `sr1` function is a **variant of `sr`** that sends a packet and waits for a **single reply**. It only returns the first response received and doesn’t handle multiple replies.

- **Usage**: `sr1(packet, timeout=2)`
- **Returns**: The first response packet received.
- **When to Use**: This is best used when you expect a single reply, such as checking the response from a specific server or device.

**Example**:
```python
from scapy.all import *

# Sending a single ICMP packet and getting only the first reply
response = sr1(IP(dst="192.168.1.1")/ICMP())
response.show()
```

In this case, `sr1` waits for the first ICMP reply from `192.168.1.1` and returns it. If no response is received, it will return `None` after the timeout.

### 3. `srp`
The `srp` function stands for **"send and receive at layer 2 (data link layer)"** and is used for packets that require a Layer 2 address (such as MAC addresses). This is particularly useful for protocols like **ARP** that operate at Layer 2.

- **Usage**: `srp(packet, timeout=2)`
- **Returns**: Two lists—one for answered and one for unanswered packets.
- **When to Use**: Use `srp` when working with protocols at Layer 2 or when you need to send Ethernet frames directly, like in ARP spoofing or LAN scanning.

**Example**:
```python
from scapy.all import *

# Sending an ARP request to each device on a network and getting responses
packet = Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst="192.168.1.0/24")
answered, unanswered = srp(packet, timeout=2)
answered.summary()
```

In this example, `srp` is used to send an ARP broadcast (`ff:ff:ff:ff:ff:ff`) to discover devices on the `192.168.1.0/24` subnet. The function operates at the data link layer (Ethernet) and collects replies from devices that respond to the ARP request.

---

### Summary of Differences

| Function | Layer       | Expected Replies                        | Use Case                          |
|----------|-------------|-----------------------------------------|-----------------------------------|
| `sr`     | Layer 3+    | Multiple replies                       | IP-level communication, multi-response scans |
| `sr1`    | Layer 3+    | Single reply                           | Expecting a single reply          |
| `srp`    | Layer 2     | Multiple replies at the data link layer | MAC-level communication (e.g., ARP) |

Each function serves a slightly different purpose, with `sr` and `sr1` operating above the IP layer and `srp` working directly with Ethernet frames.