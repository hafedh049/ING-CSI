HAFEDH GUENICHI & IBITHEL SALHI
### Objectif

Créer une application web interactive qui combine des outils de gestion réseau, un système de messagerie avancé, des fonctionnalités de sécurité robustes, et des options de personnalisation utilisateur. L'application vise à fournir une plateforme complète pour les utilisateurs techniques et professionnels.

---
### Description

L'application est conçue pour répondre à divers besoins :

1. **Authentification** : Permet aux utilisateurs de se connecter, de créer des comptes, et de récupérer leurs mots de passe.
2. **Section réseau** : Fournit des outils avancés pour les administrateurs réseau, notamment la validation d’adresses IP, la génération de plages IP, et des outils comme le scanner de ports, la géolocalisation IP, et le traceroute.
3. **Salle de chat** : Offre une plateforme de messagerie riche avec des fonctionnalités telles que la gestion des conversations, le partage de fichiers, et des interactions en temps réel. Elle inclut aussi l’intégration d’un chatbot basé sur l’API Gemini.
4. **Section sécurité** : Propose des outils pour générer des mots de passe sécurisés, créer des paires de clés RSA, hacher des données, et analyser le trafic réseau.
5. **Personnalisation utilisateur** : Permet aux utilisateurs de modifier leurs profils, de personnaliser les couleurs de l'interface, et de gérer leurs préférences.

---

### Technologies utilisées

1. **Frontend** :
    - **Flutter** : Pour développer une interface utilisateur moderne et réactive, compatible avec plusieurs plateformes.
2. **Backend** :
    - **Flask** : Pour créer les API REST et gérer la logique métier de l'application.
    - **WebSocket avec Flask-SocketIO** : Pour fournir une communication en temps réel dans la salle de chat.
3. **Base de données et stockage** :
    - **Firebase Firestore** : Pour le stockage des données utilisateur, des conversations, et des configurations réseau.
    - **Firebase Storage** : Pour gérer et stocker les fichiers multimédias, tels que les images et vidéos échangées dans la salle de chat.
4. **API externes** :
    - **ipinfo.io** : Pour la géolocalisation des adresses IP.
    - **Gemini Chatbot API** : Pour intégrer l'intelligence artificielle dans les discussions.
5. **Authentification et sécurité** :
    - **Firebase Authentication** : Pour gérer l’inscription, la connexion, et la récupération des mots de passe.

---

### Product Backlog

|**UID**|**Description**|**Priorité (MoSCoW)**|**Priorité (heures)**|**DoD**|
|---|---|---|---|---|
|**Auth-01**|En tant qu'utilisateur, je peux me connecter avec mon adresse e-mail et mot de passe.|Must|8|L'utilisateur peut se connecter avec succès et être redirigé vers le tableau de bord.|
|**Auth-02**|En tant qu'utilisateur, je peux créer un compte avec mon adresse e-mail et un mot de passe.|Must|10|Le compte est créé avec validation des données (e-mail valide, mot de passe sécurisé).|
|**Auth-03**|En tant qu'utilisateur, je peux réinitialiser mon mot de passe en utilisant un e-mail de récupération.|Must|6|L'utilisateur reçoit un e-mail avec un lien pour réinitialiser son mot de passe.|
|**Net-01**|En tant qu'utilisateur, je peux valider une adresse IP (IPv4 ou IPv6).|Must|4|Une validation correcte est effectuée, et les erreurs sont affichées pour les adresses invalides.|
|**Net-02**|En tant qu'utilisateur, je peux générer une plage d'adresses IP valide.|Should|5|Une plage d'adresses IP correcte est générée selon les paramètres fournis.|
|**Net-03**|En tant qu'utilisateur, je peux obtenir la géolocalisation d'une adresse IP en utilisant l'API ipinfo.io.|Should|6|L'utilisateur voit les données de géolocalisation correctes dans l'interface.|
|**Net-04**|En tant qu'utilisateur, je peux scanner des ports en utilisant des sockets et le multithreading.|Must|12|Le scan détecte les ports ouverts/fermés avec une performance optimisée.|
|**Net-05**|En tant qu'utilisateur, je peux effectuer un ping sur une adresse IP et voir les résultats.|Must|4|Les résultats du ping (temps, perte de paquets, etc.) sont affichés à l'utilisateur.|
|**Net-06**|En tant qu'utilisateur, je peux effectuer une recherche traceroute sur une adresse IP.|Should|6|Le chemin complet des sauts vers l'adresse cible est affiché avec précision.|
|**Net-07**|En tant qu'utilisateur, je peux effectuer une recherche DNS pour un domaine donné.|Should|5|Les enregistrements DNS du domaine sont affichés avec succès.|
|**Net-08**|En tant qu'utilisateur, je peux effectuer une recherche DNS inversée sur une adresse IP.|Could|5|Le domaine associé à l'adresse IP est affiché, ou une erreur est renvoyée si aucun n'existe.|
|**Net-09**|En tant qu'utilisateur, je peux scanner un réseau en utilisant des sockets et le multithreading.|Must|14|Tous les appareils connectés au réseau sont listés avec leurs adresses IP et MAC.|
|**Net-10**|En tant qu'utilisateur, je peux trouver une adresse MAC pour une adresse IP spécifique.|Could|6|L'adresse MAC correspondante est affichée si elle est disponible.|
|**Chat-01**|En tant qu'utilisateur, je peux participer à une salle de chat générale pour communiquer avec d'autres utilisateurs.|Must|10|L'utilisateur peut envoyer et recevoir des messages texte dans la salle de chat générale.|
|**Chat-02**|En tant qu'utilisateur, je peux voir une liste de mes conversations classées par date des derniers messages en ordre décroissant.|Should|8|Toutes les conversations sont affichées avec leur dernier message et horodatage.|
|**Chat-03**|En tant qu'utilisateur, je peux rechercher des utilisateurs et des conversations via une barre de recherche.|Must|10|La recherche renvoie des résultats corrects basés sur l'entrée de l'utilisateur.|
|**Chat-04**|En tant qu'utilisateur, je peux archiver, sceller, supprimer ou épingler une conversation.|Must|12|Les actions sont correctement effectuées et la liste des conversations est mise à jour.|
|**Chat-05**|En tant qu'utilisateur, je peux voir le statut (en ligne/hors ligne) des autres utilisateurs.|Must|6|Le statut des utilisateurs est affiché en temps réel dans l'interface.|
|**Chat-06**|En tant qu'utilisateur, je peux modifier l'image d'arrière-plan de mes discussions et la supprimer si nécessaire.|Should|6|L'image d'arrière-plan peut être changée ou supprimée avec succès.|
|**Chat-07**|En tant qu'utilisateur, je peux changer les surnoms dans mes discussions et appliquer un filtre de recherche sur les messages.|Could|8|Les surnoms peuvent être changés, et la recherche sur les messages fonctionne correctement.|
|**Chat-08**|En tant qu'utilisateur, je peux envoyer des images, vidéos, fichiers téléchargeables et messages texte.|Must|12|Tous les types de messages peuvent être envoyés et reçus avec succès.|
|**Chat-09**|En tant qu'utilisateur, je peux copier, supprimer, réagir, répondre et éditer mes messages.|Must|10|Les actions de gestion des messages sont correctement effectuées dans la discussion.|
|**Chat-10**|En tant qu'utilisateur, je peux interagir avec l'API Gemini en utilisant ma clé API pour échanger des messages texte.|Could|6|Les messages texte envoyés à l'API Gemini renvoient des réponses attendues.|
|**Sec-01**|En tant qu'utilisateur, je peux générer un mot de passe aléatoire et sécurisé.|Must|4|Le mot de passe généré respecte les critères de sécurité (longueur, complexité).|
|**Sec-02**|En tant qu'utilisateur, je peux générer une paire de clés RSA.|Must|6|Les clés RSA générées sont valides et utilisables.|
|**Sec-03**|En tant qu'utilisateur, je peux hacher un texte brut en utilisant différents algorithmes de hachage disponibles.|Should|5|Le texte haché correspond à l'algorithme sélectionné.|
|**Sec-04**|En tant qu'utilisateur, je peux utiliser un outil d'identification de hachage.|Could|4|L'algorithme de hachage est identifié avec précision.|
|**Sec-05**|En tant qu'utilisateur, je peux générer des listes de mots basées sur des critères spécifiques, comme la répétition.|Should|8|La liste générée respecte les critères fournis.|
|**Sec-06**|En tant qu'utilisateur, je peux utiliser le service SFTP pour télécharger et téléverser des fichiers.|Must|10|Les fichiers sont téléchargés/téléversés avec succès via SFTP.|
|**Sec-07**|En tant qu'utilisateur, je peux analyser le trafic réseau et appliquer des filtres (TCP, UDP, etc.).|Must|12|Le trafic réseau est capturé et filtré correctement.|
|**Prof-01**|En tant qu'utilisateur, je peux changer mes informations personnelles, comme mon nom et mon avatar.|Must|6|Les modifications sont enregistrées et reflétées dans le profil.|
|**Prof-02**|En tant qu'utilisateur, je peux changer la couleur principale du site web.|Could|4|La couleur principale du site change et est sauvegardée dans les paramètres utilisateur.|
|**Prof-03**|En tant qu'utilisateur, je peux me déconnecter de mon compte.|Must|3|La session utilisateur est terminée, et l'utilisateur est redirigé vers la page de connexion.|


---

### **Sprint Planification : Division Complète du Product Backlog**

Voici la division complète du backlog en **5 sprints équilibrés**, chaque sprint contenant des user stories avec leurs heures associées.

---

### **Sprint 1 : Authentification et Gestion du Profil**

**Heures totales** : 28 heures

|**User Story ID**|**Description**|**Heures**|
|---|---|---|
|US1|En tant qu'utilisateur, je peux me connecter avec un email et un mot de passe.|6|
|US2|En tant qu'utilisateur, je peux créer un compte.|6|
|US3|En tant qu'utilisateur, je peux réinitialiser mon mot de passe.|4|
|US4|En tant qu'utilisateur, je peux modifier mes informations de profil.|6|
|US5|En tant qu'utilisateur, je peux personnaliser la couleur principale du site.|6|

#### **Sprint Review**

- **Accomplissements** : Fonctionnalités d’authentification et de gestion de profil terminées avec succès.
- **Feedback** : Ajouter plus de validations pour les données utilisateur.

#### **Sprint Rétrospective**

- **Points positifs** : Bonne gestion des tâches et respect des délais.
- **Axes d’amélioration** : Dédier du temps supplémentaire aux tests d’expérience utilisateur.

![[Copie de SPRINT I_ UC.png]]

---

### **Sprint 2 : Outils Réseau (1ère partie)**

**Heures totales** : 28 heures

|**User Story ID**|**Description**|**Heures**|
|---|---|---|
|US6|En tant qu'utilisateur, je peux valider une adresse IP (v4, v6).|6|
|US7|En tant qu'utilisateur, je peux générer une plage d'adresses IP.|6|
|US8|En tant qu'utilisateur, je peux obtenir la géolocalisation d’une adresse IP.|8|
|US9|En tant qu'utilisateur, je peux utiliser un outil de ping.|8|

#### **Sprint Review**

- **Accomplissements** : Premiers outils réseau fonctionnels et testés.
- **Feedback** : Ajouter des messages d’erreur plus détaillés pour des cas limites.

#### **Sprint Rétrospective**

- **Points positifs** : Bonne intégration entre Flask et Flutter.
- **Axes d’amélioration** : Simplifier les interfaces utilisateur pour les outils techniques.

![[Copie de SPRINT I_ UC (1).png]]

---

### **Sprint 3 : Outils Réseau (2ème partie)**

**Heures totales** : 28 heures

|**User Story ID**|**Description**|**Heures**|
|---|---|---|
|US10|En tant qu'utilisateur, je peux effectuer un traceroute.|6|
|US11|En tant qu'utilisateur, je peux utiliser un scanner de ports.|8|
|US12|En tant qu'utilisateur, je peux effectuer une recherche DNS.|8|
|US13|En tant qu'utilisateur, je peux effectuer une recherche DNS inversée.|6|

#### **Sprint Review**

- **Accomplissements** : Complétion des outils réseau avancés.
- **Feedback** : Ajouter des tests automatisés pour valider les résultats.

#### **Sprint Rétrospective**

- **Points positifs** : Documentation détaillée des outils.
- **Axes d’amélioration** : Optimiser le temps d’exécution pour les outils lourds.

![[Copie de SPRINT I_ UC (2).png]]

---

### **Sprint 4 : Salle de Chat**

**Heures totales** : 28 heures

|**User Story ID**|**Description**|**Heures**|
|---|---|---|
|US14|En tant qu'utilisateur, je peux voir mes conversations et les utilisateurs.|6|
|US15|En tant qu'utilisateur, je peux effectuer des recherches dans mes conversations.|6|
|US16|En tant qu'utilisateur, je peux archiver, sceller, ou supprimer des conversations.|6|
|US17|En tant qu'utilisateur, je peux envoyer des messages, fichiers, images, et vidéos.|6|
|US18|En tant qu'utilisateur, je peux copier, supprimer ou réagir aux messages.|4|

#### **Sprint Review**

- **Accomplissements** : Fonctionnalités principales de la salle de chat complètes.
- **Feedback** : Améliorer la fluidité de l’expérience utilisateur dans les grandes discussions.

#### **Sprint Rétrospective**

- **Points positifs** : Intégration réussie de Flutter et Flask-SocketIO.
- **Axes d’amélioration** : Ajout de tests de charge pour le chat.

![[Copie de SPRINT I_ UC (3).png]]

---

### **Sprint 5 : Sécurité et Chatbot**

**Heures totales** : 28 heures

|**User Story ID**|**Description**|**Heures**|
|---|---|---|
|US19|En tant qu'utilisateur, je peux générer des mots de passe sécurisés.|4|
|US20|En tant qu'utilisateur, je peux générer des paires de clés RSA.|6|
|US21|En tant qu'utilisateur, je peux hacher des données avec des algorithmes divers.|6|
|US22|En tant qu'utilisateur, je peux interagir avec le chatbot Gemini.|8|
|US23|En tant qu'utilisateur, je peux analyser le trafic réseau.|4|

#### **Sprint Review**

- **Accomplissements** : Sécurité et chatbot Gemini intégrés avec succès.
- **Feedback** : Vérifier la compatibilité des outils sur différentes plateformes.

#### **Sprint Rétrospective**

- **Points positifs** : Bon équilibre entre fonctionnalités réseau et sécurité.
- **Axes d’amélioration** : Documenter toutes les options disponibles pour l'utilisateur.

![[Copie de SPRINT I_ UC (4).png]]

---

### Diagramme du classe

![[Diagramme vierge 1.png]]

### LINK
https://recon-cyber.web.app/
### PICS
![[Screenshot (102).png]]

![[Screenshot (103).png]]

![[Screenshot (104).png]]

![[Screenshot (105).png]]

![[Screenshot (106).png]]

![[Screenshot (107).png]]

![[Screenshot (108).png]]

![[Screenshot (109).png]]

![[Screenshot (110).png]]

![[Screenshot (111).png]]

![[Screenshot (112).png]]

![[Screenshot (113).png]]

![[Screenshot (114).png]]

![[Screenshot (115).png]]

![[Screenshot (116).png]]

![[Screenshot (117).png]]

![[Screenshot (118).png]]

![[Screenshot (119).png]]

![[Screenshot (120).png]]

![[Screenshot (121).png]]

![[Screenshot (122).png]]

![[Screenshot (123).png]]

![[Screenshot (124).png]]

![[Screenshot (125).png]]

![[Screenshot (127).png]]

![[Screenshot (128).png]]

![[Screenshot (1).png]]

![[Screenshot (6).png]]

![[Screenshot (7).png]]

![[Screenshot (8).png]]

![[Screenshot (90).png]]

![[Screenshot (91).png]]

![[Screenshot (92).png]]

![[Screenshot (93).png]]

![[Screenshot (94).png]]

![[Screenshot (95).png]]

![[Screenshot (96).png]]

![[Screenshot (97).png]]

![[Screenshot (99).png]]

![[Screenshot (100).png]]

![[Screenshot (101).png]]
