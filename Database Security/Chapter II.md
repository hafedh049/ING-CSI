### **📌 Liste Complète des Vues et Tables Essentielles dans le Dictionnaire de Données Oracle**

Le **Dictionnaire de Données** d’Oracle contient des **vues statiques**, **vues dynamiques** et **tables internes** qui permettent aux DBA et aux développeurs d’interroger et de gérer la base de données.

---

## **🔹 1. Tables Internes du Dictionnaire de Données** (Non accessibles directement)

Ces tables stockent les métadonnées de la base et sont situées dans le **tablespace SYSTEM**.

|**Nom de la Table**|**Description**|
|---|---|
|`TAB$`|Stocke la liste des tables|
|`OBJ$`|Contient tous les objets de la base (tables, indexes, vues, etc.)|
|`USER$`|Contient les utilisateurs de la base|
|`COL$`|Stocke les colonnes de toutes les tables|
|`SEG$`|Contient les informations des segments (espace de stockage)|
|`TS$`|Contient les tablespaces|
|`IND$`|Contient les indexes|
|`CON$`|Contient les contraintes (PK, FK, etc.)|
|`LOBS$`|Stocke les métadonnées des LOB (Large Objects)|

🔴 **Attention** : Ces tables sont internes et ne doivent jamais être modifiées directement. Oracle les expose à travers des vues.

---

## **🔹 2. Vues Statiques (`DBA_*`, `ALL_*`, `USER_*`)**

Ces vues permettent de consulter les **métadonnées stockées dans les fichiers de données (`.dbf`)**.

### **📍 Tables & Indexes**

|**Vue**|**Description**|
|---|---|
|`DBA_TABLES`|Liste toutes les tables de la base|
|`ALL_TABLES`|Tables accessibles par l’utilisateur|
|`USER_TABLES`|Tables appartenant à l’utilisateur|
|`DBA_TAB_COLUMNS`|Liste des colonnes des tables|
|`DBA_INDEXES`|Liste des indexes|
|`DBA_IND_COLUMNS`|Colonnes des indexes|

### **📍 Espaces de Stockage**

|**Vue**|**Description**|
|---|---|
|`DBA_TABLESPACES`|Liste des tablespaces|
|`DBA_DATA_FILES`|Liste des fichiers de données (`.dbf`)|
|`DBA_TEMP_FILES`|Liste des fichiers temporaires|
|`DBA_SEGMENTS`|Informations sur les segments de stockage|
|`DBA_EXTENTS`|Informations sur les extents utilisés|

### **📍 Utilisateurs & Privilèges**

|**Vue**|**Description**|
|---|---|
|`DBA_USERS`|Liste des utilisateurs de la base|
|`DBA_ROLES`|Liste des rôles|
|`DBA_ROLE_PRIVS`|Privilèges des rôles|
|`DBA_SYS_PRIVS`|Privilèges système des utilisateurs|
|`DBA_TAB_PRIVS`|Privilèges sur les tables|

### **📍 Contraintes & Clés Étrangères**

|**Vue**|**Description**|
|---|---|
|`DBA_CONSTRAINTS`|Liste des contraintes (PK, FK, CHECK, UNIQUE)|
|`DBA_CONS_COLUMNS`|Colonnes associées aux contraintes|

---

## **🔹 3. Vues Dynamiques (`V$` et `GV$`)**

Les vues dynamiques récupèrent des informations **en temps réel** depuis la mémoire (SGA, PGA) ou certains fichiers critiques (`control file`, `redo logs`).

### **📍 Informations Générales**

|**Vue**|**Description**|
|---|---|
|`V$DATABASE`|Informations générales sur la base|
|`V$INSTANCE`|Détails sur l’instance Oracle en cours|
|`V$VERSION`|Version d’Oracle utilisée|

### **📍 Sessions & Processus**

|**Vue**|**Description**|
|---|---|
|`V$SESSION`|Liste des sessions actives|
|`V$PROCESS`|Liste des processus actifs|
|`V$SESSION_WAIT`|Liste des sessions en attente d’événements|
|`V$SYSTEM_EVENT`|Liste des événements système en cours|

### **📍 Mémoire (SGA & PGA)**

|**Vue**|**Description**|
|---|---|
|`V$SGA`|Taille et composants du SGA|
|`V$SGASTAT`|Statistiques détaillées du SGA|
|`V$PGA`|Statistiques sur la mémoire PGA|
|`V$PGASTAT`|Utilisation de la mémoire PGA|

### **📍 Transactions & Logs**

|**Vue**|**Description**|
|---|---|
|`V$LOG`|Informations sur les redo logs|
|`V$LOGFILE`|Liste des fichiers redo logs|
|`V$ARCHIVED_LOG`|Liste des archives redo logs|
|`V$TRANSACTION`|Transactions en cours|

---

## **🎯 Résumé des Types de Vues**

|**Type**|**Préfixe**|**Données Consultées**|
|---|---|---|
|**Vues Statiques**|`DBA_*`, `ALL_*`, `USER_*`|Métadonnées stockées dans les fichiers `.dbf`|
|**Vues Dynamiques**|`V$` et `GV$`|Informations en temps réel (mémoire SGA/PGA, fichiers de contrôle, redo logs)|
|**Tables Internes**|`TAB$`, `OBJ$`, `USER$`, `COL$`...|Stockage brut des métadonnées (non accessibles directement)|

---

## **🚀 Pourquoi Connaître ces Vues ?**

🔹 **Pour gérer l’espace de stockage** (`DBA_DATA_FILES`, `DBA_TABLESPACES`)  
🔹 **Pour surveiller les sessions et processus actifs** (`V$SESSION`, `V$PROCESS`)  
🔹 **Pour auditer les privilèges et utilisateurs** (`DBA_USERS`, `DBA_TAB_PRIVS`)  
🔹 **Pour optimiser les performances mémoire** (`V$SGA`, `V$PGA`)  
🔹 **Pour suivre les transactions et logs** (`V$TRANSACTION`, `V$LOG`)

