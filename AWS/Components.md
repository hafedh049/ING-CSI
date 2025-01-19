
![[Pasted image 20250117111039.png]]

### **1. Regions**

- **Definition:** Geographical locations around the world where AWS operates its data centers.
- **Examples:**
    - **us-east-1** (North Virginia)
    - **eu-west-1** (Ireland)
    - **ap-southeast-1** (Singapore)
- **Purpose:**
    - Allow users to deploy resources closer to their users.
    - Enable compliance with data residency requirements.
    - Provide high availability by distributing resources across different regions.

---

### **2. Availability Zones (AZs)** (2-6)

- **Definition:** Isolated data centers within a region.
- **Characteristics:**
    - Each AZ is physically separate with independent power, cooling, and networking.
    - Typically, a region has 2–6 AZs.
    - Low-latency connections between AZs (usually <10ms).
- **Purpose:**
    - Provide fault tolerance by deploying resources across multiple AZs.
    - High availability for applications (e.g., load balancing across AZs).

---

### **3. Edge Locations**

- **Definition:** Points of presence (PoPs) used by services like **Amazon CloudFront** for caching content.
- **Purpose:**
    - Reduce latency for users by delivering content from locations closer to them.
    - Support services like CloudFront, AWS Shield, and Route 53.

---

### **4. Local Zones**

- **Definition:** Extensions of AWS Regions that place compute, storage, and database services closer to users.
- **Purpose:**
    - Enable ultra-low latency for applications (e.g., gaming, video rendering).
    - Provide local data residency options.

---

A **PoP (Point of Presence)** is a network access point that enables data and services to be delivered closer to users for improved performance, reduced latency, and higher availability. It is a physical location where AWS or another service provider hosts caching servers or other infrastructure to provide low-latency network access.

### Key Characteristics of PoPs:

1. **Purpose**:  
    PoPs primarily serve as hubs for content delivery, network traffic management, and edge computing.
    
2. **Used in Content Delivery**:  
    AWS uses PoPs extensively in services like **Amazon CloudFront** (its content delivery network or CDN) to cache content closer to end users.
    
3. **Distributed Globally**:  
    PoPs are located in many cities worldwide, separate from AWS Regions and Availability Zones. They are part of AWS’s global edge network.
    
4. **Low Latency**:  
    PoPs minimize the time it takes for user requests to reach AWS services by providing local access points for content and applications.
    
5. **Supports Services**:  
    AWS services like **CloudFront**, **AWS Global Accelerator**, and **AWS WAF** use PoPs to improve performance and security.
    

### Difference Between PoPs and AZs:

- **PoPs**:
    
    - Focused on content delivery and edge services.
    - Positioned in densely populated cities to be closer to end users.
    - Do not host compute or database resources directly.
- **AZs (Availability Zones)**:
    
    - Focused on hosting core AWS infrastructure for compute, storage, and databases.
    - Located within AWS Regions, designed for high availability and fault tolerance.


---

### **5. AWS Outposts**

- **Definition:** Fully managed AWS infrastructure delivered on-premises.
- **Purpose:**
    - Extend AWS services to on-premises environments.
    - Ideal for workloads that require low latency or local data processing.

---

### **6. VPC (Virtual Private Cloud)**

- **Definition:** Isolated virtual networks within AWS where users can deploy resources.
- **Components:**
    - **Subnets:** Divide a VPC into smaller IP ranges (public and private subnets).
    - **Route Tables:** Control traffic routing within the VPC.
    - **Internet Gateway (IGW):** Connects a VPC to the internet.
    - **NAT Gateway:** Allows private subnets to access the internet securely.
    - **Endpoints:** Connect VPCs to AWS services without using the internet.
- **Purpose:**
    - Secure and customizable networking.
    - Support for hybrid cloud environments with VPN or Direct Connect.

---

### **7. Elastic Load Balancing (ELB)**

- **Definition:** Distributes incoming traffic across multiple targets (e.g., EC2 instances) in one or more AZs.
- **Types:**
    - Application Load Balancer (ALB)
    - Network Load Balancer (NLB)
    - Gateway Load Balancer (GLB)
- **Purpose:**
    - Ensure high availability and fault tolerance.
    - Improve scalability of applications.

---

### **8. Amazon Route 53**

- **Definition:** Scalable Domain Name System (DNS) service.
- **Purpose:**
    - Translate domain names into IP addresses.
    - Manage DNS records for applications.
    - Provide routing features like latency-based routing and geolocation routing.

---

### **9. AWS Direct Connect**

- **Definition:** Dedicated network connection between on-premises data centers and AWS.
- **Purpose:**
    - Provide a secure and low-latency connection to AWS services.
    - Avoid public internet for data transfer.

---

### **10. Transit Gateway**

- **Definition:** A central hub for connecting multiple VPCs and on-premises networks.
- **Purpose:**
    - Simplify large-scale networking across multiple environments.
    - Enable efficient and secure communication between VPCs.

---

### **11. Internet Gateway (IGW)**

- **Definition:** A component that connects a VPC to the public internet.
- **Purpose:**
    - Enable resources in public subnets to send and receive traffic from the internet.

---

### **12. NAT Gateway**

- **Definition:** A service that allows resources in private subnets to access the internet while remaining inaccessible from the internet.
- **Purpose:**
    - Maintain security by keeping private resources hidden from the internet.

---

### **13. AWS Global Infrastructure Map**

- **Components:**
    - **Region:** Group of AZs (e.g., US East).
    - **AZ:** Isolated fault domains (e.g., us-east-1a, us-east-1b).
    - **Edge Locations:** Global PoPs for caching and delivery.
    - **Local Zones:** Extend regions for ultra-low latency.
    - **Outposts:** On-premises AWS services.

---

### **14. Hybrid Connectivity**

- **Components:**
    - **VPN (Virtual Private Network):** Securely connect on-premises environments to AWS VPCs.
    - **AWS Transit Gateway:** Hub for hybrid and multi-VPC connections.
    - **Direct Connect:** Dedicated, private connection to AWS.

---

### **15. Security and Compliance Components**

- **IAM (Identity and Access Management):** Centralized access control.
- **AWS Shield:** DDoS protection.
- **AWS WAF (Web Application Firewall):** Protect web apps from malicious traffic.
- **CloudHSM:** Hardware-based key management.

