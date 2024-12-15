
# Projet Python

## Objectif du Projet

L'objectif de ce projet est de fournir une **plateforme unifiée** qui combine des outils de **diagnostic réseau avancés** avec une fonctionnalité de **chat en temps réel**. Cette plateforme permet aux administrateurs réseau de :

- **Surveiller et gérer** les configurations réseau.
- Communiquer directement avec les membres de leur équipe.
- Simplifier le **dépannage** et améliorer la **collaboration**.

---

## Description du Projet

### Outils Réseau, Sécurité et Chat en Temps Réel Basés sur Flask

Ce projet est une application web moderne, développée avec **Flask** et **Flask-SocketIO**, qui offre :

- Une suite complète d'**outils de gestion réseau et de sécurité**.
- Une **fonctionnalité de chat en temps réel**.

Il est conçu pour répondre aux besoins des **administrateurs réseau**, **ingénieurs**, **analystes de sécurité**, et même des **utilisateurs finaux**. 

Cette solution propose une approche **polyvalente** pour :
- **Analyser et sécuriser les réseaux**.
- **Faciliter la communication en temps réel**.

---

## Fonctionnalités Principales

### **1. Outils Réseau et Sécurité**

- **Validation d'Adresses IP** :
  - Vérification de la validité des adresses IPv4 et IPv6.
  - Garantit des saisies correctes pour les configurations réseau.

- **Génération de Plages IP** :
  - Génère des listes d'adresses IP à partir de blocs CIDR.
  - Simplifie la planification et l'analyse réseau.

- **Calcul des Sous-Réseaux** :
  - Affiche des détails tels que l'adresse réseau, l'adresse de diffusion et les hôtes disponibles.
  - Facilite la gestion optimale des ressources.

- **Recherche de Géolocalisation IP** :
  - Donne des informations comme le pays, la région, la ville et le FAI associés à une adresse IP.
  - Utile pour l'analyse de trafic et la détection d'anomalies.

- **Analyse de Ports** :
  - Scanne les ports ouverts d'une adresse IP.
  - Identifie des vulnérabilités ou points d'accès non sécurisés.

- **Outils Ping et Traceroute** :
  - Teste la connectivité d'un réseau avec "Ping".
  - Trace le chemin des paquets réseau jusqu'à une IP cible.

- **Recherches DNS et Whois** :
  - Permet des recherches pour associer domaines et adresses IP.
  - Fournit des informations sur l'enregistrement via Whois.

- **Sécurité et Gestion d'Adresses IP** :
  - Analyse ARP pour détecter des appareils actifs.
  - Bloque les adresses IP suspectes pour renforcer la sécurité.

### **2. Sécurité Avancée**

- **Gestion des Clés RSA** :
  - Génère des clés publiques/privées pour chiffrer les communications.
  - Garantit l'intégrité des données avec des fonctions de hachage.

- **Comparaison de Hashes** :
  - Vérifie l'intégrité des fichiers et chaînes de texte.
  - Détecte les modifications non autorisées.

---

### **3. Fonctionnalités de Chat (Basé sur Flask-SocketIO)**

- **Salles de Chat** :
  - Création de salles thématiques en temps réel.
  - Les utilisateurs peuvent rejoindre des conversations selon leurs besoins.

- **Envoi de Messages et Médias** :
  - Supporte les messages texte, les fichiers, les images et les vidéos.
  - Notifications instantanées pour les nouveaux messages.

- **Gestion des Messages** :
  - Les utilisateurs peuvent ajouter, modifier ou supprimer leurs messages.
  - Données stockées dans TinyDB pour une recherche rapide.

- **Réactions et Interactions** :
  - Réactions avec des émojis pour dynamiser les conversations.

---

## Technologies Utilisées

### **Backend**
- **Flask** pour la gestion des événements et API.
- **Flask-SocketIO** pour les communications en temps réel.
- **Scapy** pour analyser les réseaux.

### **Base de Données**
- **TinyDB** : Base légère pour le stockage rapide des logs et des données de chat.

### **Sécurité**
- **Cryptography** : Génération et gestion des clés RSA.

### **Frontend**
- Une interface intuitive pour gérer le chat et les diagnostics réseau.

---

## Public Cible

1. **Administrateurs Réseau** : Gérer les configurations réseau et collaborer en direct.
2. **Analystes Sécurité** : Analyser les ports, détecter les vulnérabilités et gérer les IP suspectes.
3. **Développeurs** : Effectuer des validations réseau pour les tests d'applications.
4. **Utilisateurs Finaux** : Participer à des discussions en temps réel via des salles de chat.

---
### **Backlog Produit**

| **UID** | **Description**                                                                                                                                       | **Priorité** | **Estimation (heures)** | **Définition de fait (DoD)**                                                                                     |
| ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ----------------------- | ---------------------------------------------------------------------------------------------------------------- |
| 1       | En tant qu'**utilisateur**, je veux pouvoir me connecter avec mon identifiant et mon mot de passe.                                                    | MUST         | 4                       | L'utilisateur peut se connecter avec un identifiant et un mot de passe valides.                                  |
| 2       | En tant qu'**utilisateur**, je veux pouvoir m'inscrire en fournissant des informations personnelles valides.                                          | MUST         | 4                       | L'utilisateur peut créer un compte en saisissant des informations valides.                                       |
| 3       | En tant qu'**utilisateur**, je veux pouvoir récupérer mon mot de passe si je l'ai oublié.                                                             | MUST         | 6                       | L'utilisateur peut demander un lien de réinitialisation et définir un nouveau mot de passe.                      |
| 4       | En tant qu'**utilisateur**, je veux accéder à une page d'accueil avec les sections principales : Services Réseau, Chatrooms, et Services de Sécurité. | MUST         | 10                      | La page d'accueil affiche les sections correctement et permet de naviguer entre elles.                           |
| 5       | En tant qu'**utilisateur**, je veux rechercher et filtrer les résultats sur la page d'accueil.                                                        | SHOULD       | 6                       | L'utilisateur peut rechercher et filtrer les résultats par catégorie.                                            |
| 6       | En tant qu'**utilisateur**, je veux utiliser un menu latéral pour accéder à la déconnexion, au profil, et au changement de thème.                     | SHOULD       | 5                       | Le menu latéral est accessible et fonctionne correctement.                                                       |
| 7       | En tant qu'**utilisateur**, je veux valider des adresses IP pour m'assurer qu'elles sont correctes.                                                   | MUST         | 4                       | L'utilisateur peut valider les adresses IP saisies.                                                              |
| 8       | En tant qu'**utilisateur**, je veux générer une plage d'adresses IP à partir d'une adresse IP de départ et d'un masque réseau.                        | SHOULD       | 5                       | L'utilisateur peut générer une plage d'adresses IP en fonction d'une adresse IP de départ et d'un masque réseau. |
| 9       | En tant qu'**utilisateur**, je veux connaître la localisation d'une adresse IP grâce à un outil de géolocalisation.                                   | SHOULD       | 6                       | L'utilisateur peut obtenir la localisation d'une adresse IP.                                                     |
| 10      | En tant qu'**utilisateur**, je veux scanner les ports ouverts d'une adresse IP pour des vérifications de sécurité.                                    | MUST         | 6                       | L'utilisateur peut scanner les ports ouverts sur une adresse IP.                                                 |
| 11      | En tant qu'**utilisateur**, je veux vérifier la connectivité réseau en envoyant des requêtes ping à une adresse IP.                                   | MUST         | 5                       | L'utilisateur peut envoyer des requêtes ping pour vérifier la connectivité réseau.                               |
| 12      | En tant qu'**utilisateur**, je veux tracer le chemin parcouru par les paquets réseau jusqu'à une adresse IP cible.                                    | SHOULD       | 5                       | L'utilisateur peut tracer le chemin parcouru par les paquets réseau jusqu'à une adresse IP cible.                |
| 13      | En tant qu'**utilisateur**, je veux obtenir les informations DNS associées à un domaine ou une adresse IP.                                            | COULD        | 5                       | L'utilisateur peut obtenir les informations DNS pour une adresse ou un nom de domaine.                           |
| 14      | En tant qu'**utilisateur**, je veux retrouver les noms de domaine associés à une adresse IP grâce à une recherche DNS inverse.                        | COULD        | 4                       | L'utilisateur peut obtenir les noms de domaine associés à une adresse IP.                                        |
| 15      | En tant qu'**utilisateur**, je veux scanner un réseau pour détecter les appareils connectés.                                                          | SHOULD       | 7                       | L'utilisateur peut scanner un réseau pour détecter les appareils connectés.                                      |
| 16      | En tant qu'**utilisateur**, je veux retrouver une adresse MAC à partir d'une adresse IP sur le réseau local.                                          | COULD        | 5                       | L'utilisateur peut retrouver une adresse MAC à partir d'une adresse IP sur le réseau local.                      |
| 17      | En tant qu'**utilisateur**, je veux discuter avec une autre personne dans une salle de chat en temps réel.                                            | MUST         | 6                       | Deux utilisateurs peuvent se connecter et échanger des messages en temps réel.                                   |
| 18      | En tant qu'**utilisateur**, je veux interagir avec un chatbot intelligent basé sur OpenAI.                                                            | SHOULD       | 8                       | L'utilisateur peut interagir avec un chatbot basé sur OpenAI en temps réel.                                      |
| 19      | En tant qu'**utilisateur**, je veux générer des mots de passe sécurisés pour protéger mes comptes.                                                    | MUST         | 4                       | L'utilisateur peut générer des mots de passe aléatoires et robustes.                                             |
| 20      | En tant qu'**utilisateur**, je veux générer des paires de clés RSA pour chiffrer mes données.                                                         | MUST         | 6                       | L'utilisateur peut générer des clés privées et publiques RSA.                                                    |
| 21      | En tant qu'**utilisateur**, je veux convertir des textes en clair en hash avec différents algorithmes.                                                | SHOULD       | 5                       | L'utilisateur peut hacher un texte clair en utilisant différents algorithmes (MD5, SHA-256, etc.).               |
| 22      | En tant qu'**utilisateur**, je veux identifier le type d'un hash pour savoir quel algorithme a été utilisé.                                           | COULD        | 6                       | L'utilisateur peut identifier le type de hash en fonction de ses caractéristiques.                               |
| 23      | En tant qu'**utilisateur**, je veux générer des listes personnalisées de mots selon mes besoins.                                                      | SHOULD       | 5                       | L'utilisateur peut créer des listes de mots en fonction de critères personnalisés.                               |
| 24      | En tant qu'**utilisateur**, je veux transférer des fichiers en toute sécurité avec un protocole FTP sécurisé.                                         | MUST         | 8                       | L'utilisateur peut envoyer et recevoir des fichiers en toute sécurité avec un chiffrement approprié.             |

---
### **Sprint 1 : Authentification et Navigation**

#### **User Stories**

1. **UID 1** - En tant qu'utilisateur, je veux pouvoir me connecter avec mon identifiant et mon mot de passe.
2. **UID 2** - En tant qu'utilisateur, je veux pouvoir m'inscrire en fournissant des informations personnelles valides.
3. **UID 3** - En tant qu'utilisateur, je veux pouvoir récupérer mon mot de passe si je l'ai oublié.
4. **UID 6** - En tant qu'utilisateur, je veux utiliser un menu latéral pour accéder à la déconnexion, au profil, et au changement de thème.

#### **Tâches Détails**

- **Connexion** (UID 1) :
    
    - Créer un endpoint Flask/SocketIO pour vérifier les identifiants.
    - Implémenter l’interface utilisateur dans Flutter avec des champs pour l’email et le mot de passe.
    - Ajouter une gestion des erreurs pour des identifiants incorrects.
    - Tester avec des scénarios d’identifiants valides et invalides.
- **Inscription** (UID 2) :
    
    - Créer un endpoint Flask/SocketIO pour enregistrer de nouveaux utilisateurs.
    - Ajouter des validations pour les champs comme l'email, le mot de passe et le nom d'utilisateur.
    - Implémenter l'interface Flutter avec un formulaire d'inscription.
    - Gérer les retours d'erreurs en cas de champs manquants ou d'email déjà enregistré.
- **Récupération de mot de passe** (UID 3) :
    
    - Créer un endpoint Flask/SocketIO pour envoyer un lien de réinitialisation de mot de passe par email.
    - Implémenter une page Flutter pour demander l'email de l'utilisateur.
    - Ajouter un écran Flutter pour définir un nouveau mot de passe.
    - Tester le processus complet avec des utilisateurs existants et non existants.
- **Menu latéral** (UID 6) :
    
    - Ajouter un Drawer dans Flutter avec des boutons pour "Déconnexion", "Modifier le profil" et "Changer le thème".
    - Implémenter la déconnexion via SocketIO.
    - Ajouter une navigation pour accéder à l'écran de modification du profil.
    - Implémenter un toggle pour le changement de thème (clair/sombre).

#### **Rétrospective**

- **Positifs** :
    - L'intégration entre Flask/SocketIO et Flutter a été fluide.
    - Les tests des endpoints ont couvert tous les cas d'erreurs courants.
- **Améliorations** :
    - Mieux estimer les tâches complexes comme la gestion des emails pour la récupération de mot de passe.
    - Documenter les API plus clairement pour faciliter les tests.

#### **Revue**

- La fonctionnalité d'authentification et de navigation est complète.
- Les écrans de connexion, d'inscription et de récupération de mot de passe fonctionnent comme prévu.
- Le menu latéral est bien intégré avec les actions principales.

---

### **Sprint 2 : Page d'accueil et Recherche**

#### **User Stories**

1. **UID 4** - En tant qu'utilisateur, je veux accéder à une page d'accueil avec les sections principales : Services Réseau, Chatrooms, et Services de Sécurité.
2. **UID 5** - En tant qu'utilisateur, je veux rechercher et filtrer les résultats sur la page d'accueil.

#### **Tâches Détails**

- **Page d'accueil** (UID 4) :
    
    - Créer une interface Flutter pour afficher les sections principales : Services Réseau, Chatrooms, et Services de Sécurité.
    - Ajouter la navigation vers chaque section.
    - Implémenter des icônes et un design cohérent pour chaque section.
- **Recherche et filtres** (UID 5) :
    
    - Créer un endpoint Flask/SocketIO pour recevoir les critères de recherche et renvoyer des résultats filtrés.
    - Ajouter une barre de recherche en haut de la page d'accueil dans Flutter.
    - Implémenter des filtres dynamiques pour trier les résultats selon les catégories.

#### **Rétrospective**

- **Positifs** :
    - L'interface de la page d'accueil est intuitive et facile à naviguer.
    - La recherche avec filtres fonctionne rapidement grâce à l'optimisation côté serveur.
- **Améliorations** :
    - Inclure plus d'exemples pour tester les fonctionnalités de recherche.
    - Optimiser les filtres pour gérer de grandes quantités de données.

#### **Revue**

- La page d'accueil est fonctionnelle et bien structurée.
- Les fonctionnalités de recherche et de filtrage sont entièrement intégrées et testées.

---

### **Sprint 3 : Services Réseau**

#### **User Stories**

1. **UID 7** - En tant qu'utilisateur, je veux valider des adresses IP pour m'assurer qu'elles sont correctes.
2. **UID 8** - En tant qu'utilisateur, je veux générer une plage d'adresses IP à partir d'une adresse IP de départ et d'un masque réseau.
3. **UID 9** - En tant qu'utilisateur, je veux connaître la localisation d'une adresse IP grâce à un outil de géolocalisation.

#### **Tâches Détails**

- **Validateur d'IP** (UID 7) :
    
    - Créer un endpoint Flask pour valider des adresses IP avec des bibliothèques comme `ipaddress`.
    - Ajouter un champ dans Flutter pour saisir une adresse IP.
    - Afficher les résultats (valide ou non) sur l'écran.
- **Générateur de plages d'IP** (UID 8) :
    
    - Implémenter un algorithme dans Flask pour générer des plages d'IP.
    - Ajouter une interface Flutter pour saisir l'adresse IP et le masque réseau.
    - Tester avec des plages IP typiques.
- **Géolocalisation IP** (UID 9) :
    
    - Intégrer une API tierce comme `ipstack` ou `ipapi` dans Flask pour la géolocalisation.
    - Créer une interface Flutter pour afficher les détails (pays, ville, etc.) de l'adresse IP.

#### **Rétrospective**

- **Positifs** :
    - Les services réseau fonctionnent de manière fluide et sont bien testés.
    - Bonne intégration avec des API externes pour la géolocalisation.
- **Améliorations** :
    - Ajouter plus de validations côté client pour éviter des requêtes inutiles.
    - Documenter les API tierces utilisées.

#### **Revue**

- Les outils de validation IP, de génération de plages IP, et de géolocalisation IP sont fonctionnels et bien intégrés.
- Les utilisateurs peuvent interagir avec ces outils efficacement.

---

### **Sprint 4 : Chatrooms**

#### **User Stories**

1. **UID 17** - En tant qu'utilisateur, je veux discuter avec une autre personne dans une salle de chat en temps réel.
2. **UID 18** - En tant qu'utilisateur, je veux interagir avec un chatbot intelligent basé sur OpenAI.

#### **Tâches Détails**

- **Chatroom pour 2 utilisateurs** (UID 17) :
    
    - Implémenter une communication en temps réel avec Flask-SocketIO.
    - Créer une interface Flutter pour afficher les messages échangés.
    - Ajouter une liste des utilisateurs connectés.
- **Chatbot OpenAI** (UID 18) :
    
    - Intégrer l'API OpenAI pour la génération de réponses dans Flask.
    - Créer un écran Flutter pour discuter avec le chatbot.
    - Gérer les erreurs de connexion avec OpenAI.

#### **Rétrospective**

- **Positifs** :
    - Les chatrooms sont robustes et permettent une communication fluide.
    - L'intégration du chatbot fonctionne bien pour des cas d'utilisation simples.
- **Améliorations** :
    - Tester avec plus d'utilisateurs simultanés.
    - Ajouter des animations pour améliorer l'expérience utilisateur.

#### **Revue**

- Les fonctionnalités de chat en temps réel et de chatbot sont opérationnelles.
- L'expérience utilisateur est fluide, avec des interfaces intuitives.

### **Sprint 5 : Services de Sécurité Partie 1**

#### **User Stories**

1. **UID 19** - En tant qu'utilisateur, je veux générer des mots de passe sécurisés pour protéger mes comptes.
2. **UID 20** - En tant qu'utilisateur, je veux générer des paires de clés RSA pour chiffrer mes données.
3. **UID 21** - En tant qu'utilisateur, je veux convertir des textes en clair en hash avec différents algorithmes.

#### **Tâches Détails**

- **Génération de mots de passe sécurisés** (UID 19) :
    
    - Implémenter un endpoint Flask pour générer des mots de passe aléatoires et robustes (longueur, caractères spéciaux, etc.).
    - Créer un écran Flutter pour que l'utilisateur puisse définir des critères (longueur, types de caractères).
    - Tester les mots de passe générés pour garantir leur sécurité.
- **Génération de paires de clés RSA** (UID 20) :
    
    - Utiliser `cryptography` dans Flask pour générer des clés RSA (privée et publique).
    - Permettre à l'utilisateur de télécharger les clés via Flutter.
    - Tester avec des tailles de clé standards (2048, 4096 bits).
- **Hachage de textes** (UID 21) :
    
    - Implémenter un endpoint Flask pour hacher des textes en clair avec des algorithmes comme MD5, SHA-256, SHA-512.
    - Ajouter un écran Flutter pour saisir le texte à hacher et afficher le résultat.
    - Gérer les erreurs pour des entrées vides ou invalides.

#### **Rétrospective**

- **Positifs** :
    - Les outils de génération de mots de passe et de clés RSA sont robustes et conformes aux normes de sécurité.
    - Le hachage fonctionne avec plusieurs algorithmes.
- **Améliorations** :
    - Ajouter des options de personnalisation pour le hachage (choix de l'algorithme).
    - Documenter les fonctionnalités pour une meilleure utilisation.

#### **Revue**

- Les fonctionnalités de sécurité de base (mots de passe, clés RSA, et hachage) sont terminées.
- Les interfaces utilisateur sont intuitives et fonctionnelles.

---

### **Sprint 6 : Services de Sécurité Partie 2**

#### **User Stories**

1. **UID 22** - En tant qu'utilisateur, je veux identifier le type d'un hash pour savoir quel algorithme a été utilisé.
2. **UID 23** - En tant qu'utilisateur, je veux générer des listes personnalisées de mots selon mes besoins.

#### **Tâches Détails**

- **Identification de hash** (UID 22) :
    
    - Intégrer `hashid` dans Flask pour analyser les hashes et identifier l'algorithme utilisé.
    - Créer un écran Flutter pour saisir un hash et afficher les algorithmes possibles.
    - Tester avec différents types de hashes (MD5, SHA-256, bcrypt, etc.).
- **Génération de listes de mots** (UID 23) :
    
    - Implémenter un générateur de wordlists dans Flask avec des critères (longueur, caractères, répétitions).
    - Créer une interface Flutter pour définir les paramètres de génération.
    - Permettre à l'utilisateur de télécharger la liste générée.

#### **Rétrospective**

- **Positifs** :
    - Le module d'identification de hash est précis et rapide.
    - Les listes de mots personnalisées sont utiles pour divers scénarios.
- **Améliorations** :
    - Optimiser les performances pour les grandes wordlists.
    - Ajouter des options d'exportation en plusieurs formats (txt, csv).

#### **Revue**

- Les outils avancés de sécurité sont prêts et testés.
- L'expérience utilisateur est fluide, avec des options claires pour chaque fonctionnalité.

---

### **Sprint 7 : FTP Sécurisé**

#### **User Stories**

1. **UID 24** - En tant qu'utilisateur, je veux transférer des fichiers en toute sécurité avec un protocole FTP sécurisé.

#### **Tâches Détails**

- **FTP Sécurisé** :
    - Implémenter un serveur Flask-SocketIO pour envoyer et recevoir des fichiers avec chiffrement (ex : AES).
    - Créer des options pour le client Flutter pour :
        - Télécharger un fichier depuis le serveur.
        - Envoyer un fichier au serveur.
    - Ajouter des vérifications d'intégrité (checksums) pour chaque fichier transféré.
    - Tester avec différents types et tailles de fichiers.

#### **Rétrospective**

- **Positifs** :
    - Les transferts de fichiers sont sécurisés et fiables.
    - Le chiffrement assure la confidentialité des données.
- **Améliorations** :
    - Ajouter des logs côté serveur pour suivre les transferts.
    - Améliorer les notifications côté client pour informer l'utilisateur du statut du transfert.

#### **Revue**

- La fonctionnalité FTP sécurisé est terminée.
- Les transferts sont rapides, sécurisés, et bien testés.



![[Copie de SPRINT I_ UC.png]]![[seq.png]]

![[class 1.png]]