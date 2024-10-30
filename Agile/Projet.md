## **Product Backlog : Clone WhatsApp avec Fonctionnalités Avancées**

| **USID** | **User Story** | **Estimation (heures)** | **Priorité** | **Définition of Done (DoD)** |
|----------|----------------|-------------------------|--------------|-----------------------------|
| **US01** | En tant qu’utilisateur, je veux pouvoir m’inscrire et me connecter avec mon numéro de téléphone afin d’accéder à l’application. | 30h | Must | Connexion réussie via code OTP SMS, sans erreurs. |
| **US02** | En tant qu’utilisateur, je veux gérer mon profil avec photo et statut pour personnaliser mon compte. | 20h | Should | L’utilisateur peut modifier le nom, le statut et la photo via l’interface. |
| **US03** | En tant qu’utilisateur, je veux envoyer et recevoir des messages texte, audio et vidéo pour communiquer avec mes contacts. | 40h | Must | Envoi et réception de texte, audio et vidéo fonctionnels. |
| **US04** | En tant qu’utilisateur, je veux que mes messages vocaux soient automatiquement transcrits en texte afin de les lire rapidement. | 60h | Could | Les messages vocaux sont transcrits avec au moins 80 % de précision. |
| **US05** | En tant qu’utilisateur, je veux envoyer des messages éphémères pour que ceux-ci disparaissent après un certain délai. | 50h | Must | Les messages sont automatiquement supprimés après le délai choisi. |
| **US06** | En tant qu’utilisateur, je veux recevoir des suggestions automatiques d’IA pour répondre rapidement aux messages reçus. | 70h | Could | L’IA propose au moins 3 suggestions pertinentes par message. |
| **US07** | En tant qu’utilisateur, je veux consulter l’historique de mes discussions et voir des statistiques d’utilisation pour suivre mon activité. | 40h | Should | Accès à l’historique et affichage des statistiques de chat. |
| **US08** | En tant qu’utilisateur de groupe, je veux créer et participer à des sondages pour engager les participants. | 30h | Should | Création et participation aux sondages sans erreurs. |
| **US09** | En tant que membre de groupe, je veux partager des cartes d’activité afin de collaborer avec les autres participants. | 35h | Could | Les cartes d’activité sont accessibles à tous les membres du groupe. |
| **US10** | En tant qu’utilisateur, je veux pouvoir activer un mode sombre ou clair pour personnaliser mon expérience d’utilisation. | 15h | Should | L’utilisateur peut basculer entre les modes sombre et clair. |

---

## **Répartition en Sprints**

### **Sprint 1 : Authentification, Profil et Messagerie de Base**

- **User Stories :**  
  - **US01** : Inscription et connexion avec le numéro de téléphone  
  - **US02** : Gestion de profil utilisateur avec photo et statut  
  - **US03** : Envoi et réception de messages texte, audio et vidéo

#### **Tâches Techniques :**
- **US01** :  
  - Intégrer Firebase Authentication (2 jours)  
  - Configurer OTP avec Twilio API (3 jours)  
  - Interface d’inscription et connexion (1 jour)  

- **US02** :  
  - Écran de gestion du profil (1 jour)  
  - Mise à jour du profil dans Firestore (2 jours)  

- **US03** :  
  - Implémenter WebSocket pour messagerie instantanée (3 jours)  
  - Ajouter l’envoi de fichiers audio et vidéo (2 jours)  

**Sprint Review :**  
Les utilisateurs peuvent s’inscrire, se connecter et utiliser la messagerie instantanée avec leur profil personnalisé.

**Rétrospective :**  
- **Points Positifs :** Bonne coordination entre les équipes front et back.  
- **Amélioration :** Optimiser la configuration d’OTP pour réduire les erreurs.

---

### **Sprint 2 : Messagerie Éphémère et Suggestions IA**

- **User Stories :**  
  - **US05** : Messagerie éphémère avec expiration configurable  
  - **US06** : Suggestions IA pour les réponses contextuelles

#### **Tâches Techniques :**
- **US05** :  
  - Ajouter la gestion des délais dans Firestore (2 jours)  
  - UI pour configurer l’expiration des messages (1 jour)  
  - Tests d’expiration (1 jour)  

- **US06** :  
  - Intégrer l’API OpenAI (3 jours)  
  - Composant UI pour suggestions IA (2 jours)  
  - Tests et ajustements (2 jours)  

**Sprint Review :**  
Les utilisateurs peuvent envoyer des messages éphémères et profiter de suggestions d’IA.

**Rétrospective :**  
- **Points Positifs :** Intégration rapide des API IA.  
- **Amélioration :** Anticiper les besoins en tests plus complexes.

---

### **Sprint 3 : Historique, Sondages et Cartes d’Activité**

- **User Stories :**  
  - **US07** : Historique des discussions et statistiques  
  - **US08** : Sondages interactifs  
  - **US09** : Cartes d’activité partagées

#### **Tâches Techniques :**
- **US07** :  
  - Stocker l’historique dans Firestore (2 jours)  
  - Ajouter des statistiques basiques (2 jours)  

- **US08** :  
  - Modèle de sondage dans Firestore (2 jours)  
  - Interface pour sondages (2 jours)  

- **US09** :  
  - Implémentation des cartes d’activité (3 jours)  
  - Tests UI (1 jour)  

**Sprint Review :**  
Les utilisateurs peuvent accéder à l’historique des conversations, participer à des sondages, et partager des cartes d’activité.

**Rétrospective :**  
- **Points Positifs :** Bonne gestion des délais et des priorités.  
- **Amélioration :** Renforcer la communication avec les utilisateurs finaux pour obtenir plus de feedbacks.

---

## **Résumé des Sprints**

| **Sprint** | **User Stories Couvertes** | **Statut** |
|------------|----------------------------|------------|
| Sprint 1   | US01, US02, US03           | Terminé    |
| Sprint 2   | US05, US06                 | Terminé    |
| Sprint 3   | US07, US08, US09           | En Cours   |
