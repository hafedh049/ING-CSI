Je vais vous expliquer l'attaque par réentrance (reentrancy attack) :

Une attaque par réentrance est une vulnérabilité critique dans les smart contracts Ethereum où un contrat malveillant peut rappeler récursivement le contrat victime avant que la première exécution ne soit terminée, permettant ainsi de manipuler l'état du contrat.

Voici un exemple concret :

```js
// Contrat vulnérable
contract VulnerableContract {
    mapping(address => uint) public balances;
    
    function withdraw(uint amount) public {
        require(balances[msg.sender] >= amount);
        // Danger : envoi des fonds avant la mise à jour du solde
        (bool success, ) = msg.sender.call{value: amount}("");
        require(success);
        balances[msg.sender] -= amount;
    }
}

// Contrat malveillant
contract AttackContract {
    VulnerableContract public target;
    
    receive() external payable {
        if (address(target).balance >= 1 ether) {
            // Rappel récursif avant que le solde ne soit mis à jour
            target.withdraw(1 ether);
        }
    }
}
```

Pour prévenir cette attaque, il faut :

1. Suivre le pattern "Checks-Effects-Interactions" :
```js
function withdraw(uint amount) public {
    require(balances[msg.sender] >= amount); // Checks
    balances[msg.sender] -= amount;          // Effects
    (bool success, ) = msg.sender.call{value: amount}(""); // Interactions
    require(success);
}
```

2. Utiliser un modificateur de réentrance :
```js
bool private locked;

modifier noReentrant() {
    require(!locked, "Pas de réentrance");
    locked = true;
    _;
    locked = false;
}

function withdraw(uint amount) public noReentrant {
    // ... code sécurisé
}
```

Cette attaque est particulièrement dangereuse car elle peut permettre à un attaquant de :
- Vider les fonds du contrat
- Contourner les vérifications de solde
- Exécuter plusieurs fois des opérations qui ne devraient être faites qu'une seule fois

Le hack du DAO en 2016, qui a conduit au fork d'Ethereum Classic, est l'exemple le plus célèbre d'une attaque par réentrance qui a causé la perte de millions de dollars.