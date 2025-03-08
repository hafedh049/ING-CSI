# 🔍 **OSINT Tools Documentation**

This document covers five powerful **Open Source Intelligence (OSINT)** tools used for **user reconnaissance, domain research, and network intelligence**.

- **1️⃣ Holehe** – Checks if an email is linked to social media.
- **2️⃣ Sherlock** – Finds usernames across multiple social networks.
- **3️⃣ TheHarvester** – Gathers emails, domains, IPs, and more from public sources.
- **4️⃣ Shodan** – A search engine for exposed devices and services on the internet.
- **5️⃣ Sublist3r** – Subdomain enumeration tool.

---

## **1️⃣ Holehe – Email Enumeration Tool**

📌 **Description**: Holehe checks if an **email address** is **linked** to accounts on different online services without brute force.

### **Installation**

```bash
pip install holehe
```

### **Usage**

Run Holehe with an email:

```bash
holehe example@gmail.com
```

### **Options**

```bash
holehe -h
```

- `-p` → Uses proxychains.
- `-s <service>` → Check only a specific service.
- `-j` → Output results in JSON format.

---

## **2️⃣ Sherlock – Username Reconnaissance**

📌 **Description**: Sherlock finds accounts linked to a username across multiple social media platforms.

### **Installation**

```bash
git clone https://github.com/sherlock-project/sherlock.git
cd sherlock
pip install -r requirements.txt
```

### **Usage**

Run Sherlock to find a username:

```bash
python3 sherlock username
```

### **Advanced Usage**

- Save results to a file:
    
    ```bash
    python3 sherlock username --output result.txt
    ```
    
- Run with multiple usernames:
    
    ```bash
    python3 sherlock user1 user2 user3
    ```
    
- Search only on specific sites:
    
    ```bash
    python3 sherlock username --site Twitter Instagram
    ```
    

---

## **3️⃣ TheHarvester – Information Gathering Tool**

📌 **Description**: TheHarvester gathers **emails, subdomains, IPs**, and **data leaks** from public sources like Google, Bing, and Shodan.

### **Installation**

```bash
git clone https://github.com/laramies/theHarvester.git
cd theHarvester
pip install -r requirements.txt
```

### **Usage**

Find information about a target domain:

```bash
python3 theHarvester.py -d example.com -b google
```

### **Options**

```bash
python3 theHarvester.py -d example.com -b all
```

- `-d <domain>` → Target domain
- `-b <source>` → Data sources (`google`, `bing`, `shodan`, `crtsh`, `all`)
- `-l <number>` → Number of results to fetch

Example:

```bash
python3 theHarvester.py -d example.com -b bing -l 500
```

---

## **4️⃣ Shodan – The Search Engine for Hackers**

📌 **Description**: Shodan scans the internet for **open ports, services, and vulnerabilities**.

### **Installation**

```bash
pip install shodan
```

Get an **API key** from [https://www.shodan.io/](https://www.shodan.io/) and set it up:

```bash
shodan init YOUR_API_KEY
```

### **Basic Usage**

Search for all IPs running Apache servers:

```bash
shodan search apache
```

Find open **RDP (Remote Desktop Protocol)** ports:

```bash
shodan search port:3389
```

### **Advanced Queries**

- Find **open webcams**:
    
    ```bash
    shodan search "webcamxp"
    ```
    
- Find **open SSH servers**:
    
    ```bash
    shodan search port:22
    ```
    
- Find **vulnerable servers**:
    
    ```bash
    shodan search "Microsoft-IIS/7.5"  
    
    ```
    

---

## **5️⃣ Sublist3r – Subdomain Enumeration**

📌 **Description**: Sublist3r gathers **subdomains** of a target domain using search engines like **Google, Bing, Yahoo, and Virustotal**.

### **Installation**

```bash
git clone https://github.com/aboul3la/Sublist3r.git
cd Sublist3r
pip install -r requirements.txt
```

### **Usage**

Find subdomains of a target domain:

```bash
python3 sublist3r.py -d example.com
```

### **Advanced Usage**

- Save results to a file:
    
    ```bash
    python3 sublist3r.py -d example.com -o subdomains.txt
    ```
    
- Use brute-force enumeration:
    
    ```bash
    python3 sublist3r.py -d example.com -b
    ```
    

---

## **🔥 Summary Table**

|Tool|Purpose|Key Features|
|---|---|---|
|**Holehe**|Checks if an email is linked to accounts|No brute force, API-based|
|**Sherlock**|Finds usernames across platforms|Fast and customizable|
|**TheHarvester**|Gathers emails, subdomains, IPs|Uses search engines, Shodan|
|**Shodan**|Scans internet devices|Finds open ports, exploits|
|**Sublist3r**|Finds subdomains of a domain|Uses Google, Bing, VirusTotal|
