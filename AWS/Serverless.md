**Serverless** est un modèle d'exécution proposé par AWS (et d'autres fournisseurs de cloud) qui permet aux développeurs de créer et d'exécuter des applications sans avoir à gérer l'infrastructure des serveurs sous-jacents. Le terme "serverless" ne signifie pas qu'il n'y a pas de serveurs, mais plutôt que leur gestion est entièrement prise en charge par AWS, permettant aux développeurs de se concentrer uniquement sur l'écriture du code. 

- **Principes Clés du Serverless** : 
	• **Pas de Gestion de Serveurs** : Pas besoin de provisionner, configurer ou maintenir des serveurs. AWS gère l'infrastructure (redondance, mise à jour, sécurité). 
	• **Mise à l'Échelle Automatique** : Les applications peuvent s'adapter automatiquement en fonction de la charge. On paie uniquement pour les ressources consommées (facturation au milliseconde ou par requête). 
	• **Haute Disponibilité Intégrée** : Les services serverless sont conçus pour être hautement disponibles et résilients par défaut. 
	• **Basé sur des Événements** : Les applications serverless fonctionnent souvent sur des déclencheurs, comme une requête HTTP, un changement de base de données ou un message dans une file d'attente.


**Les services AWS serverless**: AWS Lambda, DynamoDB, AWS Cognito, AWS API Gateway, Amazon S3, AWS SNS & SQS, AWS Kinesis Data Firehose, Aurora Serverless, Step Functions, Fargate… 

**Avantages du Serverless** : 
	• Réduction des coûts : Paiement que pour le temps de calcul ou les requêtes réellement utilisés. 
	• Simplicité de gestion : Moins de tâches liées à l'infrastructure. 
	• Scalabilité : Applications automatiquement évolutives.