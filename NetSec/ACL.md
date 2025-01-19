### **Access Control List (ACL)**

An **Access Control List (ACL)** is a set of rules applied to a router or switch interface that controls the traffic flow in a network. It filters network traffic by permitting or denying packets based on criteria such as source/destination IP address, protocol type, port number, etc.

---

### **Types of ACLs**
There are two primary types of ACLs:

#### 1. **Standard ACL**
   - Filters traffic based only on the **source IP address**.
   - It is less granular and typically used closer to the destination.
   - Number range: **1–99** **&** **1300-1999** (numbered) or named ACLs.

   **Commands**:
   ```css
   access-list <number> {permit|deny} [host] <source> [wildcard-mask]
   ```
   Example:
   ```css
   access-list 10 permit 192.168.1.0 0.0.0.255
   access-list 10 deny any
   ```

   **Application**:
   ```css
   interface <interface-id>
   ip access-group <number> {in|out}
   ```

#### 2. **Extended ACL**
   - Filters traffic based on **source and destination IP addresses**, **protocols**, and **ports**.
   - Number range: **100–199** **&** **2000-2699** (numbered) or named ACLs.
   - Typically applied closer to the source.

   **Commands**:
   ```css
   access-list <number> {permit|deny} <protocol> <source> <wildcard-mask> <destination> <wildcard-mask> [operator port]
   ```
   Example:
   ```css
   access-list 100 permit tcp 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255 eq 80
   access-list 100 deny ip any any
   ```

   **Application**:
   ```css
   interface <interface-id>
   ip access-group <number> {in|out}
   ```

---

### **Named ACL**
   - Offers more flexibility and better management.
   - Can be either standard or extended.

   **Commands**:
   ```css
   ip access-list {standard|extended} <name>
   {permit|deny} <criteria>
   ```
   Example:
   ```css
   ip access-list extended BLOCK_WEBSITES
   deny tcp any any eq 80
   permit ip any any
   ```

   **Application**:
   ```css
   interface <interface-id>
   ip access-group BLOCK_WEBSITES {in|out}
   ```

---

### **How to Configure ACL in GNS3**

#### **1. Setup the Topology**
1. Open GNS3 and create a network topology.
2. Add routers, switches, and end devices (PCs).
3. Connect devices using appropriate links (Ethernet, serial, etc.).

#### **2. Configure IP Addressing**
Assign IP addresses to all devices:
```css
interface <interface-id>
ip address <IP address> <subnet mask>
no shutdown
```

#### **3. Configure Routing (if needed)**
If your topology spans multiple networks, configure routing protocols like OSPF, RIP, or static routes.

#### **4. Apply ACLs**
- Configure the ACL on the router:
   - For a **Standard ACL**:
     ```css
     access-list 1 permit 192.168.1.0 0.0.0.255
     access-list 1 deny any
     interface g0/1
     ip access-group 1 in
     ```
   - For an **Extended ACL**:
     ```css
     access-list 101 deny tcp 192.168.1.0 0.0.0.255 any eq 80
     access-list 101 permit ip any any
     interface g0/1
     ip access-group 101 in
     ```

#### **5. Verify the ACL**
Check if the ACL is working using:
```css
show access-lists
show ip interface <interface-id>
```

#### **6. Test Traffic**
Use ping or HTTP requests from PCs to verify whether traffic is permitted or denied as per the ACL rules.

---

### **Examples in GNS3**

#### **Scenario 1: Block ICMP Traffic**
1. Create an ACL to deny ICMP:
   ```css
   access-list 110 deny icmp any any
   access-list 110 permit ip any any
   ```
2. Apply it on an interface:
   ```css
   interface g0/0
   ip access-group 110 in
   ```
3. Test using a ping from one PC to another.

#### **Scenario 2: Allow Only HTTP Traffic**
1. Configure the ACL:
   ```css
   access-list 120 permit tcp any any eq 80
   access-list 120 deny ip any any
   ```
2. Apply the ACL:
   ```css
   interface g0/0
   ip access-group 120 out
   ```
3. Test by trying to access a web server (allowed) and other services (denied).