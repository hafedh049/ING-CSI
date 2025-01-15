### **Contrats intelligents (Smart Contracts)**

Un **contrat intelligent** est un programme informatique auto-exécuté qui fonctionne sur une blockchain. Il exécute automatiquement des actions prédéfinies lorsque des conditions spécifiques sont remplies. Ces contrats éliminent le besoin d'intermédiaires et garantissent une exécution fiable et transparente.

---

### **Caractéristiques des contrats intelligents :**

1. **Automatisation** : Une fois déployé, le contrat intelligent s'exécute automatiquement selon les règles définies.
2. **Fiabilité** : Les transactions et les données sont enregistrées sur une blockchain immuable.
3. **Transparence** : Toutes les parties peuvent voir le code et vérifier les règles du contrat.
4. **Sécurité** : Protégé par la technologie blockchain, ce qui rend la falsification ou la modification quasi impossible.
5. **Efficacité** : Élimine les délais et les coûts associés aux intermédiaires.

---

### **Exemple simple : Vente de biens**

Un contrat intelligent peut être utilisé pour une transaction immobilière :

1. L'acheteur envoie les fonds à l'adresse du contrat.
2. Le contrat vérifie si les fonds sont suffisants.
3. Une fois la condition remplie, la propriété est automatiquement transférée à l'acheteur et les fonds sont envoyés au vendeur.

---

### **Domaines d’application :**

- **Finance décentralisée (DeFi)** : Prêts, échanges, et paiements automatisés.
- **Assurance** : Versement automatique des indemnités en cas d’événement déclencheur.
- **Jeux vidéo** : Propriété vérifiable des actifs numériques (NFTs).
- **Chaîne d’approvisionnement** : Suivi des produits et automatisation des paiements.

Les contrats intelligents sont principalement développés sur des plateformes comme **Ethereum** (en utilisant des langages comme js) et sont à la base de nombreuses innovations blockchain.

```js
pragma solidity ^0.8.0;

contract ExempleContrat {
    address public owner;
    uint public balance;
    
    constructor() {
        owner = msg.sender;
        balance = 0;
    }
    
    function deposit() public payable {
        balance += msg.value;
    }
    
    function withdraw(uint amount) public {
        require(msg.sender == owner, "Vous n'êtes pas autorisé.");
        require(amount <= balance, "Solde insuffisant.");
        balance -= amount;
        payable(owner).transfer(amount);
    }
}
```

This is a basic Ethereum smart contract that implements a simple digital wallet. Let me break down its functionality:

1. State Variables:
   - `owner`: Stores the address of the contract owner
   - `balance`: Keeps track of the contract's balance

2. Constructor:
   - Executes when the contract is deployed
   - Sets the contract owner to the address that deployed the contract (`msg.sender`)
   - Initializes the balance to 0

3. Functions:
   - `deposit()`: 
     - Allows anyone to deposit Ether into the contract
     - The `payable` modifier enables the function to receive Ether
     - Updates the balance by adding the sent amount (`msg.value`)
   
   - `withdraw(uint amount)`:
     - Allows only the owner to withdraw funds
     - Has two security checks using `require`:
       - Verifies that the caller is the owner
       - Ensures there are sufficient funds for withdrawal
     - Updates the balance and transfers the amount to the owner

The error messages in the code are in French:
- "Vous n'êtes pas autorisé" means "You are not authorized"
- "Solde insuffisant" means "Insufficient balance"

This contract demonstrates basic concepts of smart contract development including:
- Access control (only owner can withdraw)
- State management (tracking balance)
- Security checks (require statements)
- Handling Ether transactions (deposit and withdraw functions)

---

a) Que fait ce contrat intelligent? (1 point)
Ce contrat intelligent agit comme un portefeuille numérique simple sur Ethereum. Il permet de:
- Stocker des ethers via une fonction de dépôt accessible à tous
- Gérer les retraits qui sont uniquement autorisés pour le propriétaire du contrat
- Suivre le solde du contrat

b) Quel est le rôle de la fonction require dans ce code? (1 point)
La fonction `require` joue un rôle de sécurité crucial en vérifiant des conditions qui doivent être remplies avant l'exécution du code. Dans ce contrat, elle est utilisée pour:
- Vérifier que seul le propriétaire peut effectuer des retraits
- S'assurer que le montant du retrait ne dépasse pas le solde disponible
Si ces conditions ne sont pas remplies, la transaction est annulée et un message d'erreur est renvoyé.

c) Quels risques de sécurité peuvent exister dans ce contrat et comment les éviter? (1 point)
Plusieurs risques de sécurité peuvent être identifiés:

1. Risque de réentrance: Le contrat transfère les fonds avant de mettre à jour le solde, ce qui pourrait être exploité. Pour l'éviter, il faudrait:
   - Appliquer le pattern "checks-effects-interactions"
   - Mettre à jour le solde avant d'effectuer le transfert

2. Pas de gestion du changement de propriétaire: Si le propriétaire perd accès à son compte, les fonds sont bloqués. Solutions:
   - Ajouter une fonction de transfert de propriété
   - Implémenter un système de propriété multiple

3. Absence de limitation des montants: Pas de limite sur les dépôts/retraits. Pour sécuriser:
   - Ajouter des limites maximales de transaction
   - Implémenter des délais entre les retraits importants

