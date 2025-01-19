### **1. Basics of EC2**

- **What is EC2?**
    - A web service that provides scalable compute capacity in the AWS cloud.
    - Allows users to launch virtual servers (instances) on-demand. **(REGION SPECIFIC)**
- **Use Cases:**
    - Hosting websites and applications.
    - Running backend services, databases, or machine learning models.
    - Performing batch processing and high-performance computing.

---

### **2. EC2 Instances**

- **Instance Types:**
    
    - Categorized by compute, memory, storage, and networking capabilities.
        - **General Purpose:** Balanced performance (e.g., t3, t4g, m6i).
        - **Compute Optimized:** High compute performance (e.g., c7g, c6i).
        - **Memory Optimized:** Large memory for databases and in-memory analytics (e.g., r6i, x2idn).
        - **Storage Optimized:** High disk throughput and IOPS (e.g., i4i, d3).
        - **Accelerated Computing:** GPUs for ML, AI, and graphics (e.g., p4d, g5).
- **Instance Sizes:**
    
    - Each instance type has multiple sizes (e.g., t3.micro, t3.large) to meet specific resource requirements.
- **Instance Lifecycle:**
    
    - **Launch:** Boot up an EC2 instance.
    - **Run:** Instance is operational.
    - **Stop/Start:** Save costs and preserve the state of the instance.
    - **Terminate:** Shut down and delete the instance.

---

### **3. EC2 Pricing Models**

- **On-Demand Instances:**
    
    - Pay per second for compute capacity without long-term commitments.
    - Suitable for short-term workloads.
- **Reserved Instances:**
    
    - Commit to a 1- or 3-year term for discounts.
    - Ideal for predictable workloads.
- **Spot Instances:**
    
    - Use unused EC2 capacity at up to 90% lower cost.
    - Suitable for fault-tolerant, flexible applications.
- **Savings Plans:**
    
    - Flexible pricing model offering savings in exchange for usage commitment.
- **Dedicated Hosts:**
    
    - Physical servers dedicated to your use.
    - Required for regulatory or licensing compliance.
- **Free Tier:**
    
    - AWS Free Tier includes **750 hours/month** of t2.micro or t3.micro instances for the first 12 months.

---
An **Amazon Machine Image (AMI)** is a pre-configured template in Amazon Web Services (AWS) that provides the necessary information to launch an **EC2 (Elastic Compute Cloud)** instance. AMIs contain the operating system, application server, software, and configurations that an EC2 instance will run when launched. **[REGION SERVICE]**

Here’s a detailed look at **EC2 AMIs**:

### 1. **Components of an AMI**

- **Operating System (OS)**: The core of the AMI, which can be Linux (e.g., Ubuntu, Red Hat, CentOS) or Windows (e.g., Windows Server 2016, 2019).
- **Application Software**: AMIs can include pre-installed software, such as web servers (Apache, Nginx), databases (MySQL, PostgreSQL), development environments, or custom applications.
- **Configuration Settings**: These include system settings, user data, security groups, and other custom configurations needed to run the application.
- **Block Device Mappings**: This defines the volumes that are associated with the AMI and how the data is stored (e.g., Amazon EBS volumes).

### 2. **Types of AMIs**

- **AWS Provided AMIs**: These are standard, pre-configured AMIs provided by AWS, offering common operating systems like Amazon Linux, Ubuntu, Windows Server, etc.
- **Custom AMIs**: These are AMIs created by users or organizations. You can create a custom AMI by configuring an EC2 instance to your needs and then saving it as a custom AMI to launch future instances with the same configuration.
- **Marketplace AMIs**: These AMIs are available in the AWS Marketplace, where third-party vendors offer pre-configured, specialized AMIs that include proprietary software or services.

### 3. **Benefits of AMIs**

- **Consistency**: With AMIs, you can ensure that every EC2 instance launched from the same AMI will have identical configurations, making it easy to scale and maintain your infrastructure.
- **Scalability**: You can launch multiple EC2 instances from a single AMI, allowing you to quickly scale up your infrastructure based on demand.
- **Automation**: AMIs can be used in automated processes like continuous deployment or infrastructure as code (e.g., using AWS CloudFormation or Terraform).
- **Backup and Disaster Recovery**: By creating an AMI of a running instance, you effectively take a snapshot of its state, allowing for quick recovery or duplication if needed.

---

**UserData** is a feature in Amazon EC2 that allows you to run scripts or commands automatically when an EC2 instance is launched. It is typically used to configure instances at launch time, perform initial setup, or install software and applications. UserData is passed to the instance when it's created and can be used to automate various configuration tasks.

### Key Aspects of UserData:

1. **Usage**:
    
    - **Automating Instance Setup**: UserData is commonly used for initializing EC2 instances when they first start. For example, you can use UserData to install software, update packages, set up configuration files, or create specific system configurations.
    - **Bootstrapping**: It helps in bootstrapping your EC2 instances, meaning setting up the instance's environment before it is fully available for use.
2. **How It Works**:
    
    - When launching an EC2 instance, you can specify a script (usually in Bash or PowerShell) that is executed automatically during the first boot.
    - The script is executed with **root** privileges (Linux) or **administrator** privileges (Windows), so it can perform tasks like package installation or configuration updates.
3. **Supported Formats**:
    
    - **Linux/Unix-based Instances**: For Linux instances, UserData can be a **Bash script**.
    - **Windows Instances**: For Windows instances, UserData can be a **PowerShell script** or a batch file.
4. **Script Example**:
    
    - **Linux Example**:
        
        ```bash
        #!/bin/bash
        yum update -y
        yum install httpd -y
        service httpd start
        echo "Hello, EC2!" > /var/www/html/index.html
        ```
### Common Use Cases for UserData:

- **Installing and Configuring Software**: Automatically install applications like web servers (Apache, Nginx), databases, or monitoring tools when an instance is launched.
- **Setting Up Firewalls/Security Groups**: Configure firewall rules, security settings, and services.
- **CloudFormation Integration**: Use UserData within CloudFormation templates to handle instance-specific configurations.
- **Customizing EC2 Instances**: When creating multiple EC2 instances with identical configurations, UserData ensures that the setup is consistent across all instances.

---

