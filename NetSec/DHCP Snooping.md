### **DHCP Snooping Configuration**

#### **What is DHCP Snooping?**

DHCP Snooping is a security feature on Cisco switches that prevents unauthorized (rogue) DHCP servers from assigning IP addresses to clients. It acts as a firewall between untrusted devices and the trusted DHCP server, ensuring only legitimate DHCP servers can provide IP configurations.

---

### **How DHCP Snooping Works**
1. **Trusted Ports**: Ports connected to legitimate DHCP servers are marked as trusted.
2. **Untrusted Ports**: Ports connected to client devices or unauthorized devices are marked as untrusted.
3. **Filtering**: DHCP Snooping inspects DHCP messages from untrusted ports and allows only valid DHCP responses.
4. **Binding Table**: The switch creates a database (DHCP Snooping Binding Table) containing IP-to-MAC address mappings for legitimate devices.

---

### **Key Use Cases**
- Prevent rogue DHCP servers from assigning IP addresses.
- Protect against DHCP starvation and man-in-the-middle attacks.
- Ensure only authorized devices obtain IP addresses.

---

### **Configuration Steps**

#### **Step 1: Enable DHCP Snooping Globally**
Enable DHCP Snooping on the switch:
```c
ip dhcp snooping
```

#### **Step 2: Enable DHCP Snooping on VLANs**
Specify the VLANs where DHCP Snooping is required:
```c
ip dhcp snooping vlan <vlan-id>
```

Example:
```c
ip dhcp snooping vlan 10
```

#### **Step 3: Configure Trusted Ports**
Mark interfaces connected to legitimate DHCP servers as trusted:
```c
interface <interface-id>
ip dhcp snooping trust
```

Example:
```c
interface g0/1
ip dhcp snooping trust
```

#### **Step 4: Set Rate Limiting on Untrusted Ports (Optional)**
Limit the rate of DHCP packets on untrusted ports to protect against DHCP starvation attacks:
```c
interface <interface-id>
ip dhcp snooping limit rate <rate>
```

Example:
```c
interface f0/2
ip dhcp snooping limit rate 15
```

#### **Step 5: Verify Configuration**
1. Verify global DHCP Snooping status:
   ```c
   show ip dhcp snooping
   ```

2. Verify the DHCP Snooping binding table:
   ```c
   show ip dhcp snooping binding
   ```

---

### **Example Configuration**

#### **Scenario**
- VLAN 10 is used for client devices.
- The DHCP server is connected to port `g0/1`.
- Ports `f0/2` and `f0/3` are connected to client devices.

#### **Configuration**
```c
conf t
ip dhcp snooping
ip dhcp snooping vlan 10

interface g0/1
ip dhcp snooping trust

interface f0/2
ip dhcp snooping limit rate 10

interface f0/3
ip dhcp snooping limit rate 10
```

---

### **Verification Commands**

1. **Check DHCP Snooping Status**:
   ```c
   show ip dhcp snooping
   ```

2. **View Trusted Ports**:
   ```c
   show ip dhcp snooping
   ```

3. **Check DHCP Snooping Binding Table**:
   ```c
   show ip dhcp snooping binding
   ```

4. **Verify Interface Settings**:
   ```c
   show ip dhcp snooping interface <interface-id>
   ```

---

### **How to Configure DHCP Snooping in GNS3**

#### **1. Setup the Topology**
1. Add a switch, a DHCP server, and client PCs.
2. Connect the DHCP server to one interface (e.g., `g0/1`) and clients to other interfaces (e.g., `f0/2`, `f0/3`).

#### **2. Configure DHCP Server**
1. Assign an IP address to the server interface.
2. Configure DHCP pools on the server.

#### **3. Configure DHCP Snooping on the Switch**
1. Enable DHCP Snooping:
   ```c
   ip dhcp snooping
   ip dhcp snooping vlan 10
   ```
2. Mark the DHCP server port as trusted:
   ```c
   interface g0/1
   ip dhcp snooping trust
   ```
3. Set rate limits on client-facing ports:
   ```c
   interface f0/2
   ip dhcp snooping limit rate 10
   ```

---

### **Testing DHCP Snooping**
1. **Connect a Rogue DHCP Server**:
   - Connect an unauthorized DHCP server to an untrusted port.
   - Ensure the rogue DHCP server's packets are blocked.

2. **Check Client IP Addresses**:
   - Verify clients receive IP addresses only from the legitimate DHCP server.

3. **Inspect the Binding Table**:
   - Use the `show ip dhcp snooping binding` command to ensure only authorized devices are listed.

---

### **Troubleshooting**

1. **DHCP Snooping Not Working**:
   - Ensure DHCP Snooping is enabled globally and on the correct VLAN.
   - Verify the trusted port configuration.

2. **Clients Not Receiving IP Addresses**:
   - Confirm the DHCP server port is marked as trusted.
   - Check the rate limiting configuration.

3. **Binding Table is Empty**:
   - Ensure DHCP Snooping is correctly configured on the VLAN.
   - Verify DHCP messages are being processed by the switch.

---
