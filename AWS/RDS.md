**Amazon RDS (Relational Database Service)** est un service managé d’AWS conçu pour simplifier la configuration, l’utilisation et la gestion des bases de données relationnelles dans le cloud. 

**Bases de données supportées** : 
1. **Amazon Aurora (Compatible avec MySQL et PostgreSQL)**. 
2. **MySQL.** 
3. **PostgreSQL.** 
4. **MariaDB.** 
5. **Oracle Database.** 
6. **Microsoft SQL Server.** 
7. **IBM db2**

> [!tip] Avantages
> • Provisionnement automatisé
> • Sauvegardes continues
> • Read Replicas
> • Tolérance aux pannes

# Storage Auto Scaling

La fonctionnalité Storage Auto Scaling d'Amazon RDS permet d'augmenter automatiquement la capacité de stockage d'une instance de base de données RDS lorsque cela est nécessaire, sans intervention manuelle. 

**Caractéristiques principales** : 
	• Augmentation automatique du stockage
	• Évite les ajustements manuels 
	• Définition d’un seuil de stockage maximal : définir un **Maximum Storage Threshold** (seuil maximal de stockage) pour limiter la taille maximale que la base de données peut atteindre. 
	
**Conditions pour déclencher la mise à l'échelle** : 
	• Stockage libre inférieur à 10 % de la capacité allouée. 
	• La situation de faible stockage doit durer au moins 5 minutes. 
	• Au moins 6 heures doivent s’être écoulées depuis la dernière modification de stockage

# Read Replicas

Les **Read Replicas** d’Amazon RDS permettent de créer des copies en lecture seule d’une base de données RDS. Elles sont utilisées pour améliorer les performances des applications qui ont des charges de lecture élevées.

**Fonctionnalités Clés des Read Replicas**: 
	• **Répliques en lecture seule** : Les Read Replicas sont conçues pour traiter uniquement les requêtes de lecture. 
	• **Réplication asynchrone** : La réplication entre l’instance principale (primary) et la Read Replica est asynchrone. Cela signifie qu’il peut y avoir un léger décalage entre les données de l’instance principale et les Read Replicas. 
	• **Prise en charge multi-région** : Les Read Replicas peuvent être créées dans la même région ou dans d’autres régions AWS. 
	• **Scalabilité horizontale** : Permet de répartir la charge de lecture entre plusieurs répliques pour améliorer les performances globales. Jusqu’à 5 Read Replicas peuvent être créées à partir d’une même instance principale. 
	• **Promotion en tant qu’instance principale** : Une Read Replica peut être promue en instance principale en cas défaillence

> [!important]
> La réplication des données entre AZs dans une même région n’est pas facturée mais entre régions la réplication est facturée.

# Multi-AZ

La fonctionnalité Multi-AZ (Multi-Zone Availability) d'Amazon RDS garantit une haute disponibilité et une tolérance aux pannes pour bases de données. 

**Fonctionnalités RDS Mutli AZ** : 
1. **Répliques synchrones** : Lors de l’activation Multi-AZ pour une instance RDS, AWS crée automatiquement une copie synchrone de cette base de données principale dans une autre zone de disponibilité (AZ) de la même région. La base principale et sa copie sont constamment synchronisées. 
2. **Basculement automatique (Failover)** : En cas de défaillance de l’instance principale ou de l’AZ où elle se trouve, RDS effectue un basculement automatique vers l’instance de secours (standby) dans une autre AZ. Ce basculement est transparent pour l'application, qui continue d'utiliser le même endpoint (adresse de connexion). 
3. **Maintenance planifiée** : Les mises à jour de l’infrastructure RDS (comme les correctifs logiciels) sont appliquées d’abord à l’instance de secours, puis à l’instance principale, minimisant les interruptions.

**Limitations de Multi-AZ** : 
	• **Coût élevé** : payer pour deux instances RDS (principale et standby), même si l'instance standby est inactive la plupart du temps. 
	• **Pas pour la scalabilité** : Multi-AZ ne permet pas de gérer des charges de lecture élevées ; pour cela, il faut utiliser des **Read Replicas**. 
	• **Temps de basculement** : Bien que rapide, le basculement peut prendre 1 à 2 minutes, ce qui peut entraîner une courte interruption pour les applications sensibles

# Processus de Passage de Single-AZ à Multi-AZ dans RDS

1. **Création d’un snapshot** : AWS prend un instantané de la base de données existante. 
2. **Restauration dans une nouvelle AZ** : Une nouvelle instance de base de données est créée à partir du snapshot dans une autre zone de disponibilité (AZ) de la même région. 
3. **Mise en place de la synchronisation** : Une réplication synchrone est établie entre la base principale et l’instance standby pour garantir que les deux sont toujours à jour. 
4. **Mise à jour de la configuration** : Une fois la synchronisation terminée, la base de données est configurée en Multi-AZ, sans interruption de service et sans modification nécessaire côté application.

# RDS Custom

**RDS Custom** est une fonctionnalité d'Amazon RDS qui permet de gérer des bases de données Oracle et Microsoft SQL Server tout en offrant une personnalisation plus poussée du système d'exploitation (OS) et de la base de données. 

**Fonctionnalités de RDS Custom** : 
	• **Base de données gérée** : Comme RDS standard, RDS Custom automatise la configuration, l'exploitation et l'évolutivité des bases de données dans AWS, mais avec des options supplémentaires de personnalisation. 
	• **Accès à la base de données et à l'OS** : Un accès complet à la base de données et à l'OS sous-jacent, qui permet de configurer des paramètres spécifiques et d’installer des patchs ou des mises à jour système. Accéder à l'instance EC2 sous-jacente via SSH ou SSM Session Manager pour effectuer des tâches administratives avancées. 
	• **Mode d’automatisation** : Pour effectuer des personnalisations spécifiques, il faut désactiver temporairement le mode d’automatisation d'AWS, mais il est recommandé de prendre un snapshot de la base de données avant de faire des modifications importantes.

# Backup

1- **Backups automatiques** : 
	• **Sauvegarde complète quotidienne** : Chaque jour, une sauvegarde complète de la base de données est effectuée pendant la fenêtre de maintenance. 
	• **Sauvegarde des journaux de transaction** : RDS effectue des sauvegardes des journaux de transaction toutes les 5 minutes, permettant ainsi une restauration à n’importe quel moment, de la sauvegarde la plus ancienne à 5 minutes avant. 
	• **Conservation des backups** : La rétention des backups peut être configurée entre 1 et 35 jours. Si vous définissez la rétention à 0, les backups automatiques sont désactivées. 
	
2- **Snapshots manuels** : Les snapshots sont déclenchés manuellement par l’utilisateur à tout moment. Les snapshots sont conservés aussi longtemps que vous le souhaitez. Astuce : Lorsque vous arrêtez une base de données RDS, vous continuez à payer pour l’espace de stockage. Si vous prévoyez de l’arrêter longtemps, il est préférable de prendre un snapshot et de restaurer la base de données lorsque vous en avez besoin.

# Securité

1- **Chiffrement au Repos (At-Rest Encryption)** : 
	• Les données des bases de données principales et des réplicas sont chiffrées à l'aide d'AWS KMS. 
	• Le chiffrement doit être défini lors de la création de la base de données. 
	
2- **Chiffrement en Transit (In-Flight Encryption)** : 
	• Le chiffrement des données en transit est activé par défaut via TLS. 
	• Pour garantir une connexion sécurisée depuis le client avec les certificats racines TLS fournis par AWS. 
	
3- **Authentification IAM** : 
		• Des rôles IAM pour connecter à la base de données qui permet de remplacer l'utilisation des identifiants traditionnels (nom d'utilisateur et mot de passe) par une authentification IAM plus sécurisée. 
		
4- **Groupes de Sécurité (Security Groups)** : 
	• Contrôlent l'accès réseau à les instances RDS. 
	
5- **Accès SSH** : Pas d'accès SSH pour les bases de données RDS classiques. Seules les instances RDS Custom permettent un accès SSH pour personnaliser le système.