### 🔹 Sources of Logs & Events

- Syslogs (Linux, Windows)
    
- Active Directory logs
    
- DNS, DHCP, VoIP, VPN, Proxy logs
    
- Firewall + IDS/IPS (Snort, Suricata)
    
- Application logs (Web servers, DBs, etc.)
    
- EDR/XDR agents
    
- Honeytokens & deception logs
    
- Emulator activity (Android, VoIP attacks, etc.)
### 🔹 Collection Methods

- `FluentBit` or `Vector` DaemonSets on each node
    
- Windows Event Forwarding (Winlogbeat)
    
- Syslog/rsyslog to a Fluentd endpoint
    
- Custom log shippers (for honeypots / emulators)
    
- Python/Flask API endpoints (collect HTTP/JSON logs)