## ✅ Let's Correct That: Use **Zone-Based Firewall (ZBF)**

Zone-Based Firewall involves:

1. **Creating zones**
    
2. **Assigning interfaces to zones**
    
3. **Creating class-maps (traffic types)**
    
4. **Creating policy-maps (actions on that traffic)**
    
5. **Creating zone-pairs (direction of traffic and policies)**
    

---

### 🔧 **Step-by-Step ZBF Configuration**

Assuming Cisco IOS-style CLI:

#### 1. **Create Security Zones**

```bash
zone security INSIDE
zone security OUTSIDE
zone security DMZ
zone security DMZ_WEB
```

---

#### 2. **Assign Interfaces to Zones**

```bash
interface Ethernet0/2
 ip address 172.21.8.1 255.255.252.0
 zone-member security INSIDE
 no shutdown

interface Ethernet0/3
 ip address 209.165.2.1 255.255.255.240
 zone-member security OUTSIDE
 no shutdown

interface Ethernet0/0
 ip address 172.21.1.1 255.255.255.128
 zone-member security DMZ
 no shutdown

interface Ethernet0/1
 ip address 172.21.1.129 255.255.255.192
 zone-member security DMZ_WEB
 no shutdown
```

---

#### 3. **Create Class Maps** (match traffic types)

```bash
class-map type inspect match-(all|any) CLASS-HTTP
 match protocol http

class-map type inspect match-any CLASS-DNS
 match protocol dns

class-map type inspect match-any CLASS-SMTP
 match protocol smtp
```

---

#### 4. **Create Policy Maps** (define actions)

```bash
policy-map type inspect POLICY-INSIDE-TO-DMZ
 class type inspect CLASS-HTTP
  inspect
 class type inspect CLASS-DNS
  inspect

policy-map type inspect POLICY-DMZ-TO-OUTSIDE
 class type inspect CLASS-SMTP
  inspect
```

---

#### 5. **Create Zone Pairs** (define traffic direction)

```bash
zone-pair security ZP-INSIDE-DMZ source INSIDE destination DMZ
 service-policy type inspect POLICY-INSIDE-TO-DMZ

zone-pair security ZP-DMZ-OUTSIDE source DMZ destination OUTSIDE
 service-policy type inspect POLICY-DMZ-TO-OUTSIDE
```

> You can add more zone-pairs as needed, depending on communication flows.

---

### 🧠 Why Use ZBF?

- **Granular control** of inter-zone traffic.
    
- **Stateful inspection** per policy.
    
- **Modern and secure** — unlike traditional ACLs on interfaces.
    
- **Scalable** for complex architectures.
    

---

