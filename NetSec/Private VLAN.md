### **Private VLAN (PVLAN)**

#### **What is a Private VLAN?**
A **Private VLAN (PVLAN)** is a specialized type of VLAN that provides further segmentation within a VLAN. It allows isolation between devices at Layer 2 while still allowing communication with a designated gateway or shared resources. This is commonly used in environments like data centers or shared hosting setups.

---

### **Private VLAN Types**
PVLANs have three types of ports:

1. **Promiscuous Port (P-Port):**
   - Can communicate with all other ports (Promiscuous, Isolated, and Community).
   - Typically used for gateways, routers, or shared servers.

2. **Isolated Port (I-Port):**
   - Can only communicate with the Promiscuous port.
   - Cannot communicate with other ports in the same VLAN.

3. **Community Port (C-Port):**
   - Can communicate with other ports in the same Community VLAN and with the Promiscuous port.
   - Cannot communicate with Isolated or other Community VLANs.

---

### **Steps to Configure PVLAN on Cisco Switch**

#### **Step 1: Enable Private VLANs**
Private VLANs are supported on switches that support **VTP Transparent mode**. Switch to Transparent mode:
```c
vtp mode transparent
```

---

#### **Step 2: Define Primary and Secondary VLANs**
1. Create the Primary VLAN:
   ```c
   vlan <primary-vlan-id>
   private-vlan primary
   ```
2. Create the Secondary VLANs (Isolated and/or Community):
   ```c
   vlan <secondary-vlan-id>
   private-vlan isolated
   vlan <secondary-vlan-id>
   private-vlan community
   ```

Example:
```c
vlan 100
private-vlan primary

vlan 101
private-vlan isolated

vlan 102
private-vlan community
```

---

#### **Step 3: Associate Secondary VLANs with the Primary VLAN**
Link the Secondary VLANs to the Primary VLAN:
```c
vlan <primary-vlan-id>
private-vlan association <secondary-vlan-id>,<secondary-vlan-id>
```

Example:
```c
vlan 100
private-vlan association 101,102
```

---

#### **Step 4: Configure Ports**
1. **Promiscuous Port Configuration:**
   Assign a port to the Primary VLAN and mark it as Promiscuous:
   ```c
   interface <interface-id>
   switchport mode private-vlan promiscuous
   switchport private-vlan mapping <primary-vlan-id> <secondary-vlan-ids>
   ```

   Example:
   ```c
   interface f0/1
   switchport mode private-vlan promiscuous
   switchport private-vlan mapping 100 101,102
   ```

2. **Isolated Port Configuration:**
   Assign a port to the Isolated VLAN:
   ```c
   interface <interface-id>
   switchport mode private-vlan host
   switchport private-vlan host-association <primary-vlan-id> <isolated-vlan-id>
   ```

   Example:
   ```c
   interface f0/2
   switchport mode private-vlan host
   switchport private-vlan host-association 100 101
   ```

3. **Community Port Configuration:**
   Assign a port to the Community VLAN:
   ```c
   interface <interface-id>
   switchport mode private-vlan host
   switchport private-vlan host-association <primary-vlan-id> <community-vlan-id>
   ```

   Example:
   ```c
   interface f0/3
   switchport mode private-vlan host
   switchport private-vlan host-association 100 102
   ```

---

#### **Step 5: Verify the Configuration**
1. Verify the VLAN configuration:
   ```c
   show vlan private-vlan
   ```
2. Verify private VLAN assignments:
   ```c
   show interfaces switchport
   ```
3. Test connectivity between devices.

---

### **How to Configure Private VLANs in GNS3**

#### **1. Setup the Topology**
1. Add a switch, PCs, and a router to your topology in GNS3.
2. Connect the devices using Ethernet cables.

#### **2. Configure Private VLANs on the Switch**
1. Create and associate the VLANs:
   ```c
   vlan 100
   private-vlan primary
   private-vlan association 101,102

   vlan 101
   private-vlan isolated

   vlan 102
   private-vlan community
   ```

2. Configure the ports:
   - **Promiscuous Port** (e.g., Router):
     ```c
     interface f0/1
     switchport mode private-vlan promiscuous
     switchport private-vlan mapping 100 101,102
     ```

   - **Isolated Port**:
     ```c
     interface f0/2
     switchport mode private-vlan host
     switchport private-vlan host-association 100 101
     ```

   - **Community Port**:
     ```c
     interface f0/3
     switchport mode private-vlan host
     switchport private-vlan host-association 100 102
     ```

#### **3. Configure the Router**
1. Assign an IP address to the router interface in the Primary VLAN:
   ```c
   interface g0/0
   ip address 192.168.1.1 255.255.255.0
   no shutdown
   ```

2. Test connectivity from PCs in the Isolated and Community VLANs to the router.

#### **4. Test the Private VLAN Setup**
1. **Devices in the same Community VLAN**:
   - Should communicate with each other and the Promiscuous port.

2. **Devices in the Isolated VLAN**:
   - Should only communicate with the Promiscuous port.

3. Verify using **ping** or other tools.

---

### **Commands Summary**
- Define Primary and Secondary VLANs:
  ```c
  vlan <primary-id>
  private-vlan primary
  vlan <secondary-id>
  private-vlan {isolated|community}
  ```
- Associate Secondary VLANs:
  ```c
  vlan <primary-id>
  private-vlan association <secondary-ids>
  ```
- Configure Ports:
  - Promiscuous:
    ```c
    switchport mode private-vlan promiscuous
    switchport private-vlan mapping <primary-id> <secondary-ids>
    ```
  - Host (Isolated/Community):
    ```c
    switchport mode private-vlan host
    switchport private-vlan host-association <primary-id> <secondary-id>
    ```
