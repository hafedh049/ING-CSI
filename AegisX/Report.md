# AegisX: AI-Driven SOAR-Integrated Honeynet CyberDefense Framework

## Overview

**AegisX** is a next-generation cybersecurity research and simulation project designed to combine the power of AI, SOAR, and advanced DevSecOps tooling into a seamless, automated, and intelligent cyber defense grid. It is centered around a strategically designed honeynet meant to emulate real-world enterprise systems, continuously under threat by both internal and external attackers. These attacks are captured, analyzed, and mitigated through the custom-built, AI-infused **Gen-Z Sentinel** platform, supported by automated deployment and configuration using **Kubernetes**, **Ansible**, and **GitOps (ArgoCD)** methodologies.

---

## Environment

> [! TIP]
> **Vmware Workstation 17.6 Pro** instead of **Proxmox**

## 🌐 Project Scope and Architecture

The project encompasses several core zones:

1. **HoneyNet Zone** (192.168.2.0/24):
    
    - Simulates internal enterprise infrastructure.
        
    - Hosts vulnerable and bait services (VMs and Pods).
        
2. **CyberWarfare Zone** (192.168.1.0/24):
    
    - Hosts the Gen-Z Sentinel and associated orchestration/deployment services.
        
3. **Threat Intelligence Core API**:
    
    - Collects global threat feeds to tune detection models.
        
4. **Internet Gateway**:
    
    - Simulates external attacks.
        
5. **Security Perimeter**:
    
    - FortiGate Firewall and Suricata IDS/IPS for initial filtration and monitoring.
        

---

## 🛡️ Core Components

### 1. **Firewall: FortiGate**

**Chosen Over**: pfSense, OPNsense, Cisco ASA

**Why FortiGate?**

- Deep integration with threat intel and advanced DPI.
    
- Built-in SD-WAN, web filtering, antivirus, and anti-bot.
    
- Proven enterprise-grade performance.
    

**Function in AegisX**:

- Controls north-south and east-west traffic.
    
- Monitors external intrusion attempts.
    
- Logs and forwards alerts to Gen-Z Sentinel.
    

---

### 2. **IPS/IDS: Suricata**

**Chosen Over**: Snort3, Zeek, Wazuh (ELK)

**Why Suricata?**

- Multi-threading and superior throughput.
    
- Rich protocol analysis and file extraction.
    
- Smooth integration with Elastic + Eve JSON for automation.
    

**Role in AegisX**:

- Deep packet inspection for both internal/external traffic.
    
- Generates detailed alerts for anomaly detection.
    
- Integrated directly with Gen-Z Sentinel ingestion pipeline.
    

---

## 🧲 Honeynet Zone - VMs and Pods

Each asset represents a common attack surface in real-world infrastructure. These act as bait to gather telemetry and trigger response actions.

### 📦 VMs

#### 🪟 Active Directory Services (Windows Server):

- **Purpose**: Privilege escalation, lateral movement, credential theft.
    
- **Web App Ideas**:
    
    - SharePoint-like Intranet Portal
        
    - AI-powered Employee Directory with facial recognition login
        

#### 🧡 FreeIPA (Ubuntu, Red Hat):

- **Purpose**: Linux-based identity and access control.
    
- **Web App Ideas**:
    
    - Self-service AI ChatOps password recovery
        
    - AI anomaly-based login detection dashboard
        

### 📱 Pods (Microservices-based targets)

#### **Mobile App Hosting Pods**:

- **Proposed Apps**:
    
    - AI ChatBot for mental health (Flutter + Firebase + TensorFlow Lite)
        
    - QR Scanner Wallet (with fraud detection using ML)
        
    - Secure AI-based OTP Generator
        

#### **VoIP Pods (Asterisk)**:

- Exposes SIP/VoIP services, vulnerable to toll fraud and DoS.
    
- **Use Case**:
    
    - Asterisk server with AI-enhanced spam call detection
        
    - Real-time transcription of VoIP to text using Whisper ASR
        

---

## ⚙️ Gen-Z Sentinel - Custom AI-Driven SOAR

### 🔧 Technologies:

- **Backend**: Flask (RESTful API)
    
- **Frontend**: Next.js (live dashboards)
    
- **Database**: MongoDB (for structured telemetry)
    
- **ML/DL**: PyTorch / TensorFlow / scikit-learn
    
- **CI/CD**: ArgoCD (GitOps)
    
- **Automation**: Ansible + Kubernetes
	
- Instead of **Grafana + Prometheus** 

### 🧬 Sentinel Capabilities:

1. **Ingestion Pipeline**:
    
    - Collects logs from Suricata, FortiGate, and system logs.
        
    - Normalizes and indexes the data into MongoDB.
        
2. **Detection Engine**:
    
    - Applies ML models (anomaly detection, classification)
        
    - Uses DL for behavioral patterns (e.g. LSTM for session-based analysis)
        
3. **SOAR Playbooks**:
    
    - Auto-quarantine pods
        
    - Restart services
        
    - Update firewall rules
        
    - Notify admin via Discord/Webhook
        
4. **Visualization**:
    
    - Real-time graphs (threat heatmaps, incident timelines)
        
    - Drill-down on affected components
        

---

## 🧰 Kubernetes, Ansible & GitOps

### ☸️ Kubernetes

- Container orchestration to deploy apps (victim pods, tools, Sentinel components).
    
- Ensures scaling, high availability, and fault tolerance.
    
- Used to simulate real microservices in a secure segmented namespace.
    

### 🧾 Ansible

- Manages VM provisioning, software installation, and service configuration.
    
- Example: Set up AD on Windows VM, configure FreeIPA on Ubuntu, deploy mobile apps.
    
- Integrated with Git for version control.
    

### 📦 GitOps (ArgoCD)

- Syncs infrastructure state from Git.
    
- Every configuration, policy, and deployment is committed.
    
- Enables rollbacks, audits, and fast patching.
    
- Automatically applies updates to honeypot services and Gen-Z Sentinel logic.
    

---

## 📡 Threat Intel Core Integration

Feeds pulled from:

- **MISP**, **OpenCTI**, **Shodan**, **AlienVault**, **MITRE ATT&CK**, **VirusTotal**
    

These IOCs are:

- Pre-processed using NLP
    
- Mapped to MITRE techniques
    
- Enriched to enhance detection rules in Suricata + ML model training
    

---

## 🧠 Deep Learning Integration (Gen-Z Sentinel)

### Use Cases:

- **Intrusion Classification**: CNNs for packet flow classification
    
- **Anomaly Detection**: Autoencoders for rare behavior
    
- **Behavioral Sequencing**: LSTM models to detect kill chains
    
- **Threat Scoring**: Combining features from multiple tools to assign severity
    

### Training Data:

- Simulated attacks via CALDERA + PTA
    
- External traffic from Internet edge
    
- Enriched by threat intel feeds
    

---

## 🔚 Conclusion

**AegisX** is more than a honeynet—it's a living, evolving cyber range powered by AI and automation. With a strong foundation in real-world architecture, modern DevSecOps practices, and custom SOAR logic, it serves as a comprehensive playground for red/blue/purple teaming and machine learning research.

By integrating **Suricata, FortiGate, Kubernetes, Ansible, ArgoCD**, and an intelligent **Gen-Z Sentinel**, you have built an architecture that doesn’t just detect threats—it **understands them**, **reacts to them**, and **learns from them**.

_This is the future of cyber defense. This is **AegisX**._


---

# 🧠💥 How to Build **Your Own Gen-Z Sentinel**

---

## ⚙️ 1. **Define the Sentinel’s Mission**

Before coding a single line, define what your Sentinel should do:

- 🔎 Detect threats (via logs, packets, behaviors)
    
- 🤖 Analyze with ML/DL
    
- ⚔️ Respond (SOAR playbooks)
    
- 📊 Visualize data (custom dashboards)
    
- 🔁 Improve over time (feedback loop)
    

> **Goal**: From event ➡️ decision ➡️ action ➡️ insight — in real time.

---

## 🧱 2. **Design the Architecture (MVP Stack)**

|Layer|Tech/Tool|Role|
|---|---|---|
|Backend API|**Flask**|Receives data, routes to DB/ML|
|DB|**MongoDB**|Stores structured events, models, logs|
|ML Layer|**PyTorch / Scikit-learn**|Learns patterns, flags threats|
|Frontend|**Next.js / React**|UI for monitoring + playbook control|
|DevOps|**Kubernetes + Ansible**|Deploys pods, automates configs|
|GitOps|**ArgoCD**|Syncs code → cluster|
|Monitoring Add-on|(Optional) **Prometheus/Grafana**|Infra metrics, redundancy|

---

## 🔌 3. **Ingest Threat Data**

### 🧱 Data Sources:

- **Suricata alerts** (via EVE JSON)
    
- **Syslogs** from honeypots
    
- **Firewall logs** (FortiGate syslog or REST API)
    
- **Threat intel feeds** (MISP, Shodan, AlienVault)
    

### 🔧 Flask Setup:

```python
from flask import Flask, request, jsonify
app = Flask(__name__)

@app.route('/ingest', methods=['POST'])
def ingest():
    data = request.json
    # Save to MongoDB or pass to ML model
    return jsonify({"status": "received"}), 200
```

✅ Pipe Suricata EVE logs here with Filebeat or custom script.

---

## 🧠 4. **Build the Detection Engine (ML + DL)**

### 🔍 Models to Implement:

- **Anomaly detection** (IsolationForest, AutoEncoder)
    
- **Threat classification** (RandomForest, SVM)
    
- **Sequential attack behavior** (LSTM, GRU)
    
- **Threat scoring** (ensemble of model outputs)
    

### 🧪 Training Data:

- Simulated attacks (CALDERA, AttackIQ, PTA)
    
- Labeled threat intel logs
    
- Packet metadata (protocol, size, entropy, etc.)
    

### 🧠 Example: Simple ML Threat Score

```python
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier()
model.fit(X_train, y_train)

def predict_threat(event):
    return model.predict([extract_features(event)])
```

🔁 Store predictions in MongoDB, mark confidence.

---

## ⚔️ 5. **Create SOAR Playbooks**

### 🔥 Playbook Examples:

- `if threat == ransomware → isolate pod`
    
- `if repeated scan → update FortiGate blocklist`
    
- `if VoIP flood → restart Asterisk + send alert`
    
- `if unknown attack → trigger ML re-training`
    

### 🧩 Implement Playbooks in Flask:

```python
def handle_threat(threat_data):
    if threat_data['type'] == 'dos':
        block_ip(threat_data['source_ip'])
    elif threat_data['type'] == 'ransomware':
        isolate_node(threat_data['node'])
```

⚠️ Automate with Ansible (`ansible-runner` via Python).

---

## 🎨 6. **Build the Gen-Z Dashboard (Next.js)**

### 📊 Dashboard Modules:

- Live alerts feed (WebSocket or polling)
    
- Pod/VM health overview
    
- ML model decisions (threat type, score)
    
- SOAR activity logs
    
- IOC timeline / heatmap
    

🧠 Optional: Use Recharts, Chart.js, or D3.js for 🔥 data visualizations.

---

## 🔁 7. **Integrate ArgoCD + GitOps**

### 🌀 ArgoCD Roles:

- Watches your repo (Infra-as-Code)
    
- Auto-syncs updates to Kubernetes
    
- Reverts on failure, manages rollbacks
    

**Example Flow**:

- Update playbook or ML config in Git
    
- ArgoCD syncs → Sentinel auto-updates
    
- Gen-Z Sentinel never gets stale
    

---

## ☸️ 8. **Use Kubernetes to Run It All**

- Each component = its own pod (Flask API, Next.js UI, DB, ML worker)
    
- Use Helm charts for services like MongoDB
    
- Set namespace for `sentinel`, isolate from honeynet
    
- Expose Flask + UI via Ingress or NodePort
    

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sentinel-api
spec:
  replicas: 1
  template:
    spec:
      containers:
      - name: flask
        image: your-docker-image
        ports:
        - containerPort: 5000
```

---

## 📡 9. **Build the Feedback Loop**

- Every action the Sentinel takes → save the outcome
    
- Feed success/failure back into ML pipeline
    
- Regular model retraining schedule (weekly/monthly)
    
- Human feedback panel on UI (to approve/reject ML decisions)
    

---

## 📜 10. **Hardening and Optimization**

- 🔐 Auth: JWT-based for internal API
    
- 🚧 Rate limits, WAF around endpoints
    
- 🧠 Model versioning (MLflow or custom)
    
- 🧪 Unit + integration tests
    
- 🧩 Logs → ELK or Loki for audit trail
    

---

# 🎯 Endgame: What You'll Have

|Feature|Status|
|---|---|
|📦 Event ingestion|Live via Flask|
|🧠 AI detection|Custom models, modular pipeline|
|⚙️ SOAR response|Scripted + automated|
|📊 Dashboard UI|Fully custom, live feed|
|☸️ Infra deploy|Kubernetes, Ansible, GitOps|
|🔁 Self-healing/learning loop|Enabled|

---
# ⚔️ Comparative Table of Technologies for AegisX

|**Component**|**Chosen**|**Alternatives**|**Pros**|**Cons**|**Why You Chose This**|
|---|---|---|---|---|---|
|🔥 Firewall|**FortiGate**|pfSense, OPNsense, Cisco ASA|- Enterprise-grade DPI- Threat Intel- Easy FortiAnalyzer tie-in|- Paid license- Complex CLI at first|Powerful UTM stack + seamless alert/log forwarding|
|🔍 IDS/IPS|**Suricata**|Zeek, Snort 3, Wazuh|- Multi-threaded- EVE JSON output- Fast rule support|- Verbose logs- More config than Snort|Superior performance + tight integration with ML pipelines|
|🧠 ML Framework|**PyTorch**|TensorFlow, Scikit-learn|- Pythonic- Dynamic graph- Research-ready|- Slightly less ecosystem support than TF|More flexible for custom DL models + real-time LSTM-based threat chains|
|🧪 ML Classic|**Scikit-learn**|XGBoost, LightGBM|- Easy to train/test- Works with pandas- Tons of algorithms|- Not GPU-accelerated- Weak on huge data|Perfect for quick classification tasks + ensemble models|
|⚙️ Automation|**Ansible**|Puppet, Chef, SaltStack|- Agentless- YAML simple- SSH-native|- Slower than Salt for massive infra|Lightweight, readable, integrates well with Git + K8s|
|☸️ Orchestration|**Kubernetes**|Docker Swarm, Nomad|- Industry standard- Scale-ready- Ecosystem-rich|- Complex learning curve|Strongest container orchestration with rich pod control|
|🚀 GitOps/CD|**ArgoCD**|Flux, Jenkins X, Spinnaker|- Git-native sync- Visual dashboard- K8s-native|- Only for K8s- Some helm quirks|Instant sync + rollback for any playbook, model, or config update|
|🧩 DB Storage|**MongoDB**|PostgreSQL, Redis, InfluxDB|- JSON-friendly- Flexible schema- Perfect for logs + telemetry|- Not good for complex joins- Disk-heavy|Great for fast inserts + unstructured log data|
|🌐 Backend API|**Flask**|FastAPI, Django, Express.js|- Lightweight- Customizable- Easy ML/REST API setup|- Manual routing- No built-in auth|Full control for SOAR logic + super clean API endpoints|
|📊 Frontend UI|**Next.js**|React (CRA), Vue, Angular|- SSR-ready- Fast builds- Flexible with APIs|- Learning curve for full SSR config|Seamless data hydration + perfect for fast, reactive dashboards|
|📡 Threat Feeds|**MISP + OpenCTI**|AlienVault, Shodan, VirusTotal|- STIX support- Enrichment-ready- Community-driven|- Requires setup time|Supports CTI correlation + IOCs mapped to MITRE ATT&CK|
|📈 Metrics|_(Optional)_ Prometheus|InfluxDB, Elastic Metrics|- K8s-native scraping- Tons of exporters|- Needs Grafana for visualization|Best passive infra monitor (but not critical if Gen-Z does it all)|
|📊 Dashboard Alt|**Next.js UI**|Grafana, Kibana|- Full custom- ML-aware- UX freedom|- You build everything from scratch|Purpose-built for your SOAR + threat logic|
|🎯 Attack Sim|**CALDERA + PTA**|Red Team Ops, Metasploit|- Modular agents- TTP-based- Automatable|- Needs time to script scenarios|Realistic adversary emulation + internal + external threat blend|
|📡 Logs Ingestion|**Custom Flask/REST**|Filebeat, Logstash, Fluentd|- Total control- API-native|- Must build your own pipeline|Fine-tuned to your Sentinel architecture and storage format|

---

## 🧠 Summary Insights

- 🔥 Your choices = High performance + **customization first** mentality.
    
- 🧰 The alternatives aren’t wrong — just more “plug-and-play” or “infra-first” than “cyber ops-first”.
    
- 🤖 You’ve built a system where **every piece talks**, **thinks**, and **acts** with precision — and each chosen tech fits like armor on a knight.
    

![[Pasted image 20250611005250.png]]