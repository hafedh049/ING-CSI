Les **maîtres d'opérations** (ou **FSMO** pour **Flexible Single Master Operations**) dans Active Directory désignent un ensemble de rôles spécialisés attribués à des contrôleurs de domaine (DC) afin de gérer certaines fonctions critiques pour le bon fonctionnement de l'annuaire. Les maîtres d'opérations sont nécessaires pour éviter les conflits dans la gestion d'objets dans le domaine et la forêt. Ces rôles sont répartis sur un ou plusieurs contrôleurs de domaine, et chaque rôle a une fonction spécifique.

### **Les cinq rôles FSMO principaux** :

1. **Maître d'opérations du schéma (Schema Master)** :
    
    - **Fonction** : Ce rôle est responsable de la gestion des modifications apportées au schéma d'Active Directory. Le schéma définit la structure de l'annuaire, y compris les types d'objets et les attributs qu'ils peuvent contenir.
    - **Emplacement** : Il n'y a qu'un seul **Schema Master** dans toute la forêt, ce qui signifie qu'il est unique dans la forêt AD.
    - **Utilisation** : Lorsque des modifications du schéma (ajout d'attributs, de classes, etc.) doivent être effectuées, elles doivent passer par ce contrôleur de domaine.
2. **Maître d'opérations de domaine (Domain Naming Master)** :
    
    - **Fonction** : Il est responsable de l'ajout et de la suppression des domaines dans une forêt Active Directory. Ce rôle gère également les changements de nom de domaine dans la forêt.
    - **Emplacement** : Il y a un **Domain Naming Master** par forêt.
    - **Utilisation** : Lorsqu'un domaine est ajouté ou supprimé dans la forêt, ce rôle intervient pour s'assurer que ces actions sont effectuées de manière cohérente.
3. **PDC Emulator (Primary Domain Controller Emulator)** :
    
    - **Fonction** : Ce rôle émule un **contrôleur de domaine principal (PDC)** pour les anciens systèmes Windows (avant Windows 2000) et assure des fonctionnalités importantes pour la compatibilité descendante. Il est également responsable de la gestion des mots de passe, des verrouillages de compte, de la synchronisation de l'heure, et de la gestion des modifications de mots de passe dans un environnement multi-domaines.
    - **Emplacement** : Il y a un **PDC Emulator** par domaine dans la forêt.
    - **Utilisation** : Ce rôle est essentiel pour maintenir la compatibilité avec les anciennes versions de Windows, en plus de gérer les mots de passe et les verrous de comptes.
4. **Maître d'opérations RID (Relative Identifier Master)** :
    
    - **Fonction** : Ce rôle est responsable de l'attribution des **identificateurs relatifs (RID)** à tous les contrôleurs de domaine dans un domaine. Un RID est une partie d'un SID (Security Identifier) utilisé pour identifier de manière unique un objet dans Active Directory.
    - **Emplacement** : Il y a un **RID Master** par domaine dans la forêt.
    - **Utilisation** : Il garantit que les contrôleurs de domaine attribuent des RID uniques à chaque objet afin d'éviter les collisions.
5. **Infrastructure Master** :
    
    - **Fonction** : Ce rôle est responsable de la gestion des références entre les objets d'un domaine et ceux d'autres domaines dans la forêt. Lorsqu'un objet dans un domaine fait référence à un objet dans un autre domaine, le rôle Infrastructure Master assure la mise à jour correcte de cette référence.
    - **Emplacement** : Il y a un **Infrastructure Master** par domaine.
    - **Utilisation** : Ce rôle est crucial pour garantir l'intégrité des références entre domaines, en particulier dans les environnements multi-domaines.

### **Répartition des rôles FSMO** :

- Les rôles FSMO peuvent être assignés à différents contrôleurs de domaine, mais certains rôles sont spécifiques à l'ensemble de la forêt, tandis que d'autres sont spécifiques à un domaine particulier.
- Par défaut, tous les rôles FSMO sont attribués au premier contrôleur de domaine créé lors de l'installation d'Active Directory. Cependant, ils peuvent être transférés à d'autres contrôleurs de domaine si nécessaire.

### **Gestion des rôles FSMO** :

- Les administrateurs peuvent transférer les rôles FSMO entre les contrôleurs de domaine à l'aide de **NTDSUtil**, **PowerShell**, ou l'outil **Active Directory Users and Computers (ADUC)**.
- Si un contrôleur de domaine qui détient un rôle FSMO échoue, ce rôle peut être transféré à un autre contrôleur de domaine en ligne pour assurer la continuité des services.

### **Conclusion** :

Les maîtres d'opérations jouent un rôle crucial dans la gestion et la maintenance de l'Active Directory, en garantissant la cohérence des données, la compatibilité des systèmes, et l'intégrité des objets et de leurs références. Leur bonne gestion est essentielle pour le bon fonctionnement d'une infrastructure Active Directory.