This Python script is designed to perform an ARP scan over a specified IP address range. However, there are some issues in the code, especially in its use of `arp-scan` or `arp -a`, as they don’t directly apply to each IP in a given range. I’ll explain each section, then provide suggestions for improving its functionality.

### Code Walkthrough

```python
import os 
from platform import system
from datetime import datetime
```

1. **Imports**:
   - `os`: Used to run operating system commands.
   - `platform.system`: Helps determine the current operating system.
   - `datetime`: Used to measure the duration of the scan.

```python
net = input("Enter the Network Address: ")
```

2. **Network Address Input**:
   - `net` stores the network address, typically the first three octets of an IP address, like `192.168.1.`.
   - This leaves the last part to be filled by a range of IP addresses.

```python
st1 = int(input("Enter the Starting Number: "))
en1 = int(input("Enter the Last Number: ")) + 1
```

3. **IP Range Input**:
   - `st1`: Start of the IP range (e.g., `1`).
   - `en1`: End of the IP range (e.g., `10`). Adding `+1` ensures that the range is inclusive.

```python
arp = system().lower() == "windows" and "arp -a " or "sudo arp-scan -l "
```

4. **OS-Specific ARP Command**:
   - This line checks the operating system and assigns the appropriate command:
     - If the system is Windows, it uses `"arp -a "` to view the ARP table.
     - For other systems (Linux, MacOS), it defaults to `"sudo arp-scan -l "`, which scans the local network to discover IP addresses and their associated MAC addresses.
   - **Issue**: The `arp -a` and `arp-scan` commands here don’t utilize the `addr` variable, so they won’t scan each specific IP in the range.

```python
t1 = datetime.now()
print("Scanning in Progress:")
```

5. **Start Time and Progress Message**:
   - Records the start time of the scan and displays a message.

```python
for ip in range(st1, en1):
   addr = net + str(ip)
   comm = arp + addr
   response = os.popen(comm)
   print(response.read())
   response.close()
```

6. **Loop Through IP Addresses**:
   - `for ip in range(st1, en1)`: Loops through the IP range specified by the user.
   - `addr`: Constructs each IP address by appending `ip` to the base network address.
   - **`comm = arp`**: This doesn’t actually use `addr`, so it doesn’t achieve a per-IP scan. It instead performs a single ARP scan, and `arp -a` or `arp-scan -l` outputs the whole ARP table or local network scan, respectively.
   - `response = os.popen(comm)`: Executes the ARP scan command, reading its output.
   - `print(response.read())`: Prints the ARP table output or scan result.
   - `response.close()`: Closes the `os.popen` stream.

```python
t2 = datetime.now()
total = t2 - t1
print("Scanning completed in:", total)
```

7. **Calculate and Display Scan Duration**:
   - `t2`: End time after the scan.
   - `total = t2 - t1`: Calculates the scan’s duration.
   - Prints the time taken to complete the scan.

### Issues and Suggested Improvements

1. **Command Selection**:
   - `arp -a` (Windows) lists the ARP table without directly scanning IPs.
   - `arp-scan -l` (Linux) performs a local network scan but does not directly address the IP range provided by the user.

2. **Proposed Solution Using ICMP Ping with ARP Lookup**:
   - Instead of using ARP, we could ping each address individually and check if it’s in the ARP table afterward. This is often how live host detection is performed.

Here’s a modified approach using ICMP pings, then checking the ARP table on Windows. This avoids the need for `sudo` and works in most environments.

```python
import os
import platform
from datetime import datetime

net = input("Enter the Network Address: ")
st1 = int(input("Enter the Starting Number: "))
en1 = int(input("Enter the Last Number: ")) + 1

ping = "ping -n 1 " if platform.system().lower() == "windows" else "ping -c 1 "

t1 = datetime.now()
print("Scanning in Progress:")

for ip in range(st1, en1):
    addr = net + str(ip)
    comm = ping + addr
    response = os.popen(comm)
    
    # Check if we received a response from the host
    if "TTL" in response.read():
        print(f"{addr} --> Live")
        # Run arp -a on Windows to get the MAC address
        if platform.system().lower() == "windows":
            arp_response = os.popen("arp -a " + addr).read()
            print(f"ARP Table Entry for {addr}: {arp_response}")
            
t2 = datetime.now()
total = t2 - t1
print("Scanning completed in:", total)
```

### Explanation of the Updated Code

- This version uses ICMP `ping` to check if each IP address is live.
- When a host responds, we print its IP and check the ARP table entry (for Windows) using `arp -a` to retrieve its MAC address.
- This approach is more reliable, as it directly pings each IP address in the range and only fetches ARP details for responsive addresses.

This method provides better control over each IP scan while still leveraging ARP for MAC address lookups.