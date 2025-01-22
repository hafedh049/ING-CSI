### 1. **Standard ACL**

- **Syntax**:
    
    ```css
    access-list <1-99 | 1300-1999> permit|deny <source> [wildcard mask]
    ip access-group <ACL-number> in|out
    ```
    
- **Example**:
    
    ```css
    access-list 10 permit 192.168.1.0 0.0.0.255
    interface GigabitEthernet0/1
    ip access-group 10 in
    ```
    

---

### 2. **Extended ACL**

- **Syntax**:
    
    ```css
    access-list <100-199 | 2000-2699> permit|deny <protocol> <source> <wildcard mask> <destination> <wildcard mask> [eq port]
    ip access-group <ACL-number> in|out
    ```
    
- **Example**:
    
    ```css
    access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
    interface GigabitEthernet0/1
    ip access-group 100 in
    ```
    

---

### 3. **Named ACL**

- **Syntax**:
    
    ```css
    ip access-list standard|extended <name>
    permit|deny <criteria>
    ip access-group <name> in|out
    ```
    
- **Example**:
    
    ```css
    ip access-list extended WEB_FILTER
    permit tcp any any eq 443
    deny tcp any any eq 80
    interface GigabitEthernet0/1
    ip access-group WEB_FILTER in
    ```
    

---
### 5. **Reflexive ACL**

- Reflexive ACLs allow temporary, dynamic entries to be created for return traffic initiated from within the network.
- **Syntax**:
    
    ```css
    ip access-list extended <name>
    permit|deny <protocol> <source> <wildcard mask> <destination> <wildcard mask> reflect <name>
    evaluate <name>
    ```
    
- **Example**:
    
    ```css
    ip access-list extended OUTBOUND
    permit tcp 192.168.1.0 0.0.0.255 any reflect OUTBOUND_TRAFFIC
    
    ip access-list extended INBOUND
    evaluate OUTBOUND_TRAFFIC
    
    interface GigabitEthernet0/1
    ip access-group OUTBOUND out
    ip access-group INBOUND in
    ```
    

---

### 6. **Dynamic ACL (Lock-and-Key ACL)**

- Dynamic ACLs require users to authenticate before gaining access. Entries are created dynamically after authentication.
- **Syntax**:
    
    ```css
    access-list <ACL-number> dynamic <name> permit|deny <protocol> <source> <wildcard mask> <destination> <wildcard mask>
    line vty <number>
    login local
    autocommand access-enable timeout <minutes>
    ```
    
- **Example**:
    
    ```css
    access-list 101 dynamic DYNAMIC_PERMIT permit ip any any
    
    line vty 0 4
    login local
    autocommand access-enable timeout 5
    
    interface GigabitEthernet0/1
    ip access-group 101 in
    ```

---
### **Steps to Calculate a Wildcard Mask:**

1. **Write the Subnet Mask in Decimal**  
    Example: For subnet mask `255.255.255.0`.
    
2. **Subtract Each Octet from 255**
    
    ```c
    Wildcard Mask = 255.255.255.255 - Subnet Mask
    ```
    
3. **Result is the Wildcard Mask**  
    Example:
    
    ```c
    Subnet Mask:   255.255.255.0
    Wildcard Mask: 0.0.0.255
    ```
    

---

### **Examples for Common Subnet Masks:**

| **Subnet Mask** | **Wildcard Mask** |
| --------------- | ----------------- |
| **255.0.0.0**       | **0.255.255.255**     |
| **255.255.0.0**     | **0.0.255.255**       |
| **255.255.255.0**   | **0.0.0.255**         |
| **255.255.255.240** | **0.0.0.15**          |
| **255.255.255.252** | **0.0.0.3**           |

---

### **Key Notes:**

- Wildcard masks are commonly used in **ACLs** to specify ranges of IP addresses.
- A **binary trick**: Subtracting `1` for each `0` in the subnet mask gives you `1` in the wildcard mask.

The **"established"** keyword in an Access Control List (ACL) is used to permit or deny traffic based on the state of a connection, specifically for **TCP connections** that have completed the initial three-way handshake.

### **What Does "Established" Mean?**

- **TCP Three-Way Handshake**: A TCP connection goes through three phases:
    
    1. **SYN**: A client sends a request to open a connection.
    2. **SYN-ACK**: The server responds to the client with an acknowledgment.
    3. **ACK**: The client confirms the acknowledgment, and the connection is established.
- **Established State**: Once the handshake is completed and the connection is fully established, it is considered "established." Any traffic that is part of an ongoing, legitimate connection after the handshake can be considered **"established"**.
    

### **How Does the "Established" Keyword Work in ACLs?**

The **"established"** keyword is typically used in outbound ACLs to allow responses to inbound connections that were previously initiated from the internal network. It allows traffic from an internal source to respond to external services, such as web servers or DNS servers, after the initial connection is established.

### **Example Use of "Established" in ACL**

An example of using the **"established"** keyword in an ACL:

```plaintext
access-list 101 permit tcp any any established
```

- This ACL rule allows **return traffic** (response packets) for any TCP connection that has already been established. It ensures that once a connection is initiated from the inside (e.g., to a web server), the return traffic from the server can pass back to the internal network.
    
- The **"established"** keyword checks for the TCP **ACK** flag. It permits packets that are part of a valid, open connection, meaning it will allow response traffic (such as **SYN-ACK** and **ACK** packets) but not initial SYN requests.
    

### **Common Use Cases:**

1. **Allow Return Traffic**: It’s often used in outbound ACLs to permit return traffic for connections initiated by the internal network.
2. **Security**: It helps prevent unsolicited inbound traffic from entering the network (e.g., block random attempts at opening connections from the outside).

---

### **Conclusion**

The **"established"** keyword in ACLs is useful for allowing legitimate return traffic for already established TCP connections, enhancing security by ensuring that only response packets from valid connections are allowed while blocking unauthorized attempts to initiate connections.