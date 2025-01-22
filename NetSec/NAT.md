Here are the syntax and examples for configuring **Static NAT**, **Dynamic NAT**, and **PAT (Port Address Translation)** in GNS3. These configurations are typically done on Cisco routers.

---

### **1. Static NAT**

Static NAT maps a private IP address to a specific public IP address, one-to-one. This is often used for servers or devices that need to be accessible from the internet.

**Syntax**:

```css
Router(config)# ip nat inside source static <private-ip> <public-ip>
Router(config)# interface <inside-interface>
Router(config-if)# ip nat inside
Router(config)# interface <outside-interface>
Router(config-if)# ip nat outside
```

**Example**:

- Private IP: `192.168.1.10`
- Public IP: `203.0.113.10`

```css
Router(config)# ip nat inside source static 192.168.1.10 203.0.113.10
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 203.0.113.1 255.255.255.0
Router(config-if)# ip nat outside
```

---

### **2. Dynamic NAT**

Dynamic NAT maps a pool of private IP addresses to a pool of public IP addresses. This is useful when you have more private IPs than public IPs but don’t need all private IPs translated simultaneously.

**Syntax**:

```css
Router(config)# ip nat pool <pool-name> <start-public-ip> <end-public-ip> netmask <subnet-mask>
Router(config)# access-list <acl-number> permit <source-network> <wildcard-mask>
Router(config)# ip nat inside source list <acl-number> pool <pool-name>
Router(config)# interface <inside-interface>
Router(config-if)# ip nat inside
Router(config)# interface <outside-interface>
Router(config-if)# ip nat outside
```

**Example**:

- Public IP Pool: `203.0.113.10` - `203.0.113.20`
- Private Network: `192.168.1.0/24`

```css
Router(config)# ip nat pool NAT_POOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 pool NAT_POOL
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 203.0.113.1 255.255.255.0
Router(config-if)# ip nat outside
```

---

### **3. PAT (Port Address Translation)**

PAT maps multiple private IP addresses to a single public IP address using unique ports. This is commonly used when there is a shortage of public IP addresses.

**Syntax**:

```css
Router(config)# ip nat inside source list <acl-number> interface <outside-interface> overload
Router(config)# access-list <acl-number> permit <source-network> <wildcard-mask>
Router(config)# interface <inside-interface>
Router(config-if)# ip nat inside
Router(config)# interface <outside-interface>
Router(config-if)# ip nat outside
```

**Example**:

- Single Public IP: `203.0.113.10`
- Private Network: `192.168.1.0/24`

```css
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# ip nat inside source list 1 interface FastEthernet0/1 overload
Router(config)# interface FastEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# ip nat inside
Router(config)# interface FastEthernet0/1
Router(config-if)# ip address 203.0.113.10 255.255.255.0
Router(config-if)# ip nat outside
```

---

### **Verification Commands**

1. **Check NAT Translations**:
    
    ```c
    Router# show ip nat translations
    ```
    
2. **Check NAT Statistics**:
    
    ```c
    Router# show ip nat statistics
    ```
    

---

### **Summary**

|Type|Use Case|Example Syntax|
|---|---|---|
|Static NAT|One-to-one mapping (e.g., server access).|`ip nat inside source static <private> <public>`|
|Dynamic NAT|Many-to-many mapping using a pool.|`ip nat pool <name> <start-ip> <end-ip>`|
|PAT|Many-to-one mapping (port-based).|`ip nat inside source list <acl> interface <interface> overload`|

Let me know if you need further details or help with configurations!