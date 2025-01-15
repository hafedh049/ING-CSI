### **VLAN and Inter-VLAN Routing**

#### **What is a VLAN?**
A **Virtual Local Area Network (VLAN)** is a logical grouping of devices in a network that behave as if they are on the same physical network, even if they are on different switches. VLANs segment a network into smaller broadcast domains to improve performance, manageability, and security.

---

### **VLAN Configuration**

#### **Step 1: Define VLANs**
1. Enter VLAN configuration mode and assign an ID:
   ```c
   vlan <VLAN-ID>
   name <VLAN-NAME>
   ```

2. Example:
   ```c
   vlan 10
   name HR
   vlan 20
   name IT
   ```

#### **Step 2: Assign VLANs to Ports**
1. Access the interface configuration mode:
   ```c
   interface <interface-id>
   switchport mode access
   switchport access vlan <VLAN-ID>
   ```

2. Example:
   ```c
   interface f0/1
   switchport mode access
   switchport access vlan 10
   ```

#### **Step 3: Configure Trunk Ports (Between Switches or Router)**
1. Set the interface to trunk mode:
   ```c
   interface <interface-id>
   switchport mode trunk
   switchport trunk allowed vlan <vlan-ids>
   ```

2. Example:
   ```c
   interface f0/24
   switchport mode trunk
   switchport trunk allowed vlan 10,20
   ```

#### **Step 4: Verify VLAN Configuration**
   - View the VLAN table:
     ```c
     show vlan brief
     ```

---

### **Inter-VLAN Routing**

Inter-VLAN Routing allows communication between devices in different VLANs. It can be implemented using a **Router-on-a-Stick** or **Layer 3 Switch**.

---

#### **Option 1: Router-on-a-Stick**
A single physical router interface is divided into multiple subinterfaces, each associated with a VLAN.

##### **Step 1: Configure Router Subinterfaces**
1. Enter the interface configuration:
   ```c
   interface <interface-id>.<subinterface-number>
   encapsulation dot1Q <vlan-id>
   ip address <ip-address> <subnet-mask>
   ```

2. Example:
   ```c
   interface g0/0.10
   encapsulation dot1Q 10
   ip address 192.168.10.1 255.255.255.0

   interface g0/0.20
   encapsulation dot1Q 20
   ip address 192.168.20.1 255.255.255.0
   ```

##### **Step 2: Connect the Router to the Switch**
Ensure the router's interface is connected to a trunk port on the switch.

##### **Step 3: Enable Routing**
   - Enable IP routing on the router:
     ```c
     ip routing
     ```

##### **Step 4: Test**
   - Ping between VLANs to verify inter-VLAN communication.

---

#### **Option 2: Layer 3 Switch**

##### **Step 1: Enable Routing on the Switch**
1. Enter global configuration mode:
   ```c
   ip routing
   ```

##### **Step 2: Create VLAN Interfaces (SVIs)**
1. Configure an SVI (Switched Virtual Interface) for each VLAN:
   ```c
   interface vlan <vlan-id>
   ip address <ip-address> <subnet-mask>
   no shutdown
   ```

2. Example:
   ```c
   interface vlan 10
   ip address 192.168.10.1 255.255.255.0
   no shutdown

   interface vlan 20
   ip address 192.168.20.1 255.255.255.0
   no shutdown
   ```

##### **Step 3: Assign VLANs and Trunks**
   - Follow the VLAN configuration steps for the switch, ensuring trunk links are set up between switches.

##### **Step 4: Test**
   - Ping between devices in different VLANs to verify routing.

---

### **How to Configure VLAN and Inter-VLAN Routing in GNS3**

#### **1. Setup the Topology**
- Add switches, routers, and PCs in GNS3.
- Connect devices with Ethernet cables.

#### **2. Configure VLANs on Switches**
1. Access the switch CLI:
   ```c
   enable
   configure terminal
   vlan 10
   name HR
   vlan 20
   name IT
   ```
2. Assign VLANs to ports:
   ```c
   interface f0/1
   switchport mode access
   switchport access vlan 10

   interface f0/2
   switchport mode access
   switchport access vlan 20
   ```

3. Set up a trunk:
   ```c
   interface f0/24
   switchport mode trunk
   switchport trunk allowed vlan 10,20
   ```

#### **3. Configure Inter-VLAN Routing**
##### **Using Router-on-a-Stick:**
1. Configure subinterfaces:
   ```c
   interface g0/0.10
   encapsulation dot1Q 10
   ip address 192.168.10.1 255.255.255.0

   interface g0/0.20
   encapsulation dot1Q 20
   ip address 192.168.20.1 255.255.255.0
   ```
2. Test routing by pinging between VLANs.

##### **Using Layer 3 Switch:**
1. Enable routing:
   ```c
   ip routing
   ```
2. Create SVIs:
   ```c
   interface vlan 10
   ip address 192.168.10.1 255.255.255.0
   no shutdown

   interface vlan 20
   ip address 192.168.20.1 255.255.255.0
   no shutdown
   ```
3. Assign VLANs and test.

---

### **Verification Commands**
- Check VLAN configuration:
  ```c
  show vlan brief
  ```

- Verify trunk links:
  ```c
  show interfaces trunk
  ```

- Check IP routing:
  ```c
  show ip route
  ```

- Test connectivity:
  ```c
  ping <destination-ip>
  ```
