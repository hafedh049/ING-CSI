When dealing with **Access Control Lists (ACLs)** in networking, ACLs are typically used in routers, switches, and firewalls to control traffic and determine whether packets are allowed or denied based on criteria like IP address, protocol, or port.

Here are the key ACL commands for configuring ACLs on **Cisco devices**, as they are commonly used in networking:

### 1. **Standard ACL**

Standard ACLs control traffic based on the **source IP address**.

#### Command to Define a Standard ACL:

```python
access-list <number> permit|deny <source_ip> [wildcard_mask]
```

Example to Permit Traffic from a Specific Source:

```python
access-list 10 permit 192.168.1.0 0.0.0.255
```

This command permits traffic from the IP range `192.168.1.0` to `192.168.1.255`.

#### Applying Standard ACL to an Interface:

```python
interface <interface_type> <number>
ip access-group <acl_number> in|out
```

Example:

```python
interface gigabitEthernet 0/1
ip access-group 10 in
```

This applies ACL 10 to incoming traffic on interface `GigabitEthernet 0/1`.

### 2. **Extended ACL**

Extended ACLs control traffic based on **source and destination IP address**, **protocols**, and **port numbers**.

#### Command to Define an Extended ACL:

```python
access-list <number> permit|deny <protocol> <source_ip> [wildcard_mask] <destination_ip> [wildcard_mask] [operator <port_number>]
```

Example to Permit HTTP Traffic:

```python
access-list 100 permit tcp 192.168.1.0 0.0.0.255 172.16.0.0 0.0.0.255 eq 80
```

This permits HTTP traffic (TCP port 80) from the `192.168.1.0/24` network to the `172.16.0.0/24` network.

#### Applying Extended ACL to an Interface:

```python
interface <interface_type> <number>
ip access-group <acl_number> in|out
```

Example:

```python
interface fastEthernet 0/0
ip access-group 100 in
```

This applies ACL 100 to incoming traffic on interface `FastEthernet 0/0`.

### 3. **Named ACL**

Named ACLs allow you to give ACLs descriptive names instead of numbers.

#### Command to Define a Named Standard ACL:

```python
ip access-list standard <name>
permit|deny <source_ip> [wildcard_mask]
```

Example:

```python
ip access-list standard ALLOW_VIKAS
permit 192.168.1.10
```

#### Command to Define a Named Extended ACL:

```python
ip access-list extended <name> permit|deny <protocol> <source_ip> [wildcard_mask] <destination_ip> [wildcard_mask] [operator <port_number>]
```

Example:

```python
ip access-list extended WEB_ACCESS
permit tcp 192.168.1.0 0.0.0.255 any eq 80
```

This allows HTTP traffic from the `192.168.1.0/24` network.

#### Applying Named ACL to an Interface:

```python
interface <interface_type> <number>
ip access-group <name> in|out
```

Example:

```python
interface serial 0/1
ip access-group WEB_ACCESS in
```

### 4. **Show ACLs**

To display all configured ACLs:

```python
show access-lists
```

To display a specific ACL:

```python
show access-lists <acl_number or name>
```

Example:

```python
show access-lists 10
```

### 5. **Remove ACL**

To remove an ACL from an interface:

```python
no ip access-group <acl_number> in|out
```

Example:

```python
no ip access-group 10 in
```

To delete a specific ACL:

```python
no access-list <acl_number>
```

Example:

```python
no access-list 10
```

### 6. **Remarks in ACLs**

You can add a comment or description to an ACL using the `remark` keyword:

```python
access-list <number> remark <description>
```

Example:

```python
access-list 100 remark Allow HTTP traffic from 192.168.1.0/24 to 172.16.0.0/24
```

---

In the command `access-list <number> permit|deny <source_ip> [wildcard_mask]`, the **`number`** parameter refers to the **ACL number**, which specifies the type of Access Control List (ACL) you're creating. The number determines whether the ACL is a **standard** or an **extended** ACL and also distinguishes different ACLs on the device.

### 1. **Standard ACL Numbers:**

- **1 to 99**: Standard ACLs (legacy range)
- **1300 to 1999**: Expanded range for Standard ACLs

**Standard ACL** only filters traffic based on the **source IP address**.

Example:

```python
access-list 10 permit 192.168.1.0 0.0.0.255
```

In this example, `10` is the standard ACL number, permitting traffic from the IP range `192.168.1.0/24`.

### 2. **Extended ACL Numbers:**

- **100 to 199**: Extended ACLs (legacy range)
- **2000 to 2699**: Expanded range for Extended ACLs

**Extended ACL** can filter traffic based on several criteria such as **source and destination IP address**, **protocol**, **port numbers**, etc.

Example:

```python
access-list 100 permit tcp 192.168.1.0 0.0.0.255 any eq 80
```

In this case, `100` is the extended ACL number, allowing HTTP (TCP port 80) traffic from the IP range `192.168.1.0/24` to any destination.

### 3. **Other ACL Number Ranges:**
There are specific number ranges for other types of ACLs as well:

- **700 to 799**: IPX Standard ACLs
- **800 to 899**: IPX Extended ACLs
- **1100 to 1199**: Extended 48-bit MAC address ACLs

In most modern networks, you'll primarily use **Standard** and **Extended** ACLs in the ranges outlined above.

If you want to **permit** or **deny** traffic from a **single machine** (i.e., a specific host) in an Access Control List (ACL), you can specify the exact IP address of the machine with a **wildcard mask** of `0.0.0.0`. This wildcard mask means that all 32 bits of the IP address must match exactly, allowing you to filter traffic to or from just that one specific host.

Alternatively, you can use the keyword `host` to make the command more readable.

### Example 1: Using Wildcard Mask `0.0.0.0`

To **permit** traffic from a single machine with the IP address `192.168.1.10`:

```python
access-list 10 permit 192.168.1.10 0.0.0.0
```

To **deny** traffic from the same machine:

```python
access-list 10 deny 192.168.1.10 0.0.0.0
```

In this case, the wildcard mask `0.0.0.0` ensures that only `192.168.1.10` is matched.

### Example 2: Using the `host` Keyword

Alternatively, you can use the `host` keyword, which is a shorthand for specifying a single host without using a wildcard mask.

To **permit** traffic from a single machine:

```python
access-list 10 permit host 192.168.1.10
```

To **deny** traffic from that machine:

```python
access-list 10 deny host 192.168.1.10
```

### Explanation

- **`access-list 10`**: Refers to ACL number 10 (Standard ACL).
- **`permit` or `deny`**: Specifies whether to allow or block traffic.
- **`host 192.168.1.10`**: Refers to the specific IP address of the single machine you want to control.

Both methods (wildcard mask or `host` keyword) achieve the same result: controlling access for a specific machine with the IP `192.168.1.10`.

### Applying the ACL to an Interface

After defining the ACL, apply it to an interface (inbound or outbound) using the `ip access-group` command.

Example:

```python
interface gigabitEthernet 0/1
ip access-group 10 in
```

This applies the ACL to inbound traffic on the `GigabitEthernet 0/1` interface, controlling access for the specific machine.

The **generic mask** in networking is often referred to as the **wildcard mask**, which is used in Access Control Lists (ACLs) to match specific bits of an IP address. A wildcard mask is the **inverse** of a subnet mask.

- In a **wildcard mask**, a bit of `0` means that the corresponding bit in the IP address must match exactly.
- A bit of `1` means that the corresponding bit can vary (i.e., it is a "don't care" bit).

To calculate the wildcard mask from a given **subnet mask**, simply **subtract** each octet of the subnet mask from 255.

### Step-by-Step Calculation

1. **Write down the subnet mask.**
2. **Subtract each octet from 255** to find the wildcard mask.

### Example 1: Subnet Mask `255.255.255.0` (For a /24 network)

- Subnet mask: `255.255.255.0`
- Subtract from 255:

  ```python
  255 - 255 = 0
  255 - 255 = 0
  255 - 255 = 0
  255 - 0   = 255
  ```

- Wildcard mask: `0.0.0.255`

This wildcard mask `0.0.0.255` means that the first three octets of the IP address must match exactly, while the last octet can vary.

### Example 2: Subnet Mask `255.255.255.128` (For a /25 network)

- Subnet mask: `255.255.255.128`
- Subtract from 255:

  ```python
  255 - 255 = 0
  255 - 255 = 0
  255 - 255 = 0
  255 - 128 = 127
  ```

- Wildcard mask: `0.0.0.127`

This wildcard mask `0.0.0.127` means that the first three octets must match exactly, but the last octet can vary in the range `0-127`.

### Example 3: Subnet Mask `255.255.0.0` (For a /16 network)

- Subnet mask: `255.255.0.0`
- Subtract from 255:

  ```python
  255 - 255 = 0
  255 - 255 = 0
  255 - 0   = 255
  255 - 0   = 255
  ```

- Wildcard mask: `0.0.255.255`

This wildcard mask `0.0.255.255` means the first two octets must match exactly, while the last two octets can vary.

### Example 4: Subnet Mask `255.255.255.252` (For a /30 network)

- Subnet mask: `255.255.255.252`
- Subtract from 255:

```python
  255 - 255 = 0
  255 - 255 = 0
  255 - 255 = 0
  255 - 252 = 3
  ```

- Wildcard mask: `0.0.0.3`

This wildcard mask `0.0.0.3` means the first three octets must match exactly, while the last octet can vary in the range `0-3`.

### Formula for Wildcard Mask Calculation

$$
\text{Wildcard Mask} = 255.255.255.255 - \text{Subnet Mask}
$$

### Summary of Key Wildcard Masks

| Subnet Mask         | Wildcard Mask     | CIDR   |
|---------------------|-------------------|--------|
| 255.255.255.0       | 0.0.0.255         | /24    |
| 255.255.255.128     | 0.0.0.127         | /25    |
| 255.255.255.192     | 0.0.0.63          | /26    |
| 255.255.255.224     | 0.0.0.31          | /27    |
| 255.255.255.240     | 0.0.0.15          | /28    |
| 255.255.255.252     | 0.0.0.3           | /30    |
| 255.255.0.0         | 0.0.255.255       | /16    |
| 255.0.0.0           | 0.255.255.255     | /8     |

By calculating the wildcard mask, you can define **ranges** or **single IP addresses** in ACLs and control traffic more effectively in networking devices like routers and firewalls.

In a **standard Access Control List (ACL)**, we filter traffic based only on the **source IP address**. If you want to allow or deny traffic from **any IP address** (i.e., all IP addresses), you can use the keyword `any` in your ACL rule.

### Example of a Standard ACL Using `any`

1. **Deny all traffic**:

```python
access-list 10 deny any
```

This denies traffic from any IP address. All incoming traffic, regardless of the source, will be denied.

2. **Permit all traffic**:

```python
access-list 10 permit any
```

This allows traffic from any IP address. All incoming traffic, regardless of the source, will be permitted.

3. **Combination Example**:

In practice, you often combine rules to deny or permit specific traffic, and then you use `any` as a "catch-all" rule at the end.

```python
access-list 10 deny 192.168.1.10 0.0.0.0  # Deny traffic from a specific host
access-list 10 permit any                 # Permit traffic from any other source
```

In this example:

- Traffic from the host `192.168.1.10` will be **denied**.
- Traffic from any other source (any IP) will be **permitted**.

### Applying the ACL to an Interface

To apply this ACL to an interface for **incoming** traffic:

```python
interface gigabitEthernet 0/1
ip access-group 10 in
```

This applies the standard ACL number `10` to the `GigabitEthernet 0/1` interface, controlling all incoming traffic based on the ACL rules defined above.

### Explanation of `any`

- `any`: This keyword means any source IP address (i.e., all possible IP addresses).
- In the case of standard ACLs, which filter only based on the source IP, `any` matches every possible IP address.

This setup can be useful when you want to allow or deny traffic globally or after permitting or blocking specific addresses.

In **standard ACLs**, you can only filter traffic based on the **source IP address**, but **not** by ports or protocols. If you want to **deny** or **permit** traffic based on **ports** or **protocols**, you need to use an **extended ACL**.

### **Extended ACLs** allow filtering by:
- **Source IP**
- **Destination IP**
- **Protocols** (like TCP, UDP, ICMP)
- **Ports** (like HTTP, HTTPS, FTP, etc.)

### Syntax of Extended ACL:
```python
access-list <number> permit|deny <protocol> <source_ip> [wildcard_mask] <destination_ip> [wildcard_mask> [operator <port_number>]
```

### Common Operators for Ports:
- **eq**: Equal to (a specific port)
- **gt**: Greater than a specific port number
- **lt**: Less than a specific port number
- **range**: Range of port numbers

### Example: Denying or Permitting Traffic Based on Ports

#### Example 1: Permit HTTP (port 80) Traffic

To **permit** HTTP traffic from any source to the network `192.168.1.0/24`:
```python
access-list 100 permit tcp any 192.168.1.0 0.0.0.255 eq 80
```

Explanation:
- `100`: Extended ACL number.
- `permit`: Allows traffic.
- `tcp`: Matches TCP traffic.
- `any`: Matches traffic from any source IP.
- `192.168.1.0 0.0.0.255`: Matches the destination network `192.168.1.0/24`.
- `eq 80`: Matches traffic to destination port **80** (HTTP).

#### Example 2: Deny Telnet (port 23) Traffic

To **deny** Telnet traffic from `192.168.1.10` to any destination:
```python
access-list 101 deny tcp 192.168.1.10 0.0.0.0 any eq 23
```

Explanation:
- `101`: Extended ACL number.
- `deny`: Blocks traffic.
- `tcp`: Matches TCP traffic.
- `192.168.1.10 0.0.0.0`: Matches traffic from the source `192.168.1.10`.
- `any`: Matches any destination.
- `eq 23`: Matches traffic to port **23** (Telnet).

#### Example 3: Permit HTTPS (port 443) Traffic

To **permit** HTTPS traffic from the network `10.1.1.0/24` to any destination:
```python
access-list 102 permit tcp 10.1.1.0 0.0.0.255 any eq 443
```

Explanation:
- `102`: Extended ACL number.
- `permit`: Allows traffic.
- `tcp`: Matches TCP traffic.
- `10.1.1.0 0.0.0.255`: Matches the source network `10.1.1.0/24`.
- `any`: Matches any destination.
- `eq 443`: Matches traffic to port **443** (HTTPS).

#### Example 4: Deny a Range of Ports (20-25)

To **deny** traffic from `192.168.2.0/24` to ports in the range **20-25** (common FTP and mail ports):
```python
access-list 103 deny tcp 192.168.2.0 0.0.0.255 any range 20 25
```

Explanation:
- `103`: Extended ACL number.
- `deny`: Blocks traffic.
- `tcp`: Matches TCP traffic.
- `192.168.2.0 0.0.0.255`: Matches the source network `192.168.2.0/24`.
- `any`: Matches any destination.
- `range 20 25`: Blocks traffic to ports **20-25**.

### Applying the Extended ACL to an Interface

After defining your extended ACL, apply it to an interface:

```python
interface fastEthernet 0/0
ip access-group 100 in
```

This applies extended ACL `100` to incoming traffic on `FastEthernet 0/0`.

---

### Summary of Key Points:
- **Standard ACLs**: Only filter based on source IP (cannot filter by ports).
- **Extended ACLs**: Allow filtering by source/destination IP, protocols, and ports.
- Use `eq`, `gt`, `lt`, and `range` operators to filter traffic based on **ports** in extended ACLs.

In an **Access Control List (ACL)**, the keywords `ip any any` and `deny any any` are commonly used to permit or deny **all IP traffic**. Here's a breakdown of each:

### 1. **`ip any any`** (Permit All Traffic)

The command `ip any any` means **allow all IP traffic** regardless of the source or destination IP addresses.

- `ip`: This means you are applying the rule to all IP-based traffic, which includes TCP, UDP, ICMP, etc.
- `any`: This keyword means any source IP address.
- `any`: This keyword also means any destination IP address.

### Example: Permit All IP Traffic
```python
access-list 100 permit ip any any
```

- **ACL number `100`**: Indicates that this is an extended ACL.
- **`permit ip any any`**: Permits all IP traffic from any source to any destination.

This rule is useful when you want to allow all IP traffic and apply no restrictions.

### 2. **`deny any any`** (Deny All Traffic)

The command `deny any any` means **deny all traffic**, regardless of the source or destination.

- `deny`: This keyword blocks traffic.
- `any`: Matches any source IP address.
- `any`: Matches any destination IP address.

### Example: Deny All IP Traffic
```python
access-list 101 deny ip any any
```

- **ACL number `101`**: Indicates that this is an extended ACL.
- **`deny ip any any`**: Denies all IP traffic from any source to any destination.

This rule is typically used at the end of an ACL to block any traffic that wasn't explicitly permitted earlier in the list. 

---

### Important Use Cases

#### **Deny All Traffic Explicitly:**

You can use `deny any any` to block all traffic that doesn't match any previous permit rules.

```python
access-list 110 deny ip any any
```

This denies all IP traffic not explicitly allowed by preceding permit statements.

#### **Implicit Deny in ACLs:**
- **Note**: In most ACLs, there is an **implicit `deny any any`** at the end, which means if traffic doesn't match any previous rules, it will be denied. This happens automatically, even if you don't manually add `deny any any`. However, it's sometimes explicitly added for clarity.

#### **Permit All Traffic:**

If you want to allow all IP traffic, the following ACL would achieve that:

```python
access-list 120 permit ip any any
```

This rule explicitly permits all traffic and can be useful if you want to ensure unrestricted access.

---

### Example of Combined Use

In real-world configurations, you often combine specific permit/deny rules with `ip any any` or `deny any any` to control traffic more granularly. Here's an example:

```python
access-list 130 deny ip 192.168.1.0 0.0.0.255 any  # Deny traffic from the 192.168.1.0/24 network
access-list 130 permit ip any any                  # Permit all other traffic
```

Explanation:
1. The first rule denies all traffic coming from the **192.168.1.0/24** network.
2. The second rule permits all other traffic (from any source to any destination).

---

### **Summary**:
- **`ip any any`**: Allows all IP traffic from any source to any destination.
- **`deny any any`**: Blocks all traffic from any source to any destination.
- **Implicit `deny any any`**: Even without specifying it, all traffic not matching a permit statement will be denied by default in an ACL.

