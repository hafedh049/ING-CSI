## **Sprint 1 : Authentification et Messagerie de Base**  
**Objectif** : Permettre aux utilisateurs de s’inscrire, se connecter, et envoyer des messages texte.  

### **US01** – Inscription et Connexion avec Numéro de Téléphone  
**Tâches :**  
- Configuration de Firebase pour l’authentification. (**2 jours**)  
- Implémentation de l’envoi et validation du code OTP. (**3 jours**)  
- Gestion des erreurs et validation de l’interface. (**1 jour**)  

### **US03** – Envoi de Messages Texte  
**Tâches :**  
- Création du modèle de chat et stockage dans Firestore. (**2 jours**)  
- Mise en place de l’interface de chat (Flutter). (**2 jours**)  
- Gestion des notifications en temps réel. (**1 jour**)  

**Total Sprint 1 :** 11 jours  

#### **Revue du Sprint 1**  
- **Fonctionnalités Démontrées** : Authentification avec OTP, envoi de messages texte.  
- **Validation** : Inscription et connexion opérationnelles, messages échangés avec succès entre deux utilisateurs.  

#### **Rétrospective du Sprint 1**  
- **Ce qui a bien fonctionné** : Mise en œuvre fluide de Firebase et OTP.  
- **À améliorer** : Ajouter une gestion plus robuste des erreurs côté interface.  

---

## **Sprint 2 : Gestion du Profil et Partage Multimédia**  
**Objectif** : Ajouter la gestion du profil et les messages audio/vidéo.  

### **US02** – Gestion du Profil Utilisateur  
**Tâches :**  
- Implémentation du système de modification du profil. (**2 jours**)  
- Intégration de l’upload d’images dans Firebase Storage. (**2 jours**)  
- Tests de validation de l'interface et retour utilisateur. (**1 jour**)  

### **US03** – Envoi de Messages Audio et Vidéo  
**Tâches :**  
- Configuration de la capture audio/vidéo. (**2 jours**)  
- Compression et envoi des fichiers. (**3 jours**)  
- Gestion de la lecture et compatibilité. (**2 jours**)  

**Total Sprint 2 :** 12 jours  

#### **Revue du Sprint 2**  
- **Fonctionnalités Démontrées** : Profil personnalisé, messages audio et vidéo envoyés avec succès.  
- **Validation** : Photos de profil et statuts mis à jour, médias partagés sans problème.  

#### **Rétrospective du Sprint 2**  
- **Ce qui a bien fonctionné** : Partage fluide de médias avec compression.  
- **À améliorer** : Optimiser la vitesse de téléchargement des fichiers volumineux.  

---

## **Sprint 3 : Messagerie Éphémère et Personnalisation de l’Interface**  
**Objectif** : Offrir des messages temporaires et personnaliser l’interface.  

### **US05** – Messages Éphémères  
**Tâches :**  
- Ajout d’une option de minuterie pour les messages. (**2 jours**)  
- Suppression automatique après expiration. (**3 jours**)  
- Tests de validation et gestion des erreurs. (**1 jour**)  

### **US10** – Mode Sombre et Clair  
**Tâches :**  
- Intégration du basculement de thème dans Flutter. (**2 jours**)  
- Stockage des préférences utilisateur. (**1 jour**)  
- Tests de performance sur plusieurs appareils. (**1 jour**)  

**Total Sprint 3 :** 10 jours  

#### **Revue du Sprint 3**  
- **Fonctionnalités Démontrées** : Messages éphémères et thèmes personnalisés opérationnels.  
- **Validation** : Suppression automatique des messages fonctionnelle, basculement du thème fluide.  

#### **Rétrospective du Sprint 3**  
- **Ce qui a bien fonctionné** : Mise en œuvre des thèmes sans bugs.  
- **À améliorer** : Ajouter une option pour activer/désactiver les messages éphémères par conversation.  

---

## **Sprint 4 : IA et Transcription Audio**  
**Objectif** : Ajouter des suggestions de réponse par IA et la transcription audio.  

### **US04** – Transcription d’Audio  
**Tâches :**  
- Intégration de l’API Google Speech-to-Text. (**3 jours**)  
- Affichage des transcriptions dans le chat. (**2 jours**)  
- Tests de précision (80 % minimum). (**2 jours**)  

### **US06** – Suggestions de Réponses par IA  
**Tâches :**  
- Configuration d’un modèle NLP pour suggestions (OpenAI). (**3 jours**)  
- Affichage des suggestions pertinentes en contexte. (**2 jours**)  
- Tests d’utilisabilité avec différents scénarios. (**2 jours**)  

**Total Sprint 4 :** 14 jours  

#### **Revue du Sprint 4**  
- **Fonctionnalités Démontrées** : Transcription et suggestions d’IA fonctionnelles.  
- **Validation** : Transcriptions précises à 80 %, suggestions pertinentes fournies.  

#### **Rétrospective du Sprint 4**  
- **Ce qui a bien fonctionné** : IA efficace pour la génération de réponses.  
- **À améliorer** : Optimiser la latence de transcription pour les messages vocaux.  

---

## **Sprint 5 : Historique, Sondages et Collaboration**  
**Objectif** : Ajouter des outils d’analyse et de collaboration.  

### **US07** – Historique et Statistiques  
**Tâches :**  
- Affichage de l’historique des conversations. (**3 jours**)  
- Génération de statistiques d’usage. (**2 jours**)  
- Optimisation de la performance. (**1 jour**)  

### **US08** – Sondages de Groupe  
**Tâches :**  
- Création de sondages pour les groupes. (**2 jours**)  
- Participation et mise à jour des résultats. (**2 jours**)  

### **US09** – Cartes d’Activité  
**Tâches :**  
- Partage et accès aux cartes d’activité. (**2 jours**)  
- Tests de collaboration en groupe. (**1 jour**)  

**Total Sprint 5 :** 13 jours  

#### **Revue du Sprint 5**  
- **Fonctionnalités Démontrées** : Sondages et cartes d’activité prêts à l’emploi.  
- **Validation** : Historique et statistiques accessibles, collaboration en groupe fluide.  

#### **Rétrospective du Sprint 5**  
- **Ce qui a bien fonctionné** : Collaboration efficace entre les membres de groupe.  
- **À améliorer** : Ajouter des notifications spécifiques pour les sondages et activités partagées.  

---

## **Récapitulatif des Sprints et Tâches**  

| **Sprint**           | **User Stories**                                  | **Durée Estimée (jours)** |
|----------------------|---------------------------------------------------|---------------------------|
| Sprint 1             | US01, US03 (texte)                                | 11 jours                  |
| Sprint 2             | US02, US03 (audio/vidéo)                          | 12 jours                  |
| Sprint 3             | US05, US10                                        | 10 jours                  |
| Sprint 4             | US04, US06                                        | 14 jours                  |
| Sprint 5             | US07, US08, US09                                  | 13 jours                  |
