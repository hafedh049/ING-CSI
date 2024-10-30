Here is a detailed breakdown of **Private VLAN (PVLAN)** configurations on Cisco devices, with examples using commands.

---

## **What is PVLAN?**  
Private VLAN (PVLAN) is used to isolate ports within the same VLAN. It is mainly used in **service provider environments** to improve security by limiting communication between devices on the same VLAN.  

- **Primary VLAN**: Parent VLAN containing all PVLAN types.
- **Secondary VLAN**: Subdivided into two types:
  1. **Isolated VLAN**: Ports cannot communicate with each other, only with promiscuous ports.
  2. **Community VLAN**: Ports can communicate with each other and promiscuous ports but not with isolated ports.

---

## **1. Configuring Primary and Secondary VLANs**  
This example creates a **primary VLAN** along with **isolated** and **community VLANs**.

### **Configuration Example:**

- Primary VLAN: `10`  
- Isolated VLAN: `11`  
- Community VLAN: `12`

```css
Switch(config)# vlan 10
Switch(config-vlan)# private-vlan primary
Switch(config-vlan)# exit

Switch(config)# vlan 11
Switch(config-vlan)# private-vlan isolated
Switch(config-vlan)# exit

Switch(config)# vlan 12
Switch(config-vlan)# private-vlan community
Switch(config-vlan)# exit
```

---

## **2. Associating Secondary VLANs with Primary VLAN**  
In this step, you map the isolated and community VLANs to the primary VLAN.

```css
Switch(config)# vlan 10
Switch(config-vlan)# private-vlan association 11,12
Switch(config-vlan)# exit
```

---

## **3. Configuring Promiscuous Port**  
Promiscuous ports can communicate with all ports in the primary and secondary VLANs. This is often used for gateways or routers.

### **Configuration Example:**

- Interface: `GigabitEthernet0/1`  
- Primary VLAN: `10`

```css
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode private-vlan promiscuous
Switch(config-if)# switchport private-vlan mapping 10 11,12
Switch(config-if)# exit
```

---

## **4. Configuring Isolated Port**  
Isolated ports can only communicate with promiscuous ports, not with each other.

### **Configuration Example:**

- Interface: `GigabitEthernet0/2`

```css
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# switchport mode private-vlan host
Switch(config-if)# switchport private-vlan host-association 10 11
Switch(config-if)# exit
```

---

## **5. Configuring Community Ports**  
Community ports can communicate with each other and promiscuous ports but not with isolated ports.

### **Configuration Example:**

- Interface: `GigabitEthernet0/3`

```css
Switch(config)# interface GigabitEthernet0/3
Switch(config-if)# switchport mode private-vlan host
Switch(config-if)# switchport private-vlan host-association 10 12
Switch(config-if)# exit
```

---

## **6. Verifying PVLAN Configuration**  
After completing the setup, you can use the following commands to verify the PVLAN configuration.

```css
Switch# show vlan private-vlan
Switch# show interface GigabitEthernet0/1 switchport
Switch# show running-config | include private-vlan
```

---

## **Summary of the Configuration**  
- **Primary VLAN (10)** is associated with **Isolated VLAN (11)** and **Community VLAN (12)**.
- **Promiscuous port (Gi0/1)** communicates with all VLANs.
- **Isolated port (Gi0/2)** can only talk to the promiscuous port.
- **Community port (Gi0/3)** can talk to other community ports and the promiscuous port.

Private VLANs (**PVLANs**) are a **layer 2 isolation mechanism** within a VLAN. They help control traffic between devices within the same VLAN, improving network security, especially in **multi-tenant environments** (such as data centers or service providers). Below is a detailed explanation of the **three PVLAN types**, their behavior, and typical use cases.

---

## **PVLAN Structure**

A **Primary VLAN** is the parent VLAN containing one or more **Secondary VLANs**.  
Secondary VLANs are of **two types**:  
1. **Isolated VLAN**  
2. **Community VLAN**  

There is also a key type of port used in PVLAN:  
- **Promiscuous Ports**: Ports that can communicate with any device within the PVLAN (primary and secondary).

---

### **1. Primary VLAN**  
The **Primary VLAN** serves as the overarching parent that connects multiple isolated and community VLANs. All traffic flows through the primary VLAN. However, communication between devices depends on the type of secondary VLAN they belong to.

- **Example**: VLAN 10 is configured as a Primary VLAN.
- **Behavior**: Devices connected to this VLAN will communicate according to the rules of associated secondary VLANs (either isolated or community).

---

### **2. Secondary VLAN Types**  

There are two types of secondary VLANs:  

#### **2.1. Isolated VLAN**  
- **Description**:  
  - Isolated VLAN ensures **total isolation** between all devices connected to it.  
  - Devices in an isolated VLAN **cannot communicate with each other** but **can communicate with a promiscuous port** (such as a gateway or router).
  
- **Use Case**:  
  Useful in **public cloud services** where multiple customers share infrastructure but their devices shouldn’t interact with one another (e.g., Virtual Machines in the same hypervisor).

- **Example**:  
  VLAN 11 is configured as an **Isolated VLAN** associated with Primary VLAN 10.

- **Behavior**:  
  - Devices on VLAN 11 (isolated VLAN) **cannot talk to each other**.  
  - They can only talk to **promiscuous ports** (like a router).

---

#### **2.2. Community VLAN**  
- **Description**:  
  - Community VLAN allows devices **within the same VLAN** to communicate freely with each other.  
  - They can also communicate with **promiscuous ports** but **cannot talk to devices in isolated VLANs** or other community VLANs.

- **Use Case**:  
  Useful in **private cloud environments** where a group of devices (such as VMs belonging to the same customer) need to communicate with each other but stay isolated from other customers.

- **Example**:  
  VLAN 12 is configured as a **Community VLAN** associated with Primary VLAN 10.

- **Behavior**:  
  - Devices within VLAN 12 (community VLAN) can **talk to each other**.  
  - They can communicate with **promiscuous ports** but **cannot talk to other community VLANs** or isolated VLANs.

---

### **3. Promiscuous Port**  
- **Description**:  
  A **promiscuous port** can communicate with **all devices in the PVLAN**. It serves as the **default gateway** for devices in isolated and community VLANs. Promiscuous ports allow routing or shared services access (e.g., DHCP or internet access).

- **Use Case**:  
  - Configuring a **router interface** or **firewall port** to allow communication between PVLANs and the internet.  
  - Used in **service provider networks** to provide internet or shared service access to isolated or community devices.

- **Behavior**:  
  - Promiscuous ports can talk to **any VLAN (primary, isolated, or community)**.  
  - Devices in isolated and community VLANs can only talk to promiscuous ports (and not to each other, unless they are in the same community VLAN).

---

## **PVLAN Communication Matrix**

| **Source Device**      | **Destination Device** | **Can Communicate?** | **Explanation**                     |
|------------------------|------------------------|----------------------|-------------------------------------|
| Isolated VLAN Device   | Isolated VLAN Device   | No                   | Isolation prevents communication   |
| Isolated VLAN Device   | Promiscuous Port       | Yes                  | Allowed for gateway communication  |
| Community VLAN Device  | Same Community Device  | Yes                  | Communication within same group    |
| Community VLAN Device  | Different Community Device | No               | Isolation between communities      |
| Community VLAN Device  | Promiscuous Port       | Yes                  | Gateway or shared services access  |

---

## **Example PVLAN Configuration on a Cisco Switch**

Below is a full **configuration example** demonstrating how to set up a **Primary VLAN (10)** with an **Isolated VLAN (11)** and a **Community VLAN (12)**, along with assigning appropriate ports.

### **Step 1: Create VLANs**
```css
Switch(config)# vlan 10
Switch(config-vlan)# private-vlan primary
Switch(config-vlan)# exit

Switch(config)# vlan 11
Switch(config-vlan)# private-vlan isolated
Switch(config-vlan)# exit

Switch(config)# vlan 12
Switch(config-vlan)# private-vlan community
Switch(config-vlan)# exit
```

### **Step 2: Associate Secondary VLANs with Primary VLAN**
```css
Switch(config)# vlan 10
Switch(config-vlan)# private-vlan association 11,12
Switch(config-vlan)# exit
```

### **Step 3: Configure Promiscuous Port**
```css
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode private-vlan promiscuous
Switch(config-if)# switchport private-vlan mapping 10 11,12
Switch(config-if)# exit
```

### **Step 4: Configure Isolated Port**
```css
Switch(config)# interface GigabitEthernet0/2
Switch(config-if)# switchport mode private-vlan host
Switch(config-if)# switchport private-vlan host-association 10 11
Switch(config-if)# exit
```

### **Step 5: Configure Community Port**
```css
Switch(config)# interface GigabitEthernet0/3
Switch(config-if)# switchport mode private-vlan host
Switch(config-if)# switchport private-vlan host-association 10 12
Switch(config-if)# exit
```

---

## **Verification Commands**
You can use the following commands to verify the PVLAN configuration:

```css
Switch# show vlan private-vlan
Switch# show interface GigabitEthernet0/1 switchport
Switch# show running-config | include private-vlan
```

---

## **Conclusion**

Using PVLANs provides fine-grained control over **communication within VLANs**, ensuring that devices remain isolated when necessary. Here's a quick summary of the different types:
- **Primary VLAN**: The parent VLAN that encompasses all PVLAN types.
- **Isolated VLAN**: No communication between devices; only promiscuous ports are accessible.
- **Community VLAN**: Devices can communicate with each other and promiscuous ports.
- **Promiscuous Ports**: Accessible by all devices in the PVLAN and typically used for gateways.

This design is ideal for **multi-tenant environments**, providing **network isolation** and **security** while maintaining access to shared services like the internet.

No, you **cannot create more than one isolated VLAN** within a primary VLAN in **PVLAN** configurations on Cisco switches. Each primary VLAN can have **only one isolated VLAN** because all isolated ports must behave identically — **fully isolated from each other** and able to communicate only with **promiscuous ports**.

However, you **can create multiple community VLANs** under the same primary VLAN. These community VLANs allow devices within them to communicate with each other and with promiscuous ports.

---

### **Explanation**:  
In a PVLAN structure:

- **One isolated VLAN**: 
  - All ports within this isolated VLAN behave the same way (isolated from each other).
  - No need for multiple isolated VLANs because isolated VLANs already achieve the strictest form of isolation.

- **Multiple community VLANs**:  
  - Useful when you want to group devices that can communicate within their community but remain isolated from other communities.

---

### **Example Setup with One Isolated VLAN and Multiple Community VLANs**

In this example, we use:
- **Primary VLAN**: 100
- **Isolated VLAN**: 101  
- **Community VLANs**: 102, 103  

```css
Switch(config)# vlan 100
Switch(config-vlan)# private-vlan primary
Switch(config-vlan)# exit

Switch(config)# vlan 101
Switch(config-vlan)# private-vlan isolated
Switch(config-vlan)# exit

Switch(config)# vlan 102
Switch(config-vlan)# private-vlan community
Switch(config-vlan)# exit

Switch(config)# vlan 103
Switch(config-vlan)# private-vlan community
Switch(config-vlan)# exit
```

### **Associate VLANs with Primary VLAN 100**

```css
Switch(config)# vlan 100
Switch(config-vlan)# private-vlan association 101,102,103
Switch(config-vlan)# exit
```

### **Configure Ports:**

1. **Promiscuous Port** on Gig0/1 (router/gateway):
   ```css
   Switch(config)# interface GigabitEthernet0/1
   Switch(config-if)# switchport mode private-vlan promiscuous
   Switch(config-if)# switchport private-vlan mapping 100 101,102,103
   Switch(config-if)# exit
   ```

2. **Isolated Port** on Gig0/2:
   ```css
   Switch(config)# interface GigabitEthernet0/2
   Switch(config-if)# switchport mode private-vlan host
   Switch(config-if)# switchport private-vlan host-association 100 101
   Switch(config-if)# exit
   ```

3. **Community Port in VLAN 102** on Gig0/3:
   ```css
   Switch(config)# interface GigabitEthernet0/3
   Switch(config-if)# switchport mode private-vlan host
   Switch(config-if)# switchport private-vlan host-association 100 102
   Switch(config-if)# exit
   ```

4. **Community Port in VLAN 103** on Gig0/4:
   ```css
   Switch(config)# interface GigabitEthernet0/4
   Switch(config-if)# switchport mode private-vlan host
   Switch(config-if)# switchport private-vlan host-association 100 103
   Switch(config-if)# exit
   ```

---
### **Verification**

```css
Switch# show vlan private-vlan
Switch# show interface GigabitEthernet0/1 switchport
Switch# show running-config | include private-vlan
```

---
### **Summary**

- **One isolated VLAN per primary VLAN** is allowed (VLAN 101).
- **Multiple community VLANs** (VLANs 102, 103) can exist within the same primary VLAN.
- **Isolated VLANs** achieve complete isolation between all ports inside them, so creating more than one is unnecessary.


No, **hosts in secondary private VLANs (PVLANs)** cannot communicate directly with the **primary VLAN** itself. The **primary VLAN** acts as a framework to group secondary VLANs (isolated or community), but it does not have standalone hosts. Communication rules are defined based on **secondary VLANs and promiscuous ports**, not between secondary and primary VLANs.

Here is a breakdown of how communication works within a PVLAN structure:

---

## **PVLAN Communication Rules**  

| **Source**                 | **Destination**           | **Can Communicate?** | **Explanation**                     |
|----------------------------|---------------------------|----------------------|-------------------------------------|
| Isolated VLAN Device        | Other Isolated Devices    | No                   | No direct communication between isolated hosts. |
| Isolated VLAN Device        | Community VLAN Device     | No                   | Communication is restricted between different secondary VLAN types. |
| Community VLAN Device       | Same Community Device     | Yes                  | Communication allowed within the same community. |
| Community VLAN Device       | Different Community VLAN  | No                   | Communication restricted between different community VLANs. |
| Secondary VLAN Device       | Promiscuous Port          | Yes                  | Secondary VLAN hosts can access promiscuous ports (typically gateways). |

---

## **Explanation**

1. **Primary VLAN (parent) is a logical grouping**, not a separate broadcast domain for hosts. There are no independent hosts that exist directly in the primary VLAN. It only acts as the umbrella VLAN for secondary VLANs.

2. **Secondary VLANs (isolated or community)** determine communication behavior:
   - **Isolated VLANs** prevent all intra-VLAN communication between hosts, allowing traffic only to **promiscuous ports** (e.g., router or gateway).
   - **Community VLANs** allow communication among members of the same VLAN but restrict it from other community or isolated VLANs.

3. **Promiscuous Ports** provide the bridge between secondary VLANs and the external world (e.g., a router or gateway).

---

## **How Can Hosts in Secondary VLANs Communicate with the Outside Network?**  
To communicate with the **external network or services** (such as internet or shared services), secondary VLAN hosts must use **promiscuous ports** configured on the primary VLAN. The promiscuous port can connect the PVLAN environment to routers, firewalls, or other networks.

---

## **Example Configuration for External Communication**  

In this example:
- **Primary VLAN**: 10  
- **Isolated VLAN**: 11  
- **Community VLAN**: 12  
- **Promiscuous Port**: Gi0/1 (connected to a router or gateway)

### **Step 1: Create VLANs and Associate with Primary VLAN**  
```css
Switch(config)# vlan 10
Switch(config-vlan)# private-vlan primary
Switch(config-vlan)# exit

Switch(config)# vlan 11
Switch(config-vlan)# private-vlan isolated
Switch(config-vlan)# exit

Switch(config)# vlan 12
Switch(config-vlan)# private-vlan community
Switch(config-vlan)# exit

Switch(config)# vlan 10
Switch(config-vlan)# private-vlan association 11,12
Switch(config-vlan)# exit
```

### **Step 2: Configure Promiscuous Port on the Primary VLAN**  
```css
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode private-vlan promiscuous
Switch(config-if)# switchport private-vlan mapping 10 11,12
Switch(config-if)# exit
```

### **Step 3: Assign Hosts to VLANs (Isolated and Community Ports)**  
- **Isolated Host** on Gi0/2:
  ```css
  Switch(config)# interface GigabitEthernet0/2
  Switch(config-if)# switchport mode private-vlan host
  Switch(config-if)# switchport private-vlan host-association 10 11
  Switch(config-if)# exit
  ```

- **Community Host** on Gi0/3:
  ```css
  Switch(config)# interface GigabitEthernet0/3
  Switch(config-if)# switchport mode private-vlan host
  Switch(config-if)# switchport private-vlan host-association 10 12
  Switch(config-if)# exit
  ```

---

## **Verification Commands**
```css
Switch# show vlan private-vlan
Switch# show interface GigabitEthernet0/1 switchport
Switch# show running-config | include private-vlan
```

---

## **Summary**
- **Hosts in secondary VLANs cannot directly communicate with a primary VLAN** because the primary VLAN is just a **logical container**.
- To access external resources (such as internet or gateways), **secondary VLAN hosts must use promiscuous ports**.
- Promiscuous ports serve as a bridge between **the PVLAN environment and the outside world**.

This setup is ideal for **multi-tenant networks**, where **isolation and segmentation** are required, but some devices still need **shared service access** via a **gateway**.

Yes, **secondary VLANs in a PVLAN setup** (whether isolated or community) can **only communicate through services or devices connected to promiscuous ports**. In other words, there is **no direct communication** between secondary VLANs and the **primary VLAN** itself communication occurs indirectly through the **gateway or service device** connected to a promiscuous port.

---

### **How Communication Works in PVLANs**
1. **Primary VLAN**:  
   - The **parent VLAN** that associates multiple **secondary VLANs** (isolated and community).
   - It does **not host any devices or allow direct communication** between itself and secondary VLAN hosts.

2. **Promiscuous Ports**:  
   - Devices connected to promiscuous ports (like routers, gateways, or shared services) can **communicate with any secondary VLAN** (isolated or community).
   - Promiscuous ports are typically **gateway interfaces, DHCP servers, firewalls, or other shared services**.

3. **Communication Path for Secondary VLAN Hosts**:
   - **Isolated VLAN hosts** can **only talk to promiscuous ports** and no other hosts, even within the same VLAN.
   - **Community VLAN hosts** can **talk to each other** and to **promiscuous ports** (but not with other secondary VLANs).

---

### **Use Cases of Promiscuous Ports and Shared Services**

- **Gateway Communication**:
  - Devices in isolated or community VLANs need access to the internet or external networks through a **router interface** configured on a **promiscuous port**.

- **DHCP Server**:
  - If a DHCP server is connected to a promiscuous port, hosts in **secondary VLANs** can receive IP addresses from it.

- **Firewall or Load Balancer**:
  - A **firewall** connected to a promiscuous port can inspect and filter traffic for all devices in the PVLAN.
  - A **load balancer** connected to a promiscuous port can provide services like distributing requests to a pool of servers.

---

### **Example: Internet Access via a Router (Promiscuous Port)**  

#### **Configuration Steps:**

- **Primary VLAN**: 100  
- **Isolated VLAN**: 101  
- **Community VLAN**: 102  
- **Router Interface** on Gi0/1 as **Promiscuous Port**  

#### **Step 1: Create and Associate VLANs**
```css
Switch(config)# vlan 100
Switch(config-vlan)# private-vlan primary
Switch(config)# vlan 101
Switch(config-vlan)# private-vlan isolated
Switch(config)# vlan 102
Switch(config-vlan)# private-vlan community
Switch(config)# vlan 100
Switch(config-vlan)# private-vlan association 101,102
```

#### **Step 2: Configure Router Interface as Promiscuous Port**
```css
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode private-vlan promiscuous
Switch(config-if)# switchport private-vlan mapping 100 101,102
```

#### **Step 3: Assign Host Ports**
- **Isolated Host** on Gi0/2:
  ```css
  Switch(config)# interface GigabitEthernet0/2
  Switch(config-if)# switchport mode private-vlan host
  Switch(config-if)# switchport private-vlan host-association 100 101
  ```
- **Community Host** on Gi0/3:
  ```css
  Switch(config)# interface GigabitEthernet0/3
  Switch(config-if)# switchport mode private-vlan host
  Switch(config-if)# switchport private-vlan host-association 100 102
  ```

---

### **How This Configuration Works**  

- The router connected to **Gi0/1** (promiscuous port) provides **internet access** to all hosts in isolated VLAN 101 and community VLAN 102.
- **Isolated hosts** can only talk to the router (promiscuous port) and not with each other.
- **Community hosts** can communicate with each other and the router.

---

### **Verification Commands**
Use these commands to ensure the PVLAN configuration is correct:
```css
Switch# show vlan private-vlan
Switch# show interface GigabitEthernet0/1 switchport
Switch# show running-config | include private-vlan
```

---

### **Conclusion**
In PVLAN configurations:
- **Secondary VLANs (isolated or community)** can **only communicate with promiscuous ports** to access shared services (like internet, DHCP, or firewalls). 
- There is **no direct communication** between secondary VLANs and the **primary VLAN**.
- Promiscuous ports act as the **bridge** between isolated and community VLANs and external services.

This architecture ensures **secure segmentation** in environments like **data centers**, **hotels**, or **service provider networks**, where tenants need to access shared services but must remain isolated from each other.


Yes, **pinging host-to-host can be impossible** depending on the **type of PVLAN configuration**. Let me explain the rules of **host communication** in PVLAN:

---

## **Can Hosts Ping Each Other in PVLANs?**

1. **Isolated VLAN**:  
   - **Ping between two hosts in the same isolated VLAN is impossible**.  
   - Reason: Isolated hosts are completely blocked from communicating with each other, even though they belong to the same isolated VLAN. They can only communicate with **promiscuous ports** (e.g., routers or gateways).

2. **Community VLAN**:  
   - **Ping is possible between hosts within the same community VLAN**.  
   - Reason: Community VLAN members are allowed to communicate freely with each other (including ping), but they are **isolated from other community VLANs and isolated VLANs**.

3. **Between Different Secondary VLANs (Isolated ↔ Community)**:  
   - **Ping between isolated VLAN and community VLAN is impossible**.  
   - Reason: Different secondary VLANs are isolated from one another by design, and they cannot communicate.

4. **Through Promiscuous Port (e.g., Gateway/Router)**:
   - **Hosts can ping external devices** (like a router or internet gateway) connected to a **promiscuous port**.
   - If two hosts are configured to use the same **promiscuous port**, they can communicate **indirectly through the router**, assuming proper routing exists.

---

## **Example Scenarios**

1. **Isolated VLAN: Ping Fails**  
   - VLAN 101: Host A on Gi0/2  
   - VLAN 101: Host B on Gi0/3  
   - **Result:** Ping from Host A to Host B will **fail** because isolated hosts cannot communicate.

2. **Community VLAN: Ping Works**  
   - VLAN 102: Host C on Gi0/4  
   - VLAN 102: Host D on Gi0/5  
   - **Result:** Ping from Host C to Host D will **succeed** since they are in the same community VLAN.

3. **Between Isolated and Community VLAN: Ping Fails**  
   - VLAN 101: Host A (Isolated VLAN)  
   - VLAN 102: Host C (Community VLAN)  
   - **Result:** Ping from Host A to Host C will **fail** since isolated and community VLANs are isolated from each other.

4. **Through Promiscuous Port**  
   - Host A (VLAN 101) and Host C (VLAN 102) both connect to a **router** on a **promiscuous port**.  
   - **Result:** They can ping each other **only if routing is enabled** on the router to forward packets between their subnets.

---

## **Why Host-to-Host Communication Fails in Isolated VLANs?**

- **Isolated VLANs** are designed to **enforce strict isolation** for security purposes. For example, in a hotel or a public network, each user’s device is in an isolated VLAN to prevent interaction with others, ensuring **privacy and security**.

- **Community VLANs** are more flexible, allowing devices within the same VLAN to communicate. This is useful in scenarios where members of a group (like employees in the same department) need to interact.

---

## **Verification of Connectivity**

You can test connectivity with the following commands:

1. **Ping between two hosts:**
   ```css
   Switch# ping 192.168.1.2
   ```

2. **Check PVLAN configuration:**
   ```css
   Switch# show vlan private-vlan
   Switch# show interface GigabitEthernet0/2 switchport
   ```

3. **Check routes on a router (if using promiscuous port):**
   ```css
   Router# show ip route
   ```

---

## **Summary**

- **Ping between hosts in the same isolated VLAN**: Not possible.  
- **Ping between hosts in the same community VLAN**: Possible.  
- **Ping between isolated and community VLANs**: Not possible.  
- **Ping between secondary VLAN hosts through a router**: Possible if routing is configured.

This behavior ensures **secure segmentation** and prevents **unwanted communication**, which is especially valuable in **shared environments** like data centers, hotels, and service providers.