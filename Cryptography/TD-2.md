# Question

![[Pasted image 20250115133327.png]]

Les algorithmes de chiffrement asymétriques (également appelés cryptographie à clé publique) partagent plusieurs éléments généraux essentiels pour leur fonctionnement. Voici les principaux :

---

### 1. **Clé publique et clé privée**

- **Clé publique** : Utilisée pour chiffrer les données ou vérifier une signature numérique. Elle est partagée publiquement.
- **Clé privée** : Utilisée pour déchiffrer les données ou créer une signature numérique. Elle est gardée secrète par le propriétaire.

---

### 2. **Fonction mathématique unidirectionnelle**

- Les algorithmes reposent sur une fonction mathématique difficile à inverser sans une information secrète (par exemple, la clé privée).
- Exemple : Facteurisation des grands nombres (RSA) ou le problème du logarithme discret (ECDSA, Diffie-Hellman).

---

### 3. **Couplage des clés (relation entre clé publique et clé privée)**

- La clé publique et la clé privée sont mathématiquement liées, mais il est pratiquement impossible de dériver la clé privée à partir de la clé publique.

---

### 4. **Génération de clés sécurisée**

- Les clés doivent être générées de manière aléatoire et sécurisée pour garantir leur unicité et leur robustesse contre les attaques.
- Les algorithmes utilisent souvent des générateurs de nombres aléatoires cryptographiques pour cette étape.

---

### 5. **Chiffrement et déchiffrement**

- **Chiffrement** : La clé publique est utilisée pour convertir un message clair en un message chiffré.
- **Déchiffrement** : La clé privée est utilisée pour reconvertir le message chiffré en message clair.

---

### 6. **Authentification**

- Les algorithmes asymétriques permettent de vérifier l’identité des parties via des signatures numériques, grâce à la relation entre la clé publique et la clé privée.

---

### 7. **Échange de clés**

- Certains algorithmes, comme Diffie-Hellman, sont utilisés pour échanger des clés de manière sécurisée, permettant ensuite d’utiliser des algorithmes symétriques pour la communication.

---

### 8. **Taille des clés**

- La taille des clés est un élément critique qui affecte la sécurité de l'algorithme. Par exemple :
    - RSA : 2048 bits ou plus.
    - ECC (Elliptic Curve Cryptography) : clés plus courtes (256 bits) mais équivalentes en sécurité à RSA 2048.

---

### 9. **Preuves mathématiques de sécurité**

- Les algorithmes reposent sur des hypothèses mathématiques qui garantissent leur résistance aux attaques (par exemple, difficulté de factoriser ou de résoudre un logarithme discret).


![[Pasted image 20250115133641.png]]

### Usages de la cryptographie symétrique et asymétrique

La cryptographie symétrique et asymétrique ont des rôles complémentaires dans la sécurité informatique. Voici leurs principaux usages :

---

### **1. Cryptographie symétrique**

- **Principe** : Une seule clé est utilisée pour chiffrer et déchiffrer les données.

#### Usages :

1. **Chiffrement de données volumineuses :**
    
    - Utilisée pour sécuriser les fichiers, bases de données, disques durs, ou flux réseau.
    - Exemples : AES pour HTTPS, VPN, ou chiffrement de fichiers.
2. **Transmission de données en temps réel :**
    
    - Pour des communications nécessitant rapidité, comme les appels vidéo, audio, ou transfert de fichiers.
    - Exemple : Protocoles comme TLS utilisent des algorithmes symétriques après l'échange initial des clés.
3. **Chiffrement des communications entre systèmes fermés :**
    
    - Utilisé lorsque les deux parties ont déjà partagé une clé secrète (par exemple, dans des systèmes industriels ou des réseaux internes).
4. **Chiffrement de sessions temporaires (chiffrement hybride) :**
    
    - Les clés de session sont générées pour des connexions temporaires et échangées à l’aide de cryptographie asymétrique.
5. **Chiffrement des disques et périphériques :**
    
    - Exemple : BitLocker, LUKS (Linux Unified Key Setup).
6. **Algorithmes communs :**
    
    - AES (Advanced Encryption Standard), DES, Triple DES, ChaCha20.

---

### **2. Cryptographie asymétrique**

- **Principe** : Utilise une paire de clés (publique et privée) pour chiffrer et déchiffrer.

#### Usages :

1. **Échange de clés sécurisé :**
    
    - Permet de partager des clés de chiffrement symétriques sur un réseau non sécurisé.
    - Exemple : Algorithmes Diffie-Hellman ou RSA pour l’échange de clés.
2. **Signature numérique :**
    
    - Garantit l’intégrité et l’authenticité d’un message ou d’un fichier.
    - Exemple : Les certificats SSL/TLS pour sécuriser les sites web.
3. **Authentification :**
    
    - Vérifie l’identité d’une personne, d’un appareil ou d’un service.
    - Exemple : SSH utilise des clés publiques pour authentifier les utilisateurs.
4. **Chiffrement des courriels et des messages :**
    
    - Les messages sont chiffrés avec une clé publique et ne peuvent être déchiffrés qu’avec la clé privée correspondante.
    - Exemple : PGP (Pretty Good Privacy) pour les courriels.
5. **Infrastructure à clés publiques (PKI) :**
    
    - Gestion des certificats numériques pour sécuriser les communications sur Internet (SSL/TLS, HTTPS).
6. **Stockage de données sensibles :**
    
    - Les données peuvent être chiffrées avec une clé publique et déchiffrées uniquement par le propriétaire de la clé privée.
7. **Algorithmes communs :**
    
    - RSA, ECC (Elliptic Curve Cryptography), DSA (Digital Signature Algorithm).

---

### **Résumé des différences d’usage**

|**Aspect**|**Cryptographie Symétrique**|**Cryptographie Asymétrique**|
|---|---|---|
|**Vitesse**|Très rapide (adapté aux gros volumes)|Plus lent (adapté à des petites données)|
|**Clé utilisée**|Une seule clé partagée entre les deux parties|Deux clés : une publique et une privée|
|**Usages principaux**|Chiffrement de données volumineuses|Authentification, signatures, échange de clés|
|**Exemples courants**|AES, DES, ChaCha20|RSA, Diffie-Hellman, ECC|

![[Pasted image 20250115133821.png]]

### **Avantages et inconvénients de la cryptographie asymétrique par rapport à la symétrique**

Les deux types de cryptographie ont des forces et des faiblesses en fonction des cas d’utilisation. Voici une comparaison détaillée :

---

### **Avantages de la cryptographie asymétrique :**

1. **Pas besoin de partager une clé secrète :**
    
    - La clé publique peut être librement distribuée, éliminant le besoin d’un canal sécurisé pour transmettre la clé, contrairement à la cryptographie symétrique.
    - Cela simplifie la gestion des clés, surtout pour les systèmes à grande échelle.
2. **Authentification intégrée :**
    
    - La cryptographie asymétrique permet de vérifier l’identité des parties grâce aux signatures numériques, ce qui n’est pas possible avec la cryptographie symétrique.
3. **Protection renforcée contre le vol de clés :**
    
    - Si une clé publique est compromise, elle ne permet pas de déchiffrer les données chiffrées. Seule la clé privée, qui reste secrète, peut déchiffrer les messages.
4. **Utilisation pour l'échange sécurisé de clés symétriques :**
    
    - La cryptographie asymétrique est souvent utilisée dans des systèmes hybrides pour établir une connexion sécurisée (par exemple, dans SSL/TLS), puis passer à la cryptographie symétrique pour le reste de la communication.
5. **Infrastructure à clé publique (PKI) :**
    
    - La gestion des certificats numériques permet une sécurité à grande échelle dans les systèmes comme HTTPS ou les signatures électroniques.

---

### **Inconvénients de la cryptographie asymétrique :**

1. **Lenteur :**
    
    - Les opérations asymétriques, comme le chiffrement et le déchiffrement, sont beaucoup plus lentes que celles des algorithmes symétriques.
    - Cela les rend peu pratiques pour chiffrer de grandes quantités de données.
2. **Complexité de la mise en œuvre :**
    
    - Les algorithmes asymétriques nécessitent des opérations mathématiques complexes, ce qui les rend plus difficiles à implémenter correctement.
3. **Taille des clés :**
    
    - Les clés asymétriques doivent être beaucoup plus longues que les clés symétriques pour offrir un niveau de sécurité équivalent (par exemple, une clé RSA de 2048 bits est approximativement équivalente à une clé AES de 128 bits).
4. **Consommation de ressources :**
    
    - Les algorithmes asymétriques consomment davantage de puissance de calcul, ce qui peut être un problème pour les dispositifs à ressources limitées (par exemple, les appareils IoT).

---

### **Avantages de la cryptographie symétrique par rapport à l’asymétrique :**

1. **Vitesse et efficacité :**
    
    - Les algorithmes symétriques sont beaucoup plus rapides, ce qui les rend adaptés au chiffrement de grandes quantités de données.
2. **Moins de ressources nécessaires :**
    
    - Ils consomment moins de ressources en termes de calcul, ce qui est important pour les systèmes embarqués ou en temps réel.
3. **Simplicité d’implémentation :**
    
    - Les algorithmes symétriques sont généralement plus simples à implémenter et à comprendre.

---

### **Inconvénients de la cryptographie symétrique :**

1. **Problème de distribution des clés :**
    
    - La clé doit être partagée entre les deux parties via un canal sécurisé, ce qui peut être compliqué à grande échelle.
2. **Manque d’authentification :**
    
    - La cryptographie symétrique ne permet pas de vérifier l’identité des parties, ce qui peut rendre le système vulnérable à des attaques par interception (Man-in-the-Middle).
3. **Risque en cas de compromission :**
    
    - Si une clé est compromise, toutes les communications passées et futures utilisant cette clé sont compromises.

---

### **Résumé des différences :**

|**Critère**|**Cryptographie Asymétrique**|**Cryptographie Symétrique**|
|---|---|---|
|**Vitesse**|Lente (moins adaptée pour les gros volumes)|Très rapide|
|**Gestion des clés**|Facile (clé publique librement distribuée)|Difficile (clé secrète à partager)|
|**Authentification**|Possible grâce aux signatures numériques|Non possible directement|
|**Consommation des ressources**|Élevée|Faible|
|**Cas d'utilisation**|Échange de clés, signatures numériques|Chiffrement de données volumineuses|

![[Pasted image 20250115134042.png]]

### **Points importants pour la clé publique et la clé privée :**

Dans la cryptographie asymétrique, la sécurité repose sur l’utilisation et la gestion appropriées de la **clé publique** et de la **clé privée**. Voici les points critiques pour chacune :

---

### **Clé publique :**

1. **Distribution sécurisée :**
    
    - Bien que la clé publique soit destinée à être partagée, il est essentiel de s'assurer qu'elle provient d'une source fiable.
    - Exemple : Utilisation de certificats numériques pour garantir l’authenticité de la clé publique.
2. **Protection contre la modification :**
    
    - Si un attaquant modifie une clé publique (attaque de type "Man-in-the-Middle"), il peut intercepter ou altérer les communications.
    - Solution : Vérification de l’intégrité de la clé via des mécanismes comme les PKI.
3. **Accessibilité :**
    
    - La clé publique doit être facilement accessible à tous les utilisateurs ou systèmes autorisés pour garantir une communication fluide.
4. **Taille et robustesse :**
    
    - La clé doit être suffisamment longue pour résister aux attaques par force brute ou à des algorithmes avancés comme ceux des ordinateurs quantiques (par exemple, RSA avec une clé de 2048 ou 4096 bits).

---

### **Clé privée :**

1. **Secret absolu :**
    
    - La clé privée doit rester strictement confidentielle. Si elle est compromise, un attaquant peut déchiffrer les données chiffrées ou signer des messages frauduleux.
    - Exemple : Ne jamais partager la clé privée, même avec des partenaires de confiance.
2. **Stockage sécurisé :**
    
    - Elle doit être stockée dans un endroit sécurisé, comme un module matériel de sécurité (HSM) ou des solutions logicielles utilisant des environnements protégés.
    - Éviter de la stocker en clair dans des fichiers ou des bases de données.
3. **Sauvegarde protégée :**
    
    - Des copies de la clé privée doivent être sauvegardées pour éviter la perte, mais ces sauvegardes doivent être également protégées par des mots de passe ou des mécanismes de chiffrement.
4. **Protection contre les accès non autorisés :**
    
    - Les systèmes utilisant la clé privée doivent implémenter des contrôles d’accès stricts, comme l’authentification multifactorielle (MFA).
5. **Taille et robustesse :**
    
    - Comme pour la clé publique, elle doit être suffisamment longue pour garantir une sécurité optimale contre les attaques.
6. **Expiration et rotation :**
    
    - Les clés privées doivent avoir une durée de vie limitée et être régulièrement renouvelées pour réduire les risques liés à une éventuelle compromission.
7. **Utilisation dans des environnements sécurisés :**
    
    - Toute opération utilisant la clé privée (comme le déchiffrement ou la signature numérique) doit se faire dans un environnement protégé contre les attaques, comme les attaques par canaux auxiliaires.

---

### **Résumé des différences entre les deux :**

|**Critère**|**Clé publique**|**Clé privée**|
|---|---|---|
|**Accessibilité**|Accessible à tous|Strictement confidentielle|
|**Utilisation principale**|Chiffrement et vérification des signatures|Déchiffrement et création de signatures|
|**Gestion**|Distribution vérifiable|Stockage sécurisé et sauvegarde protégée|
|**Exposition**|Exposée aux tiers|Protégée de toute exposition externe|

# Exercice 1

![[Pasted image 20250115134237.png]]
### **Données :**

- $n = 119$
- $e = 5$

RSA repose sur la factorisation de $n$ en ses deux facteurs premiers $p$ et $q$, et le calcul de l'exposant privé $d$.

---

### **1. Trouver $p$ et $q$ :**

$n = p \times q$, où $p$ et $q$ sont des nombres premiers.

Nous testons les diviseurs de $n = 119$ pour trouver $p$ et $q$ :

- $119 \div 2$ : non divisible.
- $119 \div 3$ : non divisible.
- $119 \div 5$ : non divisible.
- $119 \div 7 = 17$, donc $p = 7$ et $q = 17$.

**Résultat :**

- $p = 7$
- $q = 17$

---

### **2. Calculer $d$, la clé secrète :**

Pour calculer $d$, nous avons besoin de l’inverse modulaire de $e$ modulo $\phi(n)$, où $\phi(n)$ est la fonction d’Euler définie par :

$\phi(n) = (p - 1) \times (q - 1)$

#### Étape 1 : Calcul de $\phi(n)$

$\phi(n) = (7 - 1) \times (17 - 1) = 6 \times 16 = 96$
#### Étape 2 : Trouver $d$

$d$ est l'inverse de $e = 5$ modulo $\phi(n) = 96$, soit :

$d \times e \equiv 1 \pmod{\phi(n)}$

Cela revient à résoudre l'équation **diophantienne** suivante :

$5d \equiv 1 \pmod{96}$

On utilise l’algorithme d’Euclide étendu pour trouver $d$.
#### Algorithme d’Euclide étendu :

1. Trouver les coefficients $x$ et $y$ tels que :
    
    $5x+96y =1$
    
1. Divisions successives :
    
    $96 = 19 \times 5 + 1$
    $5 = 5 \times 1 + 0$
    
    En remontant les étapes :
    
    $1 = 96 - 19 \times 5$
    
    Donc, $x = −19$ et $y=1$.
    
3. $d \equiv 19 \pmod{96}$, soit :
    
    $d = 96 - 19 = 77$

**Résultat :**

- $d=77$

---

### **Restate the Problem**

We are solving for $d$ such that:

$d \times e \equiv 1 \pmod{\phi(n)}$

Where:

- $e = 5$
- $\phi(n) = 96$

### **Method: Iterative Testing**

We test values of dd starting from $1$ until the equation $5 \times d \mod 96 = 1$ is satisfied.

1. Start with $d = 1$:
    
    $5 \times 1 \mod 96 = 5 \quad (\neq 1)$
    
1. $d = 2$:
    
    $5 \times 2 \mod 96 = 10 \quad (\neq 1)$
    
1. $d = 3$:
    
    $5 \times 3 \mod 96 = 15 \quad (\neq 1)$

...

16. $d = 77$:

$5 \times 77 \mod 96 = 385 \mod 96 = 1$

Thus, $d = 77$ satisfies the equation.

![[Pasted image 20250115135751.png]]

Pour résoudre cette question, nous devons factoriser $n = 14803$ en deux nombres premiers pp et qq, en utilisant les informations données.

---

### **Données :**

- $n = 14803$
- $\phi(n) = 14560$

---

### **Étapes pour trouver $p$ et $q$:**

#### 1. Relation entre $n$, $\phi(n)$, $p$, et $q$

La fonction d’Euler $\phi(n)$ pour un produit de deux nombres premiers $p$ et $q$ est donnée par :

$\phi(n) = (p - 1)(q - 1)$

On sait aussi que :

$n = p \times q$

---

#### 2. Reformulation en termes de $p + q$ et $p \times q$

- Développons $\phi(n)$:
    
    $\phi(n) = (p - 1)(q - 1) = pq - (p + q) + 1$
    
    Comme $pq = n$, nous avons :
    
    $\phi(n) = n - (p + q) + 1$
    
    D’où :
    
    $p + q = n - \phi(n) + 1$
    
- Substituons les valeurs :
    
    $p + q = 14803 - 14560 + 1 = 244$
    
- Nous avons maintenant un système d’équations :
    
    $p + q = 244$ 
    $p \times q = 14803$

---

#### 3. Résolution du système (quadratique)

Les deux équations peuvent être exprimées sous forme d'une équation quadratique :

We substitute $p = 244 - q$ into the second equation:

$p \times q = 14803$

Substituting $p = 244 - q$:

$(244 - q) \times q = 14803$

Expanding:

$244q - q^2 = 14803$

$- q^2 + 244q  - 14803 = 0$

Multiplying both sides with $(-1)$

$q^2 - 244q + 14803 = 0$

$x^2 - (p + q)x + (p \times q) = 0$

Substituons :

$x^2 - 244x + 14803 = 0$

Utilisons la formule quadratique pour résoudre :

$x = \frac{-(p + q) \pm \sqrt{(p + q)^2 - 4(p \times q)}}{2}$

Substituons :

$x = \frac{-244 \pm \sqrt{244^2 - 4 \times 14803}}{2}$ 
$x = \frac{-244 \pm \sqrt{59536 - 59212}}{2}$ 
$x = \frac{-244 \pm \sqrt{324}}{2}$ 
$x = \frac{-244 \pm 18}{2}$
#### Calcul des solutions :

1. $x_1 = \frac{-244 + 18}{2} = \frac{-226}{2} = 113$
2. $x_2 = \frac{-244 - 18}{2} = \frac{-262}{2} = 131$

---
### **Résultats :**

Les deux facteurs sont :

$p = 113, \, q = 131$

**Vérification :**

- $p \times q = 113 \times 131 = 14803$ ✅
- $\phi(n) = (p - 1)(q - 1) = 112 \times 130 = 14560$ ✅

---
### **Conclusion :**

Avec $\phi(n)$ révélé, la factorisation est possible. Les facteurs premiers sont $p = 113$ et $q = 131$.

![[Pasted image 20250115140952.png]]

Le protocole de Diffie-Hellman permet à deux parties, Alice et Bob, de partager une clé secrète de manière sécurisée en utilisant des valeurs publiques et des valeurs privées. Suivons les étapes pour compléter ce protocole.

---

### **Données :**

- $p = 17$ (modulo premier)
- $g = 3$ (base ou générateur)
- Alice choisit $a = 7$ (clé privée d'Alice)
- Bob choisit $b = 4$ (clé privée de Bob)

---

### **Étapes du protocole Diffie-Hellman :**

#### 1. Calcul des valeurs publiques ($A$ et $B$) :

- Alice calcule sa valeur publique $A$ :
    
    $A = g^a \mod p$
    
    Substituons les valeurs :
    
    $A = 3^7 \mod 17$
    $3^7 = 2187$,  $2187 \mod 17 = 11$
    
    Donc, $A = 11$.
    
- Bob calcule sa valeur publique $B$ :
    
    $B = g^b \mod p$
    
    Substituons les valeurs :
    
    $B = 3^4 \mod 17$
    $3^4 = 81$, $81 \mod 17 = 13$
    
    Donc, $B = 13$.
    

---

#### 2. Échange des valeurs publiques :

- Alice envoie $A = 11$ à Bob.
- Bob envoie $B = 13$ à Alice.

---

#### 3. Calcul de la clé secrète commune :

- Alice utilise $B = 13$ pour calculer la clé secrète $K$ :
    
    $K = B^a \mod p$
    
    Substituons :
    
    $K = 13^7 \mod 17$
    
    Utilisons une méthode rapide d’exponentiation modulaire :
    
    $13^7 \mod 17 = 4$
    
    Donc, la clé secrète pour Alice est $K = 4$.
    
- Bob utilise $A = 11$ pour calculer la clé secrète $K$ :
    
    $K = A^b \mod p$
    
    Substituons :
    
    $K = 11^4 \mod 17$
    
    Calculons :
    
    $11^4 \mod 17 = 11^2 \cdot 11^2 \mod 17 = 121 \cdot 121 \mod 17 = 4$
    
    Donc, la clé secrète pour Bob est $K = 4$.

---
### **Clé secrète commune :**

La clé secrète partagée entre Alice et Bob est :

$K = 4$

---
### **Résumé du protocole :**

1. **Valeurs publiques :** $p = 17$, $g = 3$.
2. **Valeurs privées :** $a = 7$ (Alice), $b = 4$ (Bob).
3. **Valeurs publiques calculées :**
    - $A = 11$ (Alice)
    - $B = 13$ (Bob)
4. **Clé secrète commune :** $K = 4$.

![[Pasted image 20250115144012.png]]
![[Pasted image 20250115144136.png]]

### **1. Démonstration de l'opération de déchiffrement**

**Schéma de chiffrement :**

$C_j = E_K(C_{j-1} \oplus X_j)$

Pour déchiffrer $X_j$, nous utilisons l'inverse $D_K$ de $E_K$ :

$D_K(C_j) = C_{j-1} \oplus X_j$

Isolons $X_j$ :

$X_j = D_K(C_j) \oplus C_{j-1}$

Ainsi, en utilisant $C_{j-1}$, $C_j$, et l'opération $D_K$, nous retrouvons $X_j$.

**Rappel des propriétés utilisées :**

- $A \oplus A = 0$ pour tout $A$.
- $A \oplus 0 = A$ pour tout $A$.
- $(A \oplus B) \oplus B = A$.

---

### **2. Peut-on chiffrer un bloc quelconque $X_j$ sans chiffrer les blocs qui le précèdent ?**

**Réponse : Non.**

Le chiffrement d'un bloc $X_j$ dépend de $C_{j-1}$, qui est le résultat du chiffrement du bloc précédent $X_{j-1}$. Si $C_{j-1}$ n’est pas calculé, il est impossible de chiffrer $X_j$.

---

### **3. Peut-on déchiffrer un bloc quelconque $C_j$ sans déchiffrer les blocs qui le précèdent ?**

**Réponse : Non.**

Pour déchiffrer $C_j$ et retrouver $X_j$, il est nécessaire de connaître $C_{j-1}$ car :

$X_j = D_K(C_j) \oplus C_{j-1}$

Sans $C_{j-1}$, le déchiffrement est impossible.

---

### **4. Peut-on déchiffrer un bloc $C_j$ en l'absence des autres blocs chiffrés ?**

**Réponse : Oui, mais sous condition.**

Si le bloc précédent $C_{j-1}$ et la clé $K$ sont connus, il est possible de déchiffrer $C_j$ indépendamment des autres blocs. Toutefois, si $C_{j-1}$ est manquant, le déchiffrement devient impossible.

---

### **5. Cas où $E_K(x) = D_K(x) = K \oplus x$**

Si $E_K(x) = K \oplus x$, le chiffrement devient :

$C_j = E_K(C_{j-1} \oplus X_j) = K \oplus (C_{j-1} \oplus X_j)$

Développons l’équation pour un attaquant connaissant $C_{j-1}, C_j, X_{j-1}$, et $X_j$ :

$C_j = K \oplus (C_{j-1} \oplus X_j)$

L’attaquant peut isoler KK :

$C_j \oplus (C_{j-1} \oplus X_j)$

Ainsi, si un attaquant dispose des deux blocs consécutifs $(X_{j-1}, X_j)$ et $(C_{j-1}, C_j)$, il peut retrouver la clé $K$.

---

### **6. Échange sécurisé de la clé KK**

Pour échanger la clé $K$ de manière sécurisée, $A$ et $B$ utilisent un chiffrement asymétrique.

#### a. Avec quelle clé $A$ doit-elle chiffrer $K$ ?

A doit chiffrer $K$ avec la **Clé publique de B** pour garantir que seul $B$ (avec sa clé privée) puisse déchiffrer $K$.

#### b. Avec quelle clé $B$ déchiffre $K_c$ ?

B déchiffre $K_c$ avec sa **clé privée**, car seule cette clé peut inverser le chiffrement effectué avec la clé publique de $B$.

#### c. Pourquoi cette méthode n’est pas authentifiée ? Solution proposée.

**Problème :** Cette méthode n'est pas authentifiée, car rien ne prouve que $K$ provient réellement d'$A$. Un attaquant pourrait intercepter $K_c$ et envoyer une clé malveillante.

**Solution :** Pour garantir l'authenticité :

1. A signe $K$ avec sa **clé privée**. La signature est un hachage de $K$ chiffré avec la clé privée d’$A$.
2. B peut vérifier cette signature en utilisant la **clé publique d’$A$** pour confirmer que $K$ provient bien d’$A$.

>[!important]
>Ainsi, le chiffrement garantit la confidentialité et la signature garantit l'authenticité.

