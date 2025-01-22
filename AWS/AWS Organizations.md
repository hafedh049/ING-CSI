**AWS Organizations** est un service qui permet de gérer et de consolider plusieurs comptes AWS dans une structure hiérarchique centralisée. 

**AWS Organizations** vous permet de créer un environnement multi-comptes où vous pouvez : 
	• Gérer de manière centralisée les politiques d'accès. 
	• Regrouper les comptes AWS pour appliquer des règles spécifiques. 
	• Optimiser les coûts grâce à la facturation consolidée.

# Structure

1- **Comptes AWS** 
	• **Compte racine (Root Account)** : Le premier compte créé dans AWS Organizations. Il dispose d’un contrôle total et gère l’organisation entière. 
	• **Comptes membres (Member Accounts)** : Tous les autres comptes ajoutés à l’organisation. 
	
2- **Unités Organisationnelles (Organizational Units, OU)** : Regroupements de comptes dans une structure hiérarchique. Ils permettent d’appliquer des politiques spécifiques à des groupes de comptes. 
	• Exemple : 
		• Racine (Root) 
			• Production (OU) : Compte A (App 1) et Compte B (App 2) 
			• Développement (OU): Compte C et Compte D 

3- **Politiques de Contrôle des Services (Service Control Policies, SCP)** : Règles qui définissent les actions **autorisées** ou **interdites** pour les comptes ou les unités organisationnelles. Ils fonctionnent au-dessus des politiques IAM pour imposer des restrictions supplémentaires.

![[AWS_OU.webp]]

### Use Cases:
**a) Isolation des environnements**
**b) Gestion des coûts**
**c) Renforcement de la sécurité**
**d) Ségrégation des workloads**


