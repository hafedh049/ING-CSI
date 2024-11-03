This code is a Python script designed to scan a range of IP addresses by sending ping requests to each address. It identifies which devices in that range are "live" (responding to ping requests) and reports how long the scanning process took.

Here's a detailed breakdown:

---

### Code Explanation

```python
import os
import platform
from datetime import datetime
```

1. **Import Libraries**:
   - `os`: Allows the script to run operating system commands.
   - `platform`: Used to detect the current operating system.
   - `datetime`: Used to track the scan's start and end times to measure its duration.

```python
net = input("Enter the Network Address: ")
```

2. **Network Address Input**:
   - `net`: Takes the first three parts of the network IP address from the user (e.g., `192.168.1.`), leaving room to append each final IP in the range later.

```python
st1 = int(input("Enter the Starting Number: "))
en1 = int(input("Enter the Last Number: ")) + 1
```

3. **Range for IP Scanning**:
   - `st1`: The starting number for the last segment of the IP address.
   - `en1`: The ending number (inclusive). The `+ 1` allows the range to include the last IP address.

For instance, if `st1` is `10` and `en1` is `20`, the script will check IP addresses from `192.168.1.10` to `192.168.1.20`.

```python
ping = "ping -n 1 " if platform.system().lower() == "windows" else "ping -c 1 "
```

4. **Platform-Dependent Ping Command**:
   - This line checks the operating system using `platform.system()`.
   - If it's Windows, the script uses `"ping -n 1 "`.
   - If it's Linux or another system, it uses `"ping -c 1 "`.
   - `-n 1` (Windows) and `-c 1` (Linux) tell the `ping` command to send only one packet, speeding up the scan.

```python
t1 = datetime.now()
print("Scanning in Progress:")
```

5. **Record Start Time**:
   - `t1`: Stores the current time to calculate the total scan duration later.
   - Displays a "Scanning in Progress" message.

```python
for ip in range(st1, en1):
   addr = net + str(ip)
   comm = ping + addr
   response = os.popen(comm)
```

6. **Loop Through IP Addresses**:
   - `for ip in range(st1, en1)`: Loops over each IP in the specified range.
   - `addr`: Constructs each full IP address by appending the current value of `ip` to `net`.
   - `comm`: Forms the complete ping command with `ping` (set earlier based on the OS) and the IP address.
   - `response = os.popen(comm)`: Executes the ping command and captures the output in `response`.

```python
for line in response.readlines():
   if line.count("TTL"):
      print(addr, "--> Live")
```

7. **Check for Live Devices**:
   - `for line in response.readlines()`: Reads each line in the ping output.
   - `if line.count("TTL")`: Checks if `"TTL"` (Time to Live) is present in the output.
      - If `"TTL"` is present, it indicates a response from the device, marking it as "live."
   - `print(addr, "--> Live")`: Prints the IP address of each live device.

```python
t2 = datetime.now()
total = t2 - t1
print("Scanning completed in:", total)
```

8. **Calculate and Display Duration**:
   - `t2`: Stores the end time after scanning completes.
   - `total = t2 - t1`: Calculates the duration by subtracting the start time `t1` from the end time `t2`.
   - `print("Scanning completed in:", total)`: Outputs the total time taken for the scan.

---

### Summary

This script:
1. Takes a network IP range and pings each address within that range.
2. Identifies "live" devices based on the presence of a `TTL` response.
3. Reports each active IP address and displays the scan duration. 

This type of script is commonly used in network administration for quickly identifying active devices on a network.