### **Port Address Translation (PAT)**

#### **What is PAT?**
**Port Address Translation (PAT)**, also known as **NAT Overload**, is a method of Network Address Translation (NAT) that allows multiple devices in a private network to share a single public IP address. PAT achieves this by mapping multiple private IP addresses to a single public IP address while differentiating traffic using port numbers.

---

### **Key Features of PAT**
1. **Efficient Address Usage**: PAT allows thousands of devices to access the internet using one public IP address.
2. **Port Mapping**: PAT uses source port numbers to track individual connections.
3. **Dynamic Translation**: Automatically assigns public IP and ports as needed.

---

### **Types of NAT**
- **Static NAT**: Maps one private IP to one public IP.
- **Dynamic NAT**: Maps a pool of private IPs to a pool of public IPs.
- **PAT (Dynamic NAT with Overload)**: Maps multiple private IPs to a single public IP using port numbers.

---

### **PAT Configuration on a Cisco Router**

#### **Step 1: Configure the Inside and Outside Interfaces**
1. Define the inside and outside interfaces:
   ```c
   interface <inside-interface>
   ip address <ip-address> <subnet-mask>
   ip nat inside
   no shutdown

   interface <outside-interface>
   ip address <ip-address> <subnet-mask>
   ip nat outside
   no shutdown
   ```

2. Example:
   ```c
   interface g0/0
   ip address 192.168.1.1 255.255.255.0
   ip nat inside
   no shutdown

   interface g0/1
   ip address 203.0.113.1 255.255.255.0
   ip nat outside
   no shutdown
   ```

---

#### **Step 2: Define the Access Control List (ACL)**
Create an ACL to identify the private IP addresses that need to be translated:
```c
access-list <number> permit <source-ip> <wildcard-mask>
```

Example:
```c
access-list 1 permit 192.168.1.0 0.0.0.255
```

---

#### **Step 3: Configure PAT**
Enable PAT by linking the ACL to the outside interface and enabling overload:
```c
ip nat inside source list <acl-number> interface <outside-interface> overload
```

Example:
```c
ip nat inside source list 1 interface g0/1 overload
```

---

#### **Step 4: Save and Verify the Configuration**
1. Save the configuration:
   ```c
   write memory
   ```

2. Verify NAT translations:
   ```c
   show ip nat translations
   ```

3. Verify NAT statistics:
   ```c
   show ip nat statistics
   ```

---

### **How to Configure PAT in GNS3**

#### **1. Setup the Topology**
1. Add a router, a switch, and multiple PCs.
2. Connect the PCs to the router via the switch.
3. Connect the router's outside interface to the internet or a simulated public network.

#### **2. Configure the Router**
1. Configure the inside and outside interfaces:
   ```c
   interface g0/0
   ip address 192.168.1.1 255.255.255.0
   ip nat inside
   no shutdown

   interface g0/1
   ip address 203.0.113.1 255.255.255.0
   ip nat outside
   no shutdown
   ```

2. Create an ACL to define the private network:
   ```c
   access-list 1 permit 192.168.1.0 0.0.0.255
   ```

3. Configure PAT:
   ```c
   ip nat inside source list 1 interface g0/1 overload
   ```

---

#### **3. Configure the PCs**
1. Assign IP addresses to the PCs in the private range (e.g., 192.168.1.2, 192.168.1.3, etc.).
2. Set the default gateway of the PCs to the router's inside interface (192.168.1.1).

---

#### **4. Test the Configuration**
1. Test connectivity from the PCs to an external device (e.g., a simulated web server with a public IP).
2. Use the following commands to verify PAT functionality:
   - Check NAT translations:
     ```c
     show ip nat translations
     ```
   - Check NAT statistics:
     ```c
     show ip nat statistics
     ```

---

### **Example Configuration**

#### **Topology**
- **Inside Interface (g0/0)**: 192.168.1.0/24
- **Outside Interface (g0/1)**: 203.0.113.1/24
- **PC1**: 192.168.1.2
- **PC2**: 192.168.1.3

#### **Commands**
```c
! Configure interfaces
interface g0/0
ip address 192.168.1.1 255.255.255.0
ip nat inside
no shutdown

interface g0/1
ip address 203.0.113.1 255.255.255.0
ip nat outside
no shutdown

! Create an ACL for private IPs
access-list 1 permit 192.168.1.0 0.0.0.255

! Enable PAT
ip nat inside source list 1 interface g0/1 overload

! Verify NAT
show ip nat translations
show ip nat statistics
```

---

### **Verification Commands**
- View NAT translations:
  ```c
  show ip nat translations
  ```
- View NAT statistics:
  ```c
  show ip nat statistics
  ```
- Debug NAT events:
  ```c
  debug ip nat
  ```

---
