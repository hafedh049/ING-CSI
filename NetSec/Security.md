### **1. DHCP Starvation (Rogue DHCP)**

**Definition:**

- **DHCP Starvation** is a type of **DoS (Denial of Service)** attack where an attacker floods the DHCP server with a large number of DHCP requests, effectively exhausting the pool of available IP addresses. This prevents legitimate clients from obtaining an IP address.
- **Rogue DHCP** occurs when an unauthorized DHCP server is introduced into the network, providing incorrect IP configuration to clients and potentially leading to Man-in-the-Middle (MITM) attacks.

**How it Works:**

- An attacker sends many DHCP requests using fake MAC addresses, causing the DHCP server to allocate all IP addresses in its pool.
- In the case of rogue DHCP, the attacker configures a rogue DHCP server that offers malicious IP settings to the clients (such as default gateway and DNS server).

**Countermeasures:**

- **DHCP Snooping**: A security feature that only allows trusted ports to send DHCP requests and responses.
- **Dynamic ARP Inspection (DAI)**: Validates ARP packets to prevent MITM attacks by rogue DHCP servers.
- **IP Source Guard**: Blocks unauthorized DHCP traffic based on IP-MAC bindings.

---

### **2. DHCP Spoofing**

**Definition:**

- **DHCP Spoofing** is a technique used by attackers to inject fraudulent DHCP packets into the network, impersonating a legitimate DHCP server.
- The goal is to assign incorrect network configuration (like a malicious gateway or DNS server) to clients, enabling **MITM** or **DoS** attacks.

**How it Works:**

- The attacker configures a rogue DHCP server on the network and starts offering IP addresses to clients.
- Clients may accept the malicious DHCP offer, which may provide incorrect network settings and compromise the security of the network.

**Countermeasures:**

- **DHCP Snooping**: Ensures that only trusted devices (like the legitimate DHCP server) can respond to DHCP requests.
- **Port Security**: Limits the number of MAC addresses per port to prevent unauthorized devices from connecting to the network.

---

### **3. MAC Flooding**

**Definition:**

- **MAC Flooding** is a type of attack that targets network switches by overwhelming the switch's MAC address table (also known as the CAM table) with fake MAC addresses.
- This causes the switch to act like a hub (broadcasting traffic to all ports) instead of using its MAC address table for forwarding, resulting in network slowdowns and potential security breaches.

**How it Works:**

- The attacker sends a large number of frames with different source MAC addresses to the switch, filling up the MAC address table.
- Once the table is full, the switch broadcasts frames to all connected ports, exposing sensitive data to attackers.

**Countermeasures:**

- **Port Security**: Limits the number of allowed MAC addresses on each port.
- **MAC Address Table Aging**: Reduces the time a MAC address stays in the table, forcing the switch to refresh the entries more frequently.
- **Switchport Security**: Configures switches to automatically disable ports if a MAC flood is detected.

---

### **4. Spanning Tree Protocol (STP)**

**Definition:**

- **Spanning Tree Protocol (STP)** is a network protocol used to prevent loops in Ethernet networks by dynamically choosing the best path for data to travel through the network.
- STP ensures that only one path is active at a time, avoiding **broadcast storms** and **duplicate packets** caused by loops.

**How it Works:**

- STP elects a **Root Bridge** and uses **Bridge Protocol Data Units (BPDU)** to exchange topology information between switches.
- STP disables redundant paths to avoid loops and selects the most optimal route based on cost and port states.

**Countermeasures (for STP-related attacks):**

- **BPDU Guard**: Prevents unauthorized devices from sending BPDUs to the network, securing the STP topology.
- **Root Guard**: Ensures that only the root bridge (legitimate switch) can be elected as the root, preventing malicious switches from becoming the root bridge.

---

### **5. Rapid Spanning Tree Protocol (RSTP)**

**Definition:**

- **Rapid Spanning Tree Protocol (RSTP)** is an evolution of STP, designed to provide faster convergence (recovery from network topology changes) by quickly reconfiguring the network in case of a failure or topology change.
- RSTP is backward-compatible with STP but provides quicker failover and recovery times.

**How it Works:**

- RSTP improves upon STP by immediately transitioning ports to **Forwarding** state instead of waiting for timers, significantly reducing convergence time.
- It uses new port states, such as **Discovered**, which are quicker to process.

**Countermeasures (for RSTP-related attacks):**

- **BPDU Guard**: Protects RSTP by preventing unauthorized BPDU transmissions that could disrupt the protocol.
- **PortFast**: Quickly transitions edge ports to **Forwarding** state, ensuring minimal delays for connected devices.
- **Root Guard**: Prevents malicious switches from becoming the root bridge, ensuring the integrity of the RSTP topology.

---

### **Summary of Countermeasures**

|**Attack/Threat**|**Countermeasures**|
|---|---|
|**DHCP Starvation**|DHCP Snooping, Dynamic ARP Inspection (DAI), IP Source Guard|
|**DHCP Spoofing**|DHCP Snooping, Port Security|
|**MAC Flooding**|Port Security, MAC Address Table Aging, Switchport Security|
|**STP Attacks (e.g., Root Bridge Attack)**|BPDU Guard, Root Guard|
|**RSTP Attacks**|BPDU Guard, PortFast, Root Guard|

---

### **Conclusion**

- **DHCP Starvation and Spoofing**: Malicious DHCP attacks can disrupt the network by exhausting IP address pools or redirecting traffic. Countermeasures like **DHCP Snooping** and **Port Security** can prevent unauthorized DHCP servers.
- **MAC Flooding**: This attack targets network switches and can cause traffic disruptions. Countermeasures like **Port Security** and **MAC Address Table Aging** help mitigate the impact.
- **STP and RSTP**: Both protocols are crucial for preventing loops and ensuring network stability. Protecting the STP/RSTP topology with features like **BPDU Guard**, **Root Guard**, and **PortFast** helps prevent malicious network topology changes.

These attacks and protocols highlight the importance of network security and the need for proactive measures to protect against common vulnerabilities.