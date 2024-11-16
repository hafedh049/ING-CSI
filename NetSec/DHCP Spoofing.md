### **DHCP Snooping in Detail**

**DHCP Snooping** is a security feature implemented on network switches to prevent **DHCP Spoofing** and **DHCP Starvation** attacks. It filters out untrusted DHCP messages and ensures only valid DHCP responses reach clients, effectively blocking rogue DHCP servers and unauthorized DHCP traffic.

### **How DHCP Snooping Works**

1. **Trusted vs. Untrusted Ports**:
   - **Trusted Ports**: These are designated interfaces connected to legitimate DHCP servers or devices that should be allowed to send DHCP responses (e.g., DHCP ACK, DHCP OFFER).
   - **Untrusted Ports**: All other interfaces are marked as untrusted. These ports are only allowed to forward DHCP requests (e.g., DHCP DISCOVER, DHCP REQUEST) but block any DHCP responses to prevent rogue servers from replying.

2. **DHCP Snooping Table (Binding Table)**:
   - The switch maintains a **DHCP Snooping Table**, mapping IP addresses to MAC addresses, VLANs, and ports based on DHCP transactions. This table helps in identifying and filtering unauthorized DHCP traffic and assists in mitigating ARP spoofing attacks.

3. **Packet Inspection**:
   - The switch inspects DHCP packets passing through it. Only DHCP replies from trusted ports are allowed. Any DHCP OFFER or ACK from untrusted ports is dropped.

### **Setting Up DHCP Snooping on Cisco Switches**

#### **Step 1: Enable DHCP Snooping Globally**
```css
Switch(config)# ip dhcp snooping
```

#### **Step 2: Enable DHCP Snooping on Specific VLANs**
```css
Switch(config)# ip dhcp snooping vlan 10,20
```

- This command enables DHCP Snooping on VLANs 10 and 20.

#### **Step 3: Configure Trusted Ports**
Mark the port connected to the legitimate DHCP server as trusted:
```css
Switch(config-if)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust
```

#### **Step 4: Set a Rate Limit for DHCP Packets (Optional)**
To prevent DHCP Starvation attacks, you can limit the number of DHCP packets per second:
```css
Switch(config-if)# ip dhcp snooping limit rate 15
```

#### **Step 5: Verify DHCP Snooping Status**
Check the status of DHCP Snooping and verify trusted interfaces:
```css
Switch# show ip dhcp snooping
```

**Output Example**:
```css
DHCP Snooping is enabled
DHCP Snooping is configured on the following VLANs:
    10, 20
Trusted interfaces:
    Interface               Trusted Rate limit (pps)
    ----------------------- ------- ----------------
    GigabitEthernet0/1      Yes     15
```

### **How to Verify DHCP Snooping Table**

You can view the DHCP binding table to check legitimate DHCP leases:
```css
Switch# show ip dhcp snooping binding
```

**Output Example**:
```css
MacAddress          IpAddress        Lease(sec)  Type  VLAN  Interface
------------------  ---------------  ----------  ----  ----  ----------------
00:1A:2B:3C:4D:5E   192.168.1.10     600         dhcp  10    GigabitEthernet0/2
```

This output shows valid DHCP bindings, which include the MAC address, IP address, VLAN, and the interface from where the DHCP request originated.

### **Example Scenario**

**Lab Setup**:

- **Switch** with VLAN 10.
- **Legitimate DHCP Server** connected to `GigabitEthernet0/1` (trusted port).
- **Client Machine** connected to `GigabitEthernet0/2` (untrusted port).
- **Attacker Machine** connected to `GigabitEthernet0/3` (untrusted port).

**Attack Simulation**:

1. **Without DHCP Snooping**:
   - The attacker can set up a rogue DHCP server using tools like `dnsmasq` or `Yersinia` to respond to client DHCP requests.
   - The rogue DHCP server sends malicious DHCP OFFER packets, redirecting traffic through the attacker's gateway.

2. **With DHCP Snooping**:
   - The switch blocks any DHCP OFFER from untrusted ports (`GigabitEthernet0/3`).
   - Only DHCP responses from the legitimate DHCP server on the trusted port (`GigabitEthernet0/1`) are forwarded to clients.

### **Commands for Attackers and Defenders**

**Attacker's Command (Without DHCP Snooping)**:
```css
sudo dnsmasq --interface=eth0 --dhcp-range=192.168.1.100,192.168.1.200,12h
```

- This sets up a rogue DHCP server on the attacker machine, offering IP addresses in the range 192.168.1.100 - 192.168.1.200.

**Defender's Command (With DHCP Snooping)**:
```css
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10
Switch(config-if)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust
Switch(config-if)# ip dhcp snooping limit rate 10
```

### **Mitigation and Best Practices**

1. **Enable DHCP Snooping**:
   - Prevent rogue DHCP servers by only allowing trusted ports to respond to DHCP requests.

2. **Rate Limiting**:
   - Limit the number of DHCP packets to reduce the impact of DHCP Starvation attacks.

3. **Static IP Assignments for Critical Devices**:
   - Use static IPs for servers, routers, and switches to avoid dependency on DHCP.

4. **Dynamic ARP Inspection (DAI)**:
   - Use DAI with DHCP Snooping to prevent ARP spoofing, leveraging the DHCP Snooping binding table to validate ARP requests and responses.

### **Conclusion**

DHCP Snooping is a critical security feature for mitigating DHCP-based attacks like DHCP Spoofing and DHCP Starvation. By establishing trusted and untrusted ports, inspecting DHCP traffic, and maintaining a binding table, it ensures that only legitimate DHCP servers can respond to clients, thus protecting the integrity and availability of the network's IP configuration.

This feature should be part of the default security measures on any enterprise network, along with additional layers like ARP inspection and MAC address filtering, to safeguard against common Layer 2 attacks.

### **DHCP Snooping Without VLANs**

If you are not using VLANs and still want to enable DHCP Snooping, the configuration remains similar but simplified since there is no need to specify VLANs. DHCP Snooping can still be effective on a flat network (without VLAN segmentation) by securing the interfaces directly.

### **Configuring DHCP Snooping Without VLANs**

#### **Step 1: Enable DHCP Snooping Globally**
```css
Switch(config)# ip dhcp snooping
```

#### **Step 2: Configure Trusted and Untrusted Ports**

- **Trusted Ports**: Set only on the interface connected to the legitimate DHCP server and between switches (e.g., `GigabitEthernet0/1`).
- **Untrusted Ports**: All other ports are untrusted by default. No specific configuration is needed for untrusted ports.

```css
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# ip dhcp snooping trust
```

#### **Step 3: Set Rate Limiting on Untrusted Ports (Optional)**
Rate limiting can help mitigate DHCP Starvation attacks by restricting the number of DHCP requests allowed per second:
```css
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# ip dhcp snooping limit rate 10
```

This configuration limits `GigabitEthernet0/2` to a maximum of 10 DHCP requests per second, reducing the impact of potential DHCP attacks.

#### **Step 4: Verify the Configuration**
Check the status of DHCP Snooping:
```css
Switch# show ip dhcp snooping
```

**Example Output**:
```css
DHCP Snooping is enabled
DHCP Snooping is configured on all interfaces (No VLANs specified)
Trusted interfaces:
    Interface               Trusted Rate limit (pps)
    ----------------------- ------- ----------------
    GigabitEthernet0/1      Yes     Unlimited
```

#### **Step 5: View DHCP Snooping Binding Table**
The DHCP Snooping table provides a list of valid IP-to-MAC address mappings:
```css
Switch# show ip dhcp snooping binding
```

**Output Example**:
```css
MacAddress          IpAddress        Lease(sec)  Type  Interface
------------------  ---------------  ----------  ----  ----------------
00:1A:2B:3C:4D:5E   192.168.1.10     600         dhcp  GigabitEthernet0/2
```

### **Example Scenario**

**Scenario**:
- A simple network with no VLAN segmentation.
- **Legitimate DHCP Server** connected to `GigabitEthernet0/1`.
- **Client Machines** connected to `GigabitEthernet0/2` and `GigabitEthernet0/3`.
- **Attacker** connected to `GigabitEthernet0/4`.

**Attack Simulation**:
1. **Without DHCP Snooping**:
   - The attacker sets up a rogue DHCP server using `dnsmasq` or similar tools.
   - The attacker responds to DHCP DISCOVER packets before the legitimate server, tricking clients into using a fake gateway.

   ```css
   # Attacker's command to set up rogue DHCP server
   sudo dnsmasq --interface=eth0 --dhcp-range=192.168.1.100,192.168.1.200,12h
   ```

2. **With DHCP Snooping**:
   - The switch blocks DHCP OFFER packets from the attacker's port (`GigabitEthernet0/4`) because it is untrusted.
   - Only DHCP responses from the legitimate server on the trusted port (`GigabitEthernet0/1`) are forwarded to clients.

### **Benefits of DHCP Snooping Without VLANs**

- **Protection Against Rogue DHCP Servers**: Ensures only trusted DHCP servers respond to clients.
- **Prevention of DHCP Starvation**: Rate limiting prevents excessive DHCP requests, mitigating starvation attacks.
- **Enhanced Security**: Even in flat networks without VLANs, DHCP Snooping helps protect against common Layer 2 attacks.

### **Conclusion**

Using DHCP Snooping without VLANs is effective in securing DHCP traffic on simple networks. By marking legitimate DHCP server interfaces as trusted and setting rate limits on untrusted ports, you can prevent unauthorized DHCP responses and mitigate attacks like DHCP Spoofing and DHCP Starvation.

This setup is ideal for small to medium-sized networks where VLAN segmentation is not implemented but where securing DHCP traffic is still a priority.