This Python script uses `Scapy` to perform a traceroute, which is a network diagnostic tool used to track the path packets take to reach a specific host or IP address. Here’s a detailed breakdown of the code:

### Explanation

```python
from scapy.all import *
import socket
```

1. **Importing Libraries**:
   - `scapy.all`: Imports all functionalities from Scapy, a powerful Python library for packet manipulation.
   - `socket`: Used for network-related operations, here specifically for resolving a hostname to an IP address.

```python
def traceroute(target, max_hops=40, timeout=2):
    print(f"Traceroute vers {target} :")
```

2. **Defining the `traceroute` function**:
   - `target`: The IP address or hostname to trace.
   - `max_hops`: The maximum number of hops (routers) to check before stopping.
   - `timeout`: The timeout in seconds to wait for a reply from each hop.

```python
for ttl in range(1, max_hops + 1):
    pkt = IP(dst=target, ttl=ttl) / ICMP()
```

3. **Looping through Each Hop**:
   - The loop iterates over each possible "hop" or router in the path, from 1 up to `max_hops`.
   - For each hop, a packet is created using the `IP` layer and `ICMP` layer in Scapy.
     - `IP(dst=target, ttl=ttl)`: Creates an IP packet directed at the target IP, with the `ttl` (Time-To-Live) set to the current hop count.
     - `ICMP()`: Adds an ICMP (Internet Control Message Protocol) packet to the IP layer.
       - The ICMP packet is crucial for this traceroute to receive feedback from each router.

```python
reply = sr1(pkt, verbose=0, timeout=timeout)
```

4. **Sending the Packet and Receiving the Response**:
   - `sr1`: Sends the packet and waits for a single response.
   - `verbose=0`: Suppresses Scapy’s verbose output.
   - `timeout=timeout`: Specifies the time to wait for each response.

```python
if reply is None:
    print(f"{ttl}: *** Pas de réponse ***")
```

5. **Handling No Response**:
   - If `reply` is `None`, it means no response was received within the specified timeout, so it prints that no response was received for this hop.

```python
elif reply.type == 11:
    print(f"{ttl}: {reply.src}")
```

6. **Handling an ICMP Time Exceeded Message**:
   - If `reply.type == 11`, it means the packet’s TTL expired, and an ICMP "Time Exceeded" message was received.
   - `reply.src`: Prints the IP address of the router that sent the TTL-expired message, indicating this router as one of the hops on the route.

```python
elif reply.type == 0:
    print(f"{ttl}: {reply.src} (Destination atteinte)")
    break
```

7. **Handling a Destination Reached Message**:
   - If `reply.type == 0`, it means an ICMP "Echo Reply" (type 0) was received, which confirms that the packet has reached its destination.
   - This prints the IP of the target host and breaks the loop, ending the traceroute.

```python
else:
    print(f"{ttl}: Réponse inattendue de {reply.src}")
```

8. **Handling Unexpected Replies**:
   - If the reply is neither a TTL-expired nor an echo reply, it prints that an unexpected response was received. This is rare and may indicate unusual network behavior.

```python
if __name__ == "__main__":
    target= socket.gethostbyname(input("Donner une adresse host à atteindre -->: "))
    traceroute(target)
```

9. **Main Execution**:
   - When the script runs directly, it prompts the user to enter a hostname (e.g., `www.example.com`).
   - `socket.gethostbyname(...)`: Resolves the hostname to an IP address to pass into `traceroute`.

### Summary of Execution Flow

1. The script sends ICMP packets with an increasing TTL to track each router in the path to the destination.
2. For each TTL:
   - If a router sends a TTL-expired message, it prints the router’s IP.
   - If the destination sends a reply, it prints the destination’s IP and stops.
   - If no response is received, it notes that no reply was received for that TTL.
3. This continues until either the destination is reached or the maximum hops are exceeded.