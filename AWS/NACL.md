**Network Access Control List (NACL)** is a security feature in AWS that acts as a stateless firewall for controlling inbound and outbound traffic at the **subnet** level. Unlike **Security Groups**, which operate at the **instance level**, NACLs provide an additional layer of security at the **network level**. They are used to manage traffic flowing into and out of one or more subnets in a Virtual Private Cloud (VPC).

### Key Characteristics of NACLs

1. **Stateless**:
    
    - **Stateless** means that NACLs do not remember the state of a connection. If you allow inbound traffic on a specific port (e.g., HTTP on port 80), you must also explicitly allow outbound traffic for the response to be returned.
    - Every rule in a NACL is evaluated independently. For example, if you allow incoming traffic on port 80 but do not allow outgoing traffic, the response will be blocked.
2. **Subnet-Level Security**:
    
    - NACLs apply to all instances within a specific subnet. If you attach a NACL to a subnet, it will govern all EC2 instances within that subnet, providing network-level filtering.
    - NACLs are ideal for enforcing security at the subnet level across multiple resources.
3. **Inbound and Outbound Rules**:
    
    - **Inbound Rules**: Control incoming traffic to the subnet from the internet or other subnets.
    - **Outbound Rules**: Control the outgoing traffic from the subnet to the internet or other subnets.
    - Both inbound and outbound rules can be set to allow or deny traffic based on IP address ranges, protocols, and port numbers.
4. **Allow and Deny Rules**:
    
    - Unlike security groups, which only allow rules (and implicitly deny all others), NACLs allow you to specify both **allow** and **deny** rules.
    - This gives you more granular control over what traffic is permitted or denied.
5. **Default NACL**:
    
    - When you create a new VPC, AWS automatically creates a default NACL for it. The default NACL allows all inbound and outbound traffic, similar to a default security group.
    - You can modify or replace the default NACL to meet your specific security requirements.
6. **Custom NACLs**:
    
    - You can create custom NACLs to define your own set of inbound and outbound rules for your VPC's subnets. This allows for greater flexibility and customization of your network security settings.
7. **Rule Evaluation Order**:
    
    - Rules in NACLs are evaluated in **numerical order**, starting from the lowest numbered rule. As soon as a rule matches the incoming or outgoing traffic, the decision is made and no further rules are evaluated.
    - This makes the order of the rules important for ensuring correct behavior.
8. **Per-Subnet Application**:
    
    - A single NACL can be associated with one or more subnets within a VPC. However, each subnet can only have one NACL associated at a time.
    - Multiple subnets can share the same NACL, which makes it easier to apply consistent security policies across similar subnets.

### Structure of NACL Rules

Each NACL rule consists of the following components:

- **Rule Number**: A unique identifier for each rule (between 1 and 32766). The rules are evaluated in ascending order, starting with the lowest numbered rule.
- **Type of Rule**: Whether the rule is an inbound or outbound rule.
- **Protocol**: The protocol for the traffic (e.g., TCP, UDP, ICMP).
- **Port Range**: The port range to allow or deny (e.g., 80 for HTTP, 22 for SSH).
- **Source/Destination**: The IP address range (in CIDR notation) from which the traffic originates or is destined (e.g., `0.0.0.0/0` for all traffic, `192.168.1.0/24` for a specific subnet).
- **Action**: Whether to **Allow** or **Deny** the traffic that matches the rule.
- **Logging**: Optionally, you can enable logging for a NACL to capture information about the traffic being evaluated by the rules.

### Example of NACL Rule Configuration

- **Inbound Rule**: Allow HTTP (port 80) from any IP address:
    
    ```
    Rule Number: 100
    Action: Allow
    Protocol: TCP
    Port Range: 80
    Source: 0.0.0.0/0 (any IP)
    ```
    
- **Outbound Rule**: Deny all traffic to a specific IP address range:
    
    ```
    Rule Number: 200
    Action: Deny
    Protocol: TCP
    Port Range: 0-65535 (all ports)
    Destination: 10.0.0.0/24
    ```
    

### Differences Between Security Groups and NACLs

|Feature|Security Group|NACL|
|---|---|---|
|**Level of Operation**|Instance-level|Subnet-level|
|**Statefulness**|Stateful (automatic response traffic)|Stateless (requires explicit rules)|
|**Type of Rules**|Allow-only|Allow and Deny rules|
|**Default Behavior**|Deny by default|Allow all traffic by default|
|**Evaluation Order**|N/A|Rules are evaluated in order of rule number (lowest to highest)|
|**Multiple Associations**|Can be associated with multiple instances|Can be associated with multiple subnets|
|**Rule Granularity**|Limited to IPs and ports|Allows for more granular control over IP ranges, ports, and protocols|

### Common Use Cases for NACLs

1. **Subnet-Level Protection**:
    
    - NACLs can be used to apply broad network-level controls to all instances within a subnet, ensuring that traffic is filtered consistently across the subnet.
2. **Public vs. Private Subnet Access**:
    
    - You can use NACLs to control traffic between public and private subnets. For example, a public subnet might have inbound rules allowing HTTP and HTTPS traffic from the internet, while a private subnet could deny all inbound traffic but allow communication with the public subnet.
3. **Restricting Access to Sensitive Resources**:
    
    - NACLs can be used to create a security perimeter around sensitive resources (like databases) by blocking traffic from untrusted sources at the subnet level, while allowing internal communication between trusted instances.
4. **Tightening Security on Specific IP Ranges**:
    
    - NACLs allow you to block traffic from specific IP address ranges or regions, giving you more granular control over which IPs can reach your subnet.

### Limitations of NACLs

1. **No Deep Packet Inspection**:
    
    - NACLs only operate based on IP address, port, and protocol, so they do not inspect the contents of the packets. For more advanced filtering, you would need to use other security mechanisms.
2. **Limited to 20 Inbound and 20 Outbound Rules per NACL** (Default Limit):
    
    - Although you can create multiple NACLs, each NACL can only have a limited number of rules. This can be restrictive in large, complex networks.
3. **Only Applies to Subnets**:
    
    - Unlike security groups, which apply to individual instances, NACLs operate at the subnet level. Therefore, NACLs provide less granular control over individual instance traffic.
4. **Complexity in Rule Order**:
    
    - Since NACL rules are evaluated in order from the lowest to the highest rule number, managing and ordering these rules correctly can become complex in larger setups.

### Conclusion

Network Access Control Lists (NACLs) provide a powerful, additional layer of security for controlling network traffic at the subnet level in AWS VPCs. They allow fine-grained control over which traffic can enter or leave a subnet, complementing security groups that operate at the instance level. By using both security groups and NACLs together, you can create a multi-layered defense strategy that protects your AWS resources from unauthorized access.