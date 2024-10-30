### **Sprint 1 : Authentification et Messagerie de Base**  
**Objectif** : Permettre aux utilisateurs de s’inscrire, se connecter, et envoyer des messages texte.

#### **US01** – Inscription et Connexion avec téléphone  
**Tâches :**  
- Configuration de Firebase pour l’authentification. (**2 jours**)  
- Implémentation de l’envoi et la validation du code OTP. (**3 jours**)  
- Gestion des erreurs et validation d’interface. (**1 jour**)  

#### **US03** – Envoi de Messages Texte  
**Tâches :**  
- Création du modèle de chat et stockage dans Firestore. (**2 jours**)  
- Mise en place de l’interface de chat (Flutter). (**2 jours**)  
- Gestion des notifications en temps réel. (**1 jour**)  

**Total Sprint 1 :** 11 jours

---

### **Sprint 2 : Gestion du Profil et Partage Multimédia**  
**Objectif** : Ajouter la gestion du profil et les messages audio/vidéo.  

#### **US02** – Gestion du Profil Utilisateur  
**Tâches :**  
- Implémentation du système de modification du profil. (**2 jours**)  
- Intégration de l'upload d’images (Firebase Storage). (**2 jours**)  
- Tests de validation d’interface et retour utilisateur. (**1 jour**)  

#### **US03** – Envoi de Messages Audio et Vidéo  
**Tâches :**  
- Configuration de la capture audio/vidéo. (**2 jours**)  
- Compression et envoi des fichiers. (**3 jours**)  
- Gestion de la lecture et compatibilité. (**2 jours**)  

**Total Sprint 2 :** 12 jours

---

### **Sprint 3 : Messagerie Éphémère et Personnalisation de l’Interface**  
**Objectif** : Permettre l’envoi de messages temporaires et personnalisation du thème.

#### **US05** – Messages Éphémères  
**Tâches :**  
- Ajout d’une option de minuterie pour les messages. (**2 jours**)  
- Mise en place de la suppression automatique après expiration. (**3 jours**)  
- Tests de validation et gestion des erreurs. (**1 jour**)  

#### **US10** – Mode Sombre et Clair  
**Tâches :**  
- Intégration du basculement de thème (Flutter). (**2 jours**)  
- Stockage de la préférence utilisateur. (**1 jour**)  
- Tests de performance sur plusieurs appareils. (**1 jour**)  

**Total Sprint 3 :** 10 jours

---

### **Sprint 4 : IA et Transcription Audio**  
**Objectif** : Ajouter des suggestions de réponse par IA et la transcription d’audio en texte.

#### **US04** – Transcription d’Audio  
**Tâches :**  
- Intégration d’une API de transcription (Google Speech-to-Text). (**3 jours**)  
- Affichage des transcriptions dans l’interface de chat. (**2 jours**)  
- Tests de précision (80 % minimum). (**2 jours**)  

#### **US06** – Suggestions de Réponses par IA  
**Tâches :**  
- Configuration d’un modèle NLP pour suggestions (OpenAI). (**3 jours**)  
- Affichage des suggestions pertinentes en contexte. (**2 jours**)  
- Tests d’utilisabilité avec différents scénarios. (**2 jours**)  

**Total Sprint 4 :** 14 jours

---

### **Sprint 5 : Historique, Sondages et Collaboration**  
**Objectif** : Offrir des outils de collaboration et d’analyse d’activité.

#### **US07** – Historique des Conversations et Statistiques  
**Tâches :**  
- Affichage de l’historique et recherche dans les discussions. (**3 jours**)  
- Génération et affichage des statistiques d’usage. (**2 jours**)  
- Tests de performance et optimisation. (**1 jour**)  

#### **US08** – Sondages de Groupe  
**Tâches :**  
- Implémentation de la création de sondages. (**2 jours**)  
- Participation et mise à jour des résultats en temps réel. (**2 jours**)  

#### **US09** – Cartes d’Activité  
**Tâches :**  
- Création et partage de cartes d’activité dans le chat. (**2 jours**)  
- Tests de collaboration en groupe. (**1 jour**)  

**Total Sprint 5 :** 13 jours

---

## **Récapitulatif des Sprints et Tâches**  

| **Sprint**           | **User Stories**                                  | **Durée Estimée (jours)** |
|----------------------|---------------------------------------------------|---------------------------|
| Sprint 1             | US01, US03 (texte)                                | 11 jours                  |
| Sprint 2             | US02, US03 (audio/vidéo)                          | 12 jours                  |
| Sprint 3             | US05, US10                                        | 10 jours                  |
| Sprint 4             | US04, US06                                        | 14 jours                  |
| Sprint 5             | US07, US08, US09                                  | 13 jours                  |

---

### **Plan de Travail : Revue et Rétrospective**  

- **Sprint Review :**  
  Chaque sprint se termine par une démonstration des fonctionnalités terminées (ex : envoi de messages, gestion du profil, etc.).  
- **Sprint Rétrospective :**  
  Discussions sur les points à améliorer : performance de l'application, précision des suggestions IA, gestion des erreurs.  
