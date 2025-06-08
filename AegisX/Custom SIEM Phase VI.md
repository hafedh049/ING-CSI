### 🔹 Triggered via Kafka/Redis events

- Flask service polls alert queue
    
- Lookup playbook for alert type
    
- Run Python automation chain    
### 🔹 Playbook Examples

- IP → firewall block (UFW/iptables/firewalld)
    
- User → disabled in FreeIPA or AD
    
- File → quarantined
    
- Alert → forwarded to Threat Intel service
    
- Alert → archived in Mongo with tags
### 🔹 Notifies SOC

- Slack bots / Discord webhooks
    
- HTML alert email with log trail
    
- Create case in ticket system (optional)