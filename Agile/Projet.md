## **Product Backlog : Clone WhatsApp avec Fonctionnalités Avancées**

| **USID** | **Description** | **Estimation (heures)** | **Priorité** | **Définition of Done (DoD)** |
|----------|----------------|-------------------------|--------------|-----------------------------|
| US01     | Inscription et connexion avec le numéro de téléphone | 30h | Must | Les utilisateurs peuvent s’inscrire et se connecter avec un code OTP reçu par SMS. |
| US02     | Gestion de profil utilisateur avec photo et statut | 20h | Should | L’utilisateur peut modifier son nom, son statut, et sa photo de profil. |
| US03     | Envoi et réception de messages texte, audio et vidéo | 40h | Must | L’utilisateur peut envoyer et recevoir du texte, des audios et des vidéos sans erreur. |
| US04     | Conversion audio-texte automatique des messages vocaux | 60h | Could | Les messages vocaux sont transcrits avec une précision d’au moins 80 %. |
| US05     | Messagerie éphémère avec expiration configurable | 50h | Must | Les messages disparaissent automatiquement après le délai défini par l’utilisateur. |
| US06     | Suggestions IA de réponses contextuelles | 70h | Could | Affiche au moins 3 suggestions pertinentes par message reçu. |
| US07     | Historique complet des discussions et statistiques d’utilisation | 40h | Should | L’utilisateur peut consulter l’historique de ses conversations et des statistiques de chat. |
| US08     | Sondages interactifs pour les discussions de groupe | 30h | Should | Les utilisateurs peuvent créer et participer à des sondages dans les groupes. |
| US09     | Cartes d’activité interactives pour engagement dans les groupes | 35h | Could | Les utilisateurs peuvent partager des cartes d’activité dans les groupes. |
| US10     | Mode sombre et clair personnalisable | 15h | Should | L’utilisateur peut changer entre le mode sombre et le mode clair via les paramètres. |

---

## **Répartition en Sprints**

### **Sprint 1 : Authentification, Profil et Messagerie de Base**

- **User Stories :**  
  - **US01** : Inscription et connexion avec le numéro de téléphone  
  - **US02** : Gestion de profil utilisateur avec photo et statut  
  - **US03** : Envoi et réception de messages texte, audio et vidéo

#### **Tâches Techniques :**
- **US01** :  
  - Intégration de l’authentification avec Firebase (2 jours)  
  - Gestion OTP avec Twilio API (3 jours)  
  - UI/UX pour inscription et connexion (1 jour)  

- **US02** :  
  - Création de l’interface de profil utilisateur (1 jour)  
  - Gestion des mises à jour de photo et de statut via Firestore (2 jours)  

- **US03** :  
  - Implémentation de la messagerie instantanée avec WebSocket (3 jours)  
  - Gestion de l’envoi de fichiers audio et vidéo (2 jours)  

**Sprint Review :**  
Les utilisateurs peuvent s’inscrire, se connecter, configurer leur profil et utiliser la messagerie de base pour envoyer du texte, des audios et des vidéos.

**Rétrospective :**  
- **Points Positifs :** Bonne coordination entre l’équipe front et back.  
- **Amélioration :** Mieux gérer le temps lors de l’intégration Firebase pour éviter des retards.

---

### **Sprint 2 : Messagerie Éphémère et Suggestions IA**

- **User Stories :**  
  - **US05** : Messagerie éphémère avec expiration configurable  
  - **US06** : Suggestions IA de réponses contextuelles

#### **Tâches Techniques :**
- **US05** :  
  - Implémentation des timers d’expiration dans Firestore (2 jours)  
  - UI pour configurer les délais d’expiration (1 jour)  
  - Tests et correction de bugs (1 jour)  

- **US06** :  
  - Intégration de l’API OpenAI pour suggestions contextuelles (3 jours)  
  - Création d’un composant UI pour afficher les suggestions (2 jours)  
  - Tests d’intégration et optimisation (2 jours)  

**Sprint Review :**  
Les utilisateurs peuvent envoyer des messages éphémères et recevoir des suggestions automatiques d’IA pour répondre.

**Rétrospective :**  
- **Points Positifs :** L’implémentation des suggestions d’IA a été fluide.  
- **Amélioration :** Mieux anticiper les besoins de test pour la messagerie éphémère.

---

### **Sprint 3 : Historique des Discussions, Sondages et Cartes d’Activité**

- **User Stories :**  
  - **US07** : Historique complet des discussions et statistiques  
  - **US08** : Sondages interactifs pour les discussions de groupe  
  - **US09** : Cartes d’activité interactives  

#### **Tâches Techniques :**
- **US07** :  
  - Stockage de l’historique des discussions dans Firestore (2 jours)  
  - Création d’un tableau de bord statistique simple (2 jours)  

- **US08** :  
  - Création du modèle de sondage dans Firestore (2 jours)  
  - UI/UX pour les sondages de groupe (2 jours)  

- **US09** :  
  - Implémentation des cartes d’activité avec actions partagées (3 jours)  
  - Tests d’interface utilisateur (1 jour)  

**Sprint Review :**  
Les utilisateurs peuvent consulter l’historique des discussions, participer à des sondages, et partager des cartes d’activité dans les groupes.

**Rétrospective :**  
- **Points Positifs :** Bonne gestion du backlog et respect des délais.  
- **Amélioration :** Améliorer la gestion des feedbacks utilisateurs après les tests.

---

## **Résumé des Sprints et Avancement**

| **Sprint** | **User Stories Couvertes** | **Statut** |
|------------|----------------------------|------------|
| Sprint 1   | US01, US02, US03           | Terminé    |
| Sprint 2   | US05, US06                 | Terminé    |
| Sprint 3   | US07, US08, US09           | En Cours   |
