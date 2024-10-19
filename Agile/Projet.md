| **UCID** | **Description**                                                                                       | **Score** | **Priority** | **Definition of Done**                                                  |
| -------- | ----------------------------------------------------------------------------------------------------- | --------- | ------------ | ----------------------------------------------------------------------- |
| UC-01    | En tant qu'utilisateur, je veux m'inscrire avec mon numéro de téléphone pour accéder à l'application. | 3         | Must         | Inscription réussie et redirection vers l'écran d'accueil.              |
| UC-02    | En tant qu'utilisateur, je veux vérifier mon numéro de téléphone via OTP.                             | 5         | Must         | OTP envoyé et vérifié dans le délai imparti.                            |
| UC-03    | En tant qu'utilisateur, je veux me connecter automatiquement après inscription.                       | 3         | Must         | Connexion automatique sans nouvelle demande d'OTP sur le même appareil. |
| UC-04    | En tant qu'utilisateur, je veux mettre à jour ma photo de profil et mon nom.                          | 2         | Should       | Mise à jour du profil reflétée immédiatement.                           |
| UC-05    | En tant qu'utilisateur, je veux gérer les paramètres de confidentialité de mon statut.                | 4         | Should       | Modifications enregistrées et appliquées.                               |
| UC-06    | En tant qu'utilisateur, je veux envoyer et recevoir des messages en temps réel.                       | 8         | Must         | Messages envoyés et reçus instantanément.                               |
| UC-07    | En tant qu'utilisateur, je veux supprimer des messages pour tout le monde dans une limite de temps.   | 5         | Must         | Message supprimé avec confirmation pour tous les participants.          |
| UC-08    | En tant qu'utilisateur, je veux créer des groupes avec plusieurs participants.                        | 6         | Must         | Groupe créé avec succès et participants ajoutés.                        |
| UC-09    | En tant qu'utilisateur, je veux rechercher des messages par mot-clé.                                  | 5         | Should       | Résultats de recherche affichés correctement.                           |
| UC-10    | En tant qu'utilisateur, je veux épingler des conversations en haut de la liste.                       | 3         | Should       | Conversation épinglée correctement.                                     |
| UC-11    | En tant qu'utilisateur, je veux désactiver les notifications pour des chats spécifiques.              | 4         | Should       | Aucune notification pour les chats désactivés.                          |
| UC-12    | En tant qu'utilisateur, je veux envoyer et recevoir des images, vidéos et fichiers audio.             | 8         | Must         | Médias envoyés, reçus et prévisualisés avec succès.                     |
| UC-13    | En tant qu'utilisateur, je veux partager des contacts et ma position en direct.                       | 5         | Should       | Position et contacts partagés correctement.                             |
| UC-14    | En tant qu'utilisateur, je veux archiver des conversations sans les supprimer.                        | 4         | Should       | Conversations accessibles depuis l'archive.                             |
| UC-15    | En tant qu'utilisateur, je veux publier des statuts qui disparaissent après 24 heures.                | 6         | Must         | Statut publié et supprimé automatiquement après 24 heures.              |
| UC-16    | En tant qu'utilisateur, je veux voir les statuts de mes contacts.                                     | 5         | Must         | Statuts visibles dans la section appropriée.                            |
| UC-17    | En tant qu'utilisateur, je veux voir qui a consulté mon statut.                                       | 3         | Could        | Compteur de vues affiché correctement.                                  |
| UC-18    | En tant qu'utilisateur, je veux masquer ou réactiver les statuts de certains contacts.                | 4         | Could        | Paramètres de statut enregistrés.                                       |
| UC-19    | En tant qu'utilisateur, je veux recevoir des notifications pour les messages et appels.               | 6         | Must         | Notifications reçues instantanément.                                    |
| UC-20    | En tant qu'utilisateur, je veux passer et recevoir des appels vocaux.                                 | 8         | Must         | Appels connectés avec une qualité sonore claire.                        |
| UC-21    | En tant qu'utilisateur, je veux passer et recevoir des appels vidéo.                                  | 8         | Must         | Appels vidéo connectés avec fluidité.                                   |
| UC-22    | En tant qu'utilisateur, je veux basculer entre l'appel vocal et vidéo.                                | 6         | Could        | Basculement effectué sans interruption.                                 |
| UC-23    | En tant qu'utilisateur, je veux voir l'historique des appels et les appels manqués.                   | 5         | Should       | Historique des appels mis à jour correctement.                          |
| UC-24    | En tant qu'utilisateur, je veux activer l'authentification à deux facteurs.                           | 7         | Should       | Authentification configurée et testée avec succès.                      |
| UC-25    | En tant qu'utilisateur, je veux un chiffrement de bout en bout pour mes messages et appels.           | 10        | Must         | Messages sécurisés par chiffrement.                                     |
| UC-26    | En tant qu'utilisateur, je veux bloquer et signaler des contacts suspects.                            | 5         | Should       | Actions de blocage et signalement enregistrées immédiatement.           |
| UC-27    | En tant qu'utilisateur, je veux personnaliser le thème de l'application (mode clair/sombre).          | 3         | Could        | Changement de thème appliqué partout.                                   |
| UC-28    | En tant qu'utilisateur, je veux accéder à l'application sur mon ordinateur via un code QR.            | 7         | Should       | Connexion réussie et synchronisée avec le mobile.                       |
| UC-29    | En tant qu'utilisateur, je veux me déconnecter de tous les appareils à distance.                      | 5         | Could        | Déconnexion effectuée avec succès.                                      |
| UC-30    | En tant qu'utilisateur, je veux sauvegarder mes chats et médias sur le cloud.                         | 9         | Must         | Sauvegarde effectuée et restaurable.                                    |
| UC - 31  | As a user, I want to change the application language to my preferred language.                        | 4         | Should       | Language change applied throughout the app and reflected immediately.   |
Here is the **breakdown of epics, user stories (UCs), and tasks** based on your table:  

---

## **Epic 1: User Authentication and Profile Management**  
### **UC-01:** *Sign up with a phone number*  
- **Task 01.1:** Build phone number input screen  
- **Task 01.2:** Implement API to register users  
- **Task 01.3:** Redirect to home screen upon successful registration  

### **UC-02:** *Verify phone number with OTP*  
- **Task 02.1:** Generate OTP and send it via SMS  
- **Task 02.2:** Create OTP input screen  
- **Task 02.3:** Validate OTP within the set time  

### **UC-03:** *Auto-login after sign-up*  
- **Task 03.1:** Store user session securely  
- **Task 03.2:** Implement auto-login logic  
- **Task 03.3:** Skip OTP for already registered devices  

### **UC-04:** *Update profile photo and name*  
- **Task 04.1:** Create profile settings screen  
- **Task 04.2:** Implement image upload logic  
- **Task 04.3:** Ensure immediate profile update  

---

## **Epic 2: Chat and Messaging Features**  
### **UC-06:** *Send and receive messages in real-time*  
- **Task 06.1:** Integrate real-time messaging service  
- **Task 06.2:** Create chat interface  
- **Task 06.3:** Test message synchronization  

### **UC-07:** *Delete messages for everyone within time limits*  
- **Task 07.1:** Add "Delete for Everyone" option  
- **Task 07.2:** Implement message deletion logic  
- **Task 07.3:** Notify participants about deleted messages  

### **UC-08:** *Create groups with multiple participants*  
- **Task 08.1:** Build group creation screen  
- **Task 08.2:** Allow participant selection and invitation  
- **Task 08.3:** Store group data in the backend  

### **UC-09:** *Search messages by keyword*  
- **Task 09.1:** Implement search functionality  
- **Task 09.2:** Display matching messages  
- **Task 09.3:** Test search performance  

---

## **Epic 3: Media and File Sharing**  
### **UC-12:** *Send and receive images, videos, and audio*  
- **Task 12.1:** Integrate media picker  
- **Task 12.2:** Handle file uploads and previews  
- **Task 12.3:** Store media in the cloud  

### **UC-13:** *Share contacts and live location*  
- **Task 13.1:** Implement contact sharing functionality  
- **Task 13.2:** Integrate location services  
- **Task 13.3:** Display shared data in chat  

---

## **Epic 4: Status Management**  
### **UC-15:** *Publish statuses that disappear after 24 hours*  
- **Task 15.1:** Build status creation interface  
- **Task 15.2:** Store statuses with expiration logic  
- **Task 15.3:** Implement automatic status deletion  

### **UC-16:** *View statuses from contacts*  
- **Task 16.1:** Create status viewing interface  
- **Task 16.2:** Fetch and display statuses in real-time  

---

## **Epic 5: Notifications and Alerts**  
### **UC-19:** *Receive notifications for messages and calls*  
- **Task 19.1:** Integrate push notifications  
- **Task 19.2:** Customize alerts for messages and calls  
- **Task 19.3:** Test notifications on multiple devices  

---

## **Epic 6: Calls and Call History**  
### **UC-20:** *Make and receive voice calls*  
- **Task 20.1:** Integrate voice calling API  
- **Task 20.2:** Build call interface  
- **Task 20.3:** Test call quality  

### **UC-21:** *Make and receive video calls*  
- **Task 21.1:** Integrate video calling API  
- **Task 21.2:** Design video call interface  

---

## **Epic 7: Privacy and Security**  
### **UC-25:** *End-to-end encryption for messages and calls*  
- **Task 25.1:** Implement encryption library  
- **Task 25.2:** Test encrypted communication  

---

## **Epic 8: Application Customization**  
### **UC-27:** *Switch between light and dark themes*  
- **Task 27.1:** Implement theme toggling  
- **Task 27.2:** Apply theme across all screens  

### **UC-31:** *Change application language*  
- **Task 31.1:** Implement language selection screen  
- **Task 31.2:** Apply translations dynamically  

---

## **Epic 9: Device and Session Management**  
### **UC-29:** *Logout remotely from all devices*  
- **Task 29.1:** Build "Manage Devices" screen  
- **Task 29.2:** Implement remote logout  
