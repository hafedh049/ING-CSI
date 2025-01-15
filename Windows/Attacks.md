### **Test d'Intrusion de l'Active Directory et Attaques Détails**

Les tests d'intrusion sur l'Active Directory (AD) visent à exploiter des failles de sécurité pour obtenir des informations sensibles et, potentiellement, accéder à des systèmes ou obtenir des privilèges d'administrateur. Voici un aperçu des attaques courantes contre l'Active Directory, ainsi que des mesures de protection recommandées.

#### **1. Kerberoasting**

**Description**:

- L'attaque Kerberoasting consiste à obtenir des tickets Kerberos pour les comptes de service dans Active Directory, les capturer, puis les briser hors ligne pour récupérer les mots de passe des services.
- Cela repose sur des services qui utilisent des mots de passe faibles ou des configurations incorrectes.

**Mesures de protection**:

- **Utiliser des mots de passe forts et complexes pour les comptes de service**.
- **Activer la fonctionnalité "Key Distribution Center (KDC) Protected"** pour empêcher l'extraction des clés des tickets Kerberos.
- Utiliser un contrôle de domaine pour effectuer des audits sur les tickets Kerberos émis.

---

#### **2. Password Spraying**

**Description**:

- Le **password spraying** consiste à tester un nombre limité de mots de passe sur plusieurs comptes d'utilisateurs afin d'éviter les verrouillages de compte dus à trop de tentatives échouées avec un même mot de passe.

**Mesures de protection**:

- **Activer des politiques de verrouillage de compte** après un certain nombre de tentatives infructueuses.
- **Utiliser des mots de passe uniques et complexes** pour chaque utilisateur.
- **Mettre en place une surveillance** pour détecter des tentatives d'authentification inhabituelles.

---

#### **3. LLMNR et NBT-NS (Local Loop Multicast Name Resolution et NetBIOS)**

**Description**:

- **LLMNR** et **NBT-NS** sont utilisés pour résoudre les noms d'hôtes dans un réseau local. Les attaquants peuvent envoyer des requêtes malveillantes pour forcer des postes à envoyer des informations d'authentification via le réseau.

**Mesures de protection**:

- **Désactiver LLMNR et NBT-NS** via les stratégies de groupe ou la configuration du pare-feu.
- Pour **désactiver LLMNR** via PowerShell :
    
    ```powershell
    Set-NetIPInterface -InterfaceAlias "Ethernet" -DisableNeighborDiscovery $true
    ```
    
- Pour **désactiver NBT-NS** :
    
    ```powershell
    Set-NetAdapterBinding -Name "Ethernet" -ComponentID ms_netbios -Enabled $false
    ```
    

---

#### **4. Pass-the-Hash avec Mimikatz**

**Description**:

- L'attaque **Pass-the-Hash (PTH)** permet à un attaquant de se faire passer pour un autre utilisateur en utilisant le hash du mot de passe au lieu du mot de passe en clair, souvent via des outils comme **Mimikatz**.

**Mesures de protection**:

- **Désactiver NTLM** et **utiliser Kerberos** comme méthode d'authentification par défaut.
- **Activer la signature SMB et LDAP** pour renforcer la sécurité des communications réseau.
- **Utiliser des mots de passe forts** et activer l'authentification à **multi-facteurs** (MFA) pour les comptes privilégiés.

---

#### **5. Default Credentials (Identifiants par défaut)**

**Description**:

- Les **identifiants par défaut** laissés sur les systèmes peuvent permettre aux attaquants de prendre le contrôle des systèmes si ces identifiants ne sont pas modifiés.

**Mesures de protection**:

- **Modifier les identifiants par défaut** sur tous les équipements et systèmes avant leur mise en production.
- **Vérifier les configurations** des appareils réseau et des systèmes avant leur déploiement.

---

#### **6. Hard-coded Credentials (Identifiants codés en dur)**

**Description**:

- Les **identifiants codés en dur** dans les applications ou les scripts peuvent être extraits par les attaquants pour accéder aux systèmes.

**Mesures de protection**:

- **Ne jamais stocker d'identifiants codés en dur** dans le code ou les fichiers de configuration.
- **Utiliser des gestionnaires de secrets** pour stocker les identifiants de manière sécurisée.
- **Auditer régulièrement les systèmes** pour vérifier l'absence de telles pratiques.

---

#### **7. Privilege Escalation**

**Description**:

- L'**escalade de privilèges** permet à un attaquant de monter en grade, passant d'un utilisateur standard à un administrateur ou à un autre compte plus privilégié.

**Mesures de protection**:

- **Limiter les privilèges des utilisateurs** et appliquer le principe du moindre privilège.
- **Utiliser l'authentification à deux facteurs (MFA)** pour les comptes d'administration.
- **Auditer les privilèges d'utilisateur** de manière régulière.

---

#### **8. LDAP Reconnaissance**

**Description**:

- L'attaque **LDAP reconnaissance** consiste à interroger le serveur LDAP pour obtenir des informations sur les utilisateurs, groupes et autres objets d'Active Directory.

**Mesures de protection**:

- **Restreindre les requêtes LDAP** en fonction des groupes et des rôles.
- **Filtrer les informations** sensibles dans les résultats des requêtes LDAP.

---

#### **9. BloodHound Reconnaissance**

**Description**:

- **BloodHound** est un outil utilisé pour cartographier les relations de privilèges dans Active Directory et identifier les chemins d'escalade de privilèges.

**Mesures de protection**:

- **Utiliser des contrôles d'accès stricts** et veiller à ce que les membres d'un groupe d'administration ne soient pas en mesure de donner des privilèges élevés de manière excessive.
- **Auditer et surveiller les relations d'appartenance** aux groupes d'Active Directory.

---

#### **10. NTDS.dit Extraction**

**Description**:

- L'**extraction de NTDS.dit** permet à un attaquant d'extraire les informations d'identification (hashs) des utilisateurs à partir du fichier NTDS.dit, qui contient les informations d'authentification de tous les comptes dans AD.

**Mesures de protection**:

- **Restreindre l'accès physique** et réseau aux contrôleurs de domaine.
- **Activer la protection du fichier NTDS.dit** via les stratégies de sécurité et s'assurer qu'il est crypté.
- Utiliser des **atténuations de sécurité** comme l'authentification Kerberos et l'utilisation d'un mot de passe fort pour les comptes privilégiés.

---

### **Mesures de Protection Générales contre les Attaques sur Active Directory**

1. **Désactiver LLMNR et NBT-NS** :
    
    - **Via GPO** : Désactivez LLMNR et NBT-NS pour éviter les attaques de type man-in-the-middle.
    - **Via PowerShell** :
        
        ```powershell
        Set-NetIPInterface -InterfaceAlias "Ethernet" -DisableNeighborDiscovery $true
        ```
        
2. **Limiter la Communication entre les Postes de Travail sur un Même Réseau** :
    
    - Configurer des règles de pare-feu pour empêcher la communication directe entre les machines sur le même réseau local.
3. **Désactiver WPAD via la Stratégie de Groupe** :
    
    - WPAD (Web Proxy Auto-Discovery) peut être une porte d'entrée pour les attaques de type man-in-the-middle, donc il est préférable de le désactiver via GPO.
4. **Désactiver NTLM et Utiliser Kerberos** :
    
    - **Désactiver NTLM** dans les stratégies de sécurité et privilégier l'utilisation de Kerberos pour l'authentification.
5. **Activer la Signature SMB** :
    
    - La signature SMB permet de garantir l'intégrité des messages SMB échangés entre les machines, réduisant ainsi les attaques de type Man-in-the-Middle.
6. **Activer la Signature LDAP** :
    
    - Assurez-vous que la signature LDAP est activée pour protéger les échanges LDAP contre les attaques de type interception.
7. **Désactiver l'Administrateur Intégré** :
    
    - Désactiver le compte d'administrateur par défaut d'Active Directory ou le renommer pour compliquer son identification.
8. **Politiques de Mots de Passe Forts** :
    
    - Appliquer des politiques de mots de passe forts, incluant des règles sur la complexité, l'historique des mots de passe, et la durée d'expiration.
9. **Audit et Surveillance** :
    
    - Mettre en place un système d'audit et de surveillance régulière pour détecter toute activité suspecte sur les contrôleurs de domaine et les comptes à privilèges élevés.

