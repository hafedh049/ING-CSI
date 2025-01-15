Les modes de chiffrement sont des méthodes utilisées pour appliquer des algorithmes de chiffrement sur les données. Ils déterminent comment les blocs de texte clair sont convertis en texte chiffré. Voici les principaux modes de chiffrement :

### 1. **ECB (Electronic Codebook)**

- Chaque bloc de texte clair est chiffré indépendamment.
- Simple, mais vulnérable aux attaques car les blocs identiques produisent des blocs chiffrés identiques.
- Utilisation : Rarement recommandé à cause de sa faible sécurité.

### 2. **CBC (Cipher Block Chaining)**

- Chaque bloc de texte clair est combiné avec le bloc chiffré précédent à l'aide d'une opération XOR avant d'être chiffré.
- Nécessite un vecteur d'initialisation (IV) unique pour le premier bloc.
- Utilisation : Courant pour protéger les fichiers.

### 3. **CFB (Cipher Feedback)**

- Le chiffrement se fait sur des segments de taille variable.
- Le texte clair est XORé avec une version chiffrée du précédent segment pour générer du texte chiffré.
- Utilisation : Idéal pour les flux de données.

### 4. **OFB (Output Feedback)**

- Utilise un flux de bits pseudo-aléatoires dérivé d'une fonction de chiffrement appliquée à un vecteur d'initialisation (IV).
- Utilisation : Bon pour les flux nécessitant une synchronisation.

### 5. **CTR (Counter Mode)**

- Remplace les blocs par un compteur incrémenté combiné avec un nonce (valeur unique).
- Permet un chiffrement parallèle, rapide et efficace.
- Utilisation : Très répandu pour le chiffrement moderne.
