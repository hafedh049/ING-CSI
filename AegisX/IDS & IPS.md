### 🧠 **IDS & IPS Combo (Hybrid)**

|Name|Type|Vibe|
|---|---|---|
|**Suricata**|IDS / IPS / NSM|Multithreaded beast, detects threats in real-time, supports deep packet inspection, Lua scripting, and more. Modern AF.|
|**Snort**|IDS / IPS|The OG. Still relevant. Rules-based, reliable, used in enterprise and home labs alike. Maintained by Cisco.|
|**Zeek (formerly Bro)**|Network Security Monitor (NSM) / IDS|More of a network security framework than just an IDS. Think Sherlock Holmes with Python skills. Scripting heaven.|
|**Wazuh**|IDS / Host-based Security|It combines log analysis, file integrity monitoring, and real-time alerting. Integrates with Elastic Stack. Fresh and stylish.|
|**Security Onion**|IDS / IPS / Full SOC in a box|Built on top of Zeek, Suricata, Wazuh, and more. It’s the Avengers of open-source security. Ready-to-deploy distro.|
|**SELKS**|IDS / IPS Stack|Based on Suricata, Elastic, Logstash, Kibana, and Scirius. Plug and play setup for network threat hunting. Smooth UI.|

---

### 🔥 **Host-Based IDS (HIDS)**

|Name|Type|Vibe|
|---|---|---|
|**OSSEC**|HIDS|Logs, rootkits, file integrity, and more. Lightweight, yet powerful. Open-source edition is still poppin’.|
|**Tripwire Open Source**|HIDS|One of the first file integrity monitors. A bit old school, but still solid for certain use cases.|
|**AIDE (Advanced Intrusion Detection Environment)**|HIDS|Lightweight, CLI-focused. Good for minimalist setups. Think Linux sysadmin meets watchdog.|

---

### 👾 **Cloud / Container / Modern Stack**

|Name|Type|Vibe|
|---|---|---|
|**Falco** by Sysdig|Runtime Security (K8s & Containers)|Real-time threat detection for Kubernetes and containers. Syscall-based. Lightweight and nasty (in a good way).|
|**Kube Hunter**|K8s Cluster IDS|Probes Kubernetes clusters for security issues. DevSecOps-friendly.|
|**ThreatMapper** by Deepfence|Cloud-native IPS/IDS|Visual, cloud-native, detects vulnerabilities and threats in runtime. Free tier is strong.|
|**CrowdSec**|Collaborative IDS|Like Fail2ban but evolved. Uses community-powered threat intelligence. Runs on logs, highly flexible. Gamified vibe.|

---

### ⚙️ **Detection Engines & Rule Sets**

|Name|Use|Vibe|
|---|---|---|
|**Emerging Threats (ET) Rules**|Snort / Suricata Ruleset|Free rules maintained by Proofpoint. High-quality for open-source detection engines.|
|**Sigma**|Generic SIEM Rule Format|Write once, convert anywhere — translate detection logic across platforms. YAML-based. Hacker-chic.|

---

### 🧪 **Bonus / Experimental**

|Name|Type|Vibe|
|---|---|---|
|**OpenCanary**|Honeypot / Alerting|Deploy traps and bait. If someone touches it, boom, you get alerts.|
|**Sysmon + Elastic + Sigma**|HIDS / Threat Hunting|Sysmon logs with Elastic and Sigma rules create a powerful HIDS-style combo. High signal, low noise.|

---

### 🧠 TL;DR

- ✅ **Best all-around hybrid**: **Suricata** or **Security Onion**
    
- 🖥️ **Best for servers**: **Wazuh** or **OSSEC**
    
- ☁️ **Best for cloud-native/K8s**: **Falco** or **ThreatMapper**
    
- 🔥 **Next-gen flavor**: **CrowdSec** and **Sigma**