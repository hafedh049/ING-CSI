## 1. **Static NAT (One-to-One)**  
**Description**: Maps a single private IP address to a specific public IP address.  
**Use Case**: Used when a service (e.g., a web server) needs to be accessed publicly.

### **Configuration Example**:

- Private IP: `192.168.1.10`  
- Public IP: `203.0.113.10`

```css
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# exit

R1(config)# ip nat inside source static 192.168.1.10 203.0.113.10
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 2. **Dynamic NAT (Many-to-Many)**  
**Description**: Maps internal private IP addresses to a pool of public IP addresses.

### **Configuration Example**:

- Internal Network: `192.168.1.0/24`  
- Public IP Pool: `203.0.113.10 - 203.0.113.20`

```css
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

R1(config)# ip nat pool MY_POOL 203.0.113.10 203.0.113.20 netmask 255.255.255.0
R1(config)# ip nat inside source list 1 pool MY_POOL

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 3. **Port Address Translation (PAT) / NAT Overload (Many-to-One)**  
**Description**: Multiple internal devices share a single public IP address with different port numbers.

### **Configuration Example**:

- Public IP: `203.0.113.1`  
- Internal Network: `192.168.1.0/24`

```css
R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

R1(config)# ip nat inside source list 1 interface GigabitEthernet0/0 overload

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 4. **NAT Hairpinning (NAT Loopback)**  
**Description**: Allows internal devices to access a service on the public IP from within the private network.

### **Configuration Example**:

- Internal Server: `192.168.1.10`  
- Public IP: `203.0.113.10`

```css
R1(config)# ip nat inside source static 192.168.1.10 203.0.113.10

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 5. **Dual NAT (Bidirectional NAT)**  
**Description**: Translates both source and destination IP addresses, allowing communication between two private networks.

### **Configuration Example**:

- Network A: `192.168.1.0/24` ↔ NAT A  
- Network B: `10.0.0.0/24` ↔ NAT B

```css
R1(config)# ip nat inside source static 192.168.1.10 10.0.0.1
R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 10.0.0.254 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

---

## 6. **Carrier-Grade NAT (CGNAT)**  
**Description**: Used by ISPs to allow multiple customers to share a single public IPv4 address.

### **Configuration Example**:

- Customer Network: `10.0.0.0/24`  
- Public IP: `203.0.113.1`

```css
R1(config)# access-list 1 permit 10.0.0.0 0.0.0.255

R1(config)# ip nat inside source list 1 interface GigabitEthernet0/0 overload

R1(config)# interface GigabitEthernet0/1
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# ip nat inside
R1(config-if)# exit

R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 203.0.113.1 255.255.255.0
R1(config-if)# ip nat outside
R1(config-if)# exit
```

Here are some **commands to configure PAT (Port Address Translation)** in networking environments like **Cisco routers**. This will walk you through how to set up **many-to-many PAT**.

---

## **Many-to-Many PAT Configuration Example on Cisco Router**

### **Scenario:**
- **Internal network:** 192.168.1.0/24 (Private IPs)
- **Available public IPs:** 203.0.113.1, 203.0.113.2 (from ISP)
- **Router's WAN interface:** `GigabitEthernet0/1`
- **Router's LAN interface:** `GigabitEthernet0/0`

---

### **1. Set Up the Interfaces**
```css
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 192.168.1.1 255.255.255.0
Router(config-if)# no shutdown

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 203.0.113.1 255.255.255.0
Router(config-if)# ip address 203.0.113.2 255.255.255.0 secondary
Router(config-if)# no shutdown
```

---

### **2. Configure Access Control List (ACL)**
This ACL will permit the traffic from the **internal network** to the internet.

```css
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
```

---

### **3. Configure the PAT (Many-to-Many) Translation Rules**
We define **NAT pools** for the public IP addresses.

```css
Router(config)# ip nat pool PUBLIC_POOL 203.0.113.1 203.0.113.2 netmask 255.255.255.0
```

---

### **4. Map Internal Network to Public IP Pool with PAT**
```css
Router(config)# ip nat inside source list 1 pool PUBLIC_POOL overload
```

The `overload` keyword allows **many internal IPs to use the same public IP with different ports**. With a **pool**, the router will assign multiple public IPs dynamically.

---

### **5. Assign NAT to Interfaces**
```css
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip nat inside

Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip nat outside
```

---

### **6. Verify the Configuration**
Check if NAT and PAT are working correctly with the following commands.

```css
Router# show ip nat translations
```

This will show the active NAT translations with **internal-to-external IP and port mappings**.

```css
Router# show ip nat statistics
```

This will display the NAT statistics, including **how many translations are in use**.

---

### **Example Output of `show ip nat translations`**
```
Pro  Inside global       Inside local         Outside local      Outside global
tcp  203.0.113.1:1025    192.168.1.10:12345   172.217.14.238:80  172.217.14.238:80
tcp  203.0.113.2:1026    192.168.1.11:23456   172.217.14.238:443 172.217.14.238:443
```

---

### **Summary of Commands:**
1. **Assign IPs to interfaces:**  
   `interface GigabitEthernet0/1`  
   `ip address 203.0.113.1 255.255.255.0`
2. **Create an ACL:**  
   `access-list 1 permit 192.168.1.0 0.0.0.255`
3. **Create a NAT pool:**  
   `ip nat pool PUBLIC_POOL 203.0.113.1 203.0.113.2 netmask 255.255.255.0`
4. **Set up PAT:**  
   `ip nat inside source list 1 pool PUBLIC_POOL overload`
5. **Assign NAT to interfaces:**  
   `ip nat inside / outside`

---

This setup ensures multiple devices on the **internal network** can access the internet using **multiple public IPs** via **many-to-many PAT**.