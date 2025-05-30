## ✅ **Exercice 1 : Configuration Complète de FireWall par Zone (ZBF)**

---

### 🔹 **1. Création des Zones**

```bash
zone security INSIDE
zone security OUTSIDE
zone security DMZ
zone security DMZ_WEB
```

---

### 🔹 **2. Attribution des Interfaces aux Zones**

```bash
interface Ethernet0/2
 zone-member security INSIDE
 ip address 172.21.8.1 255.255.252.0

interface Ethernet0/3
 zone-member security OUTSIDE
 ip address 209.165.2.1 255.255.255.240

interface Ethernet0/0
 zone-member security DMZ
 ip address 172.21.1.1 255.255.255.128

interface Ethernet0/1
 zone-member security DMZ_WEB
 ip address 172.21.1.129 255.255.255.192
```

---

### 🔹 **3. Class-Maps pour le Filtrage**

```bash
class-map type inspect match-any CM-HTTP
 match protocol http

class-map type inspect match-any CM-DNS
 match protocol dns

class-map type inspect match-any CM-SMTP
 match protocol smtp

class-map type inspect match-any CM-ALL
 match protocol ip
```

---

### 🔹 **4. Policy-Maps**

#### 🔸 Vers DMZ-WEB (HTTP)

```bash
policy-map type inspect PM-TO-DMZWEB
 class type inspect CM-HTTP
  inspect
 class class-default
  drop
```

#### 🔸 Inside → DMZ

```bash
policy-map type inspect PM-INSIDE-TO-DMZ
 class type inspect CM-HTTP
  inspect
 class type inspect CM-DNS
  inspect
 class type inspect CM-SMTP
  inspect
 class class-default
  drop
```

#### 🔸 DMZ → Inside (SMTP retour)

```bash
policy-map type inspect PM-DMZ-TO-INSIDE
 class type inspect CM-SMTP
  inspect
 class class-default
  drop
```

#### 🔸 DMZ → Outside (HTTP, DNS sortants)

```bash
policy-map type inspect PM-DMZ-TO-OUTSIDE
 class type inspect CM-HTTP
  inspect
 class type inspect CM-DNS
  inspect
 class class-default
  drop
```

#### 🔸 Politique Drop par défaut

```bash
policy-map type inspect PM-DROP-ALL
 class class-default
  drop
```

---

### 🔹 **5. Zone-Pairs**

#### 🔸 Vers DMZ-WEB depuis toutes les zones

```bash
zone-pair security ZP-INSIDE-DMZWEB source INSIDE destination DMZ_WEB
 service-policy type inspect PM-TO-DMZWEB

zone-pair security ZP-OUTSIDE-DMZWEB source OUTSIDE destination DMZ_WEB
 service-policy type inspect PM-TO-DMZWEB

zone-pair security ZP-DMZ-DMZWEB source DMZ destination DMZ_WEB
 service-policy type inspect PM-TO-DMZWEB
```

#### 🔸 Inside → DMZ (HTTP, DNS, SMTP)

```bash
zone-pair security ZP-INSIDE-DMZ source INSIDE destination DMZ
 service-policy type inspect PM-INSIDE-TO-DMZ
```

#### 🔸 DMZ → Inside (SMTP)

```bash
zone-pair security ZP-DMZ-INSIDE source DMZ destination INSIDE
 service-policy type inspect PM-DMZ-TO-INSIDE
```

#### 🔸 DMZ → Outside

```bash
zone-pair security ZP-DMZ-OUTSIDE source DMZ destination OUTSIDE
 service-policy type inspect PM-DMZ-TO-OUTSIDE
```

#### 🔸 Politique de sécurité par défaut

```bash
zone-pair security ZP-INSIDE-OUTSIDE source INSIDE destination OUTSIDE
 service-policy type inspect PM-DROP-ALL

zone-pair security ZP-OUTSIDE-INSIDE source OUTSIDE destination INSIDE
 service-policy type inspect PM-DROP-ALL

zone-pair security ZP-OUTSIDE-DMZ source OUTSIDE destination DMZ
 service-policy type inspect PM-DROP-ALL

zone-pair security ZP-DMZWEB-INSIDE source DMZ_WEB destination INSIDE
 service-policy type inspect PM-DROP-ALL

zone-pair security ZP-DMZWEB-OUTSIDE source DMZ_WEB destination OUTSIDE
 service-policy type inspect PM-DROP-ALL
```

---

### ✅ Résumé des Règles Implémentées

|#|Source|Destination|Service|Protocole|Action|
|---|---|---|---|---|---|
|1|Any|DMZ-WEB|HTTP|TCP|Pass|
|2|INSIDE|DMZ-HTTP|HTTP|TCP|Pass|
|3|INSIDE|DMZ-DNS|DNS|UDP|Pass|
|4|INSIDE-MAIL|DMZ-SMTP|SMTP|TCP|Pass|
|5|DMZ-SMTP|INSIDE-MAIL|SMTP|TCP|Pass|
|6|DMZ-HTTP|Any|HTTP|TCP|Pass|
|7|DMZ-DNS|Any|DNS|UDP|Pass|
|8|Any|Any|Any|IP|**Drop**|

---

# Exercice 2
### 📌 **Informations de base :**

- **Réseau privé** : `172.16.64.0/20`
    
- **Administrateur** : `172.16.64.100`
    
- **Serveur Web** : `202.100.100.1`
    
- **Serveur DNS** : `202.95.93.77`
    

---

## 🔐 Objectif des règles :

1. Les **utilisateurs du réseau privé** peuvent accéder à Internet avec :
    
    - SMTP, HTTPS, DNS, FTP, DHCP
        
2. **Seul l’admin** (`172.16.64.100`) peut accéder :
    
    - aux serveurs Web (202.100.100.1) et DNS (202.95.93.77)
        
    - via **ping**, **Telnet**, **SSH**
        
3. **Seul l’admin** peut accéder au **routeur EDGE** en Telnet et SSH
    

---

### ✅ Configuration complète des ACL :

```bash
ip access-list extended ACL-SECURITE
 ! Règle 1 : Autoriser les utilisateurs vers Internet avec les services standards
 permit tcp 172.16.64.0 0.0.15.255 any eq smtp
 permit tcp 172.16.64.0 0.0.15.255 any eq https
 permit udp 172.16.64.0 0.0.15.255 any eq domain
 permit tcp 172.16.64.0 0.0.15.255 any eq ftp
 permit udp 172.16.64.0 0.0.15.255 any eq bootpc

 ! Règle 2 : Autoriser admin vers Web et DNS pour ping, telnet, ssh
 permit icmp host 172.16.64.100 host 202.100.100.1
 permit icmp host 172.16.64.100 host 202.95.93.77
 permit tcp host 172.16.64.100 host 202.100.100.1 eq telnet
 permit tcp host 172.16.64.100 host 202.100.100.1 eq ssh
 permit tcp host 172.16.64.100 host 202.95.93.77 eq telnet
 permit tcp host 172.16.64.100 host 202.95.93.77 eq ssh

 ! Règle 3 : Admin vers routeur EDGE en telnet et ssh
 permit tcp host 172.16.64.100 host 172.16.64.1 eq telnet
 permit tcp host 172.16.64.100 host 172.16.64.1 eq ssh

 ! Tout le reste est refusé par défaut
 deny ip any any
```

---

### 🔧 Application de l’ACL :

Si l’interface `f0/1` de EDGE est celle connectée vers le réseau privé :

```bash
interface f0/1
 ip access-group ACL-SECURITE in
```

---

### 📝 Remarques :

- Le masque `0.0.15.255` correspond bien à `/20`
    
- Les services requis sont bien couverts
    
- Les IP spécifiques sont bien protégées
    
- Telnet/SSH vers EDGE sont autorisés uniquement pour l’admin
    

---

