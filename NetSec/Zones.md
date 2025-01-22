### **What is a Zone in Cisco Zone-Based Firewall (ZBFW)?**

A **zone** in Cisco's Zone-Based Policy Firewall (ZBFW) is a **logical grouping** of network interfaces that share the same security policies. Zones define the security level for each interface and how they interact with each other in terms of traffic flow. Each zone can be used to group together interfaces based on the type of network they connect to, for example, an "inside" network or an "outside" network.

- **Intra-zone communication**: Communication within the same zone is **permitted** by default.
- **Inter-zone communication**: Communication between different zones is **denied** by default unless explicit policies are defined.

---

### **Defaults in ZBFW**

- **Intra-zone traffic** (communication between interfaces in the same zone) is **allowed** by default.
- **Inter-zone traffic** (communication between interfaces in different zones) is **denied** by default, and you need to configure policies to allow traffic between zones.

---

### **Steps to Create Zones in ZBFW**

1. **Define Zones**:
    - Use the `zone security` command to define a zone.
    - Zones are typically named based on their role (e.g., inside, outside, DMZ).

#### **Command:**

```bash
Router(config)# zone security <zone-name>
```

**Example:**

```bash
Router(config)# zone security inside
Router(config)# zone security outside
```

---

### **Step 1: Assign Interfaces to Zones**

Next, assign interfaces to the zones created above using the `zone-member security` command. You can assign one interface to only one zone at a time.

#### **Command:**

```bash
Router(config)# interface <interface-name>
Router(config-if)# zone-member security <zone-name>
```

**Example:**

```bash
Router(config)# interface GigabitEthernet0/1
Router(config-if)# zone-member security inside

Router(config)# interface GigabitEthernet0/0
Router(config-if)# zone-member security outside
```

---

### **Step 2: Create Class Maps**

A **class map** is used to define the traffic or conditions (based on ACLs, protocols, or other match criteria) that will be applied in policies. Class maps match packets against certain criteria and categorize them for processing.

#### **Command:**

```bash
Router(config)# class-map type inspect match-any <class-map-name>
Router(config-cmap)# match <match-type> <parameters>
```

- **Match types** are used to match specific criteria like traffic, protocol, or IP addresses.

#### **Match Types in Class Maps**:

1. **match access-group**: Match traffic based on an ACL.
2. **match protocol**: Match traffic based on a specified protocol (e.g., HTTP, DNS).
3. **match address**: Match traffic based on source or destination IP address.
4. **match traffic**: Match on traffic types, e.g., matching traffic types like FTP, HTTP, etc.

**Example**: Create a class map to match HTTP traffic using an ACL.

```bash
Router(config)# class-map type inspect match-any http-traffic
Router(config-cmap)# match access-group 100
```

---

### **Step 3: Create Policy Maps**

A **policy map** defines how packets matched by class maps are treated. Policy maps are used to specify the actions to be taken on the matched traffic (like inspecting, passing, or dropping the traffic).

#### **Command:**

```bash
Router(config)# policy-map type inspect <policy-map-name>
Router(config-pmap)# class <class-map-name>
Router(config-pmap-c)# inspect
```

- **Actions** can be `inspect`, `pass`, or `drop`.

**Example**: Create a policy map that inspects HTTP traffic from the `http-traffic` class map.

```bash
Router(config)# policy-map type inspect http-policy
Router(config-pmap)# class http-traffic
Router(config-pmap-c)# inspect
```

---

### **Step 4: Create Zone Pairs**

A **zone pair** defines the relationship between two zones and specifies the policies applied to traffic between those zones. You configure the zone pair to define the **source** and **destination** zones and link them to the policy map.

#### **Command:**

```bash
Router(config)# zone-pair security <zone-pair-name> source <source-zone> destination <destination-zone>
Router(config-sec-zone-pair)# service-policy type inspect <policy-map-name>
```

**Example**: Define a zone pair between the `inside` and `outside` zones, using the `http-policy` policy map.

```bash
Router(config)# zone-pair security inside-to-outside source inside destination outside
Router(config-sec-zone-pair)# service-policy type inspect http-policy
```

---

### **Step 5: Activate the Zone-Based Firewall**

To activate the **Zone-Based Policy Firewall**, you must apply the security policies and enable inspection on the router.

#### **Command:**

```bash
Router(config)# zone security
Router(config)# service-policy type inspect <policy-map-name>
```

- This command activates the inspection policies between the defined zones and enables ZBFW on the router.

---

### **Example Full Configuration**

```bash
Router(config)# zone security inside
Router(config)# zone security outside

Router(config)# interface GigabitEthernet0/1
Router(config-if)# zone-member security inside

Router(config)# interface GigabitEthernet0/0
Router(config-if)# zone-member security outside

Router(config)# class-map type inspect match-any http-traffic
Router(config-cmap)# match access-group 100

Router(config)# policy-map type inspect http-policy
Router(config-pmap)# class http-traffic
Router(config-pmap-c)# inspect

Router(config)# zone-pair security inside-to-outside source inside destination outside
Router(config-sec-zone-pair)# service-policy type inspect http-policy
```

---

### **Summary of Key Concepts**

1. **Zone**: A logical group of interfaces. Each interface is part of a zone, and communication between interfaces within the same zone is allowed by default, while communication between zones is denied by default.
    
2. **Class Map**: Defines the traffic that will be matched (e.g., based on protocols, IP addresses, or ACLs).
    
3. **Policy Map**: Specifies what action to take (e.g., inspect, pass, drop) for matched traffic.
    
4. **Zone Pair**: Defines the relationship between two zones and links the security policies to be applied between them.
    
5. **Inspection**: Ensures that traffic between zones is inspected and managed according to defined policies.
    

---

### **Match Types in Class Maps**:

- **access-group**: Matches traffic based on an ACL.
- **protocol**: Matches specific traffic protocols (e.g., HTTP, FTP).
- **address**: Matches traffic based on source/destination IP address.
- **traffic**: Matches specific traffic types such as HTTP, FTP, etc.

>[!tip]
>Zones can handle $1^+$ interfaces

