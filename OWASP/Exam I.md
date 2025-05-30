# Excercice I

❌
✅
✅
❌
✅
✅

---
# Exercice II
### **Questions:**

1. Écrire une requête Google dorks pour retourner les fichiers de type .pdf localisés sur le domaine **[www.telekabel.de](http://www.telekabel.de/)**
    
2. Écrire une requête Google dorks pour retourner tous les sous-domaines du domaine **[www.telekabel.de](http://www.telekabel.de/)**
    
3. Écrire une requête Google dorks pour trouver les pages contenant le mot **VPN** dans l'adresse URL

---

### **Solutions:**

1. **Requête pour les fichiers PDF sur un domaine :**
    
    ```
    site:www.telekabel.de filetype:pdf
    ```
    
2. **Requête pour découvrir les sous-domaines :**
    
    ```
    site:*.telekabel.de -www
    ```
    
    > `-www` is used to exclude the main www subdomain so you can see the other subdomains more easily.
    
3. **Requête pour chercher "VPN" dans l'URL :**
    
    ```
    inurl:VPN
    ```
    
    > You can combine with a site if you want to restrict the search, like: `inurl:VPN site:example.com`
    

---
# Exercice III

### **URL de référence :**

`http://www.tek-up.com/page/index.html`

|URL|Résultat|Explication|
|---|---|---|
|`http://www.tek-up.com/docs/codes.html`|✅ Même origine|Même protocole (`http`), même domaine (`www.tek-up.com`), même port (80)|
|`http://www.tek-up.com/review/re.html`|✅ Même origine|Même protocole, domaine, et port|
|`http://blog.tek-up.com/page/re.html`|❌ Origine différente|Sous-domaine différent (`blog.tek-up.com` ≠ `www.tek-up.com`)|
|`http://tek-up.com/page/re.html`|❌ Origine différente|Domaine différent (`tek-up.com` ≠ `www.tek-up.com`)|
|`http://www.tek-up.com:8080/page/re.html`|❌ Origine différente|Port différent (`8080` ≠ `80`)|
|`https://www.tek-up.com/page/index.html`|❌ Origine différente|Protocole différent (`https` ≠ `http`)|

----
# Exercice IV

### **a) XSS : Cross-Site Scripting**

**Définition :**  
XSS permet à un attaquant d’injecter des scripts malveillants dans une page web vue par d’autres utilisateurs.

**Mécanismes de protection :**

- Échapper les caractères spéciaux (HTML/JavaScript) dans les entrées utilisateur.
    
- Utiliser des frameworks sécurisés (React, Angular) qui gèrent automatiquement l’échappement.
    
- Activer une **Content Security Policy (CSP)**.
    

---

### **b) Injection SQL**

**Définition :**  
Injection SQL permet à un attaquant de manipuler une requête SQL en insérant du code malveillant via des champs d'entrée.

**Mécanismes de protection :**

- Utiliser des **requêtes préparées** (prepared statements).
    
- Valider et filtrer les entrées utilisateurs.
    
- Utiliser des ORM (Object-Relational Mapping) qui protègent contre les injections.
    

---

### **c) Insecure Design**

**Définition :**  
Une application avec un design non sécurisé manque de principes de sécurité dès la phase de conception.

**Mécanismes de protection :**

- Appliquer une démarche **"Security by Design"** dès le début du développement.
    
- Réaliser des **modélisations de menace** (threat modeling).
    
- Utiliser des frameworks de sécurité standardisés (OWASP ASVS, etc.).
    

---

### **d) CSRF : Cross-Site Request Forgery**

**Définition :**  
CSRF force un utilisateur authentifié à exécuter une action non désirée sur une application web à son insu.

**Mécanismes de protection :**

- Utiliser des **jetons CSRF** (anti-CSRF tokens).
    
- Vérifier l'en-tête **Referer/Origin**.
    
- Ne pas utiliser de requêtes GET pour les actions sensibles.
    

---

### **e) Vulnerable and Outdated Components**

**Définition :**  
Utilisation de bibliothèques, plugins ou frameworks obsolètes ou non sécurisés.

**Mécanismes de protection :**

- Maintenir les dépendances à jour.
    
- Utiliser des outils de **scanning de vulnérabilités** (Snyk, OWASP Dependency-Check).
    
- Supprimer les composants inutilisés.
    

---

### **f) Échecs de journalisation et de surveillance de la sécurité (Logging and Monitoring Failures)**

**Définition :**  
Absence de journalisation ou de surveillance adéquate empêche la détection d'attaques ou d'incidents.

**Mécanismes de protection :**

- Implémenter une **journalisation centralisée** (SIEM, ELK).
    
- Surveiller les logs avec des alertes en temps réel.
    
- Assurer la **conservation sécurisée des logs**.
    

---

### **g) SSRF : Server-Side Request Forgery**

**Définition :**  
SSRF permet à un attaquant de forcer le serveur à envoyer des requêtes vers des ressources internes.

**Mécanismes de protection :**

- Valider les URLs fournies par les utilisateurs.
    
- Interdire les requêtes vers des adresses internes (127.0.0.1, localhost, etc.).
    
- Mettre en place des **firewalls applicatifs**.