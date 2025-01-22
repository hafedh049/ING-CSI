# Scalabilité

> [!tip]
> - **Types de Scalabilité** : 
> 	• **Scalabilité verticale (Scaling UP/DOWN)** : Augmenter les ressources d'un serveur unique, comme ajouter plus de CPU ou de RAM. 
> 	• **Scalabilité horizontale (Scaling OUT/IN)** : Ajouter plus de serveurs ou d'instances pour répondre à la demande, permettant une meilleure résilience et distribution de la charge

# Haute Disponibilité (High Availibility)

> [!warning]
> • Capacité d'un système à rester opérationnel et accessible, même en cas de défaillance. 
> • **Conception** : Une architecture haute disponibilité est conçue pour minimiser les points de défaillance et distribuer les ressources entre plusieurs zones de disponibilité (AZs) et régions AWS

# Load Balancing

> [!note]
> • Le load balancing (ou équilibrage de charge) est un procédé qui consiste à répartir le trafic entrant vers des ressources multiples afin d’optimiser l’utilisation des serveurs, d’assurer la haute disponibilité et de réduire les temps de réponse.
> - Dans un environnement cloud, le load balancing est essentiel pour : 
> 	• Prévenir les surcharges : En redirigeant le trafic vers les ressources disponibles. 
> 	• Optimiser la performance : En maintenant des temps de réponse rapides et une expérience utilisateur cohérente. 
> 	• Garantir la haute disponibilité : En redirigeant le trafic vers les instances saines en cas de panne.

# ELB (Elastic Load Balancing)

Il existe quatre types de Elastic Load Balancer (ELB) sur AWS : 
1. **Classic Load Balancer (CLB)**: le plus ancien des quatre, offrant un équilibrage de charge basique aux couches 4 et 7. 
2. **Application Load Balancer (ALB)**: un load balancer de couche 7 qui distribue les connexions en fonction du contenu de la requête. 
3. **Network Load Balancer (NLB)**: un load balancer de couche 4 qui distribue les connexions en fonction des données du protocole IP. 
4. **Gateway Load Balancer (GLB)**: un load balancer de couche 3/4 utilisé en amont d'appareils virtuels tels que les pare-feu et les systèmes de détection/prévention d'intrusion (IDS/IPS).

## CLB

> [!note]
> **Niveau** : Peut fonctionner au niveau 4 (TCP) ou niveau 7 (HTTP/HTTPS), mais avec moins de fonctionnalités avancées comparé aux ALB et NLB.

> [!tip]
> **Fonctionnalités principales** : 
> • Bascule simple du trafic : Répartit simplement le trafic sur les instances EC2 et assure un certain niveau de résilience. 
> • Fonctionnalités limitées : Ne supporte pas le routage avancé ou les protocoles récents comme HTTP/2 ou WebSocket. 
> • Health Checks basiques : Offre des vérifications de santé simples pour surveiller l'état des instances.

## ALB

> [!note]
> **Niveau** : Fonctionne à la couche 7 (application) du modèle OSI, adaptée aux connexions HTTP/HTTPS.

> [!important]
> **Éléments** : 
> 1. **Listeners** : Écoute le trafic entrant sur des ports/protocoles spécifiques (généralement HTTP sur 80 ou HTTPS sur 443). 
> 2. **Target Groups** : Groupes de cibles vers lesquels le load balancer redirige le trafic. Ces groupes peuvent inclure des instances EC2, conteneurs ECS, adresses IP, ou fonctions Lambda. 
> 	**Types de cibles** : 
> 		- **Instances EC2** : Gérées par des Auto Scaling Groups pour un ajustement automatique. 
> 		- **Tâches ECS** : Redirection vers des conteneurs Amazon ECS. 
> 		- **Fonctions Lambda** : Convertit les requêtes HTTP en événements JSON pour Lambda. 
> 		- **Adresses IP** : Redirige vers des IP privées.
> 3. **Règles de Routage**: L’ALB permet un routage basé sur des règles avancées, adaptées pour des architectures complexes et des microservices. On a trois types de règles de routage: 
> 	- **Routage par chemin** : Par exemple, envoyer **/users** vers un groupe de cibles spécifique et /orders vers un autre. 
> 	- **Routage par nom de domaine** : Permet d’envoyer les requêtes pour **app.example.com** vers un groupe de cibles et **api.example.com** vers un autre. 
> 	- **Routage par chaîne de requête ou en-têtes HTTP** : Par exemple, des requêtes avec (example.com/users?id=123&order=false) peuvent être envoyées vers un groupe de cibles spécifique à la région. 
> 4. **Load Balancer Security Groups**: appliqués au load balancer permettent de restreindre le trafic entrant. 
> 5. **X-Forwarded Headers**: sont des en-têtes HTTP ajoutés par les load balancers pour transmettre aux serveurs d’application des informations sur l'origine des requêtes, comme l’adresse IP, le port, et le protocole (HTTP/HTTPS) du client initial. 
> 	- X-Forwarded-For : Adresse IP du client d'origine, exemple: X-Forwarded-For: 203.0.113.1, 203.0.113.2 
> 	- X-Forwarded-Proto : Protocole utilisé (HTTP ou HTTPS), exemple: X-Forwarded-Proto: https 
> 	- X-Forwarded-Port : Port utilisé pour la connexion, exemple: X-Forwarded-Port: 443

## NLB

> [!note]
> • **Niveau** : Fonctionne au niveau 4 (couche transport) du modèle OSI, gérant les protocoles TCP et UDP.

> [!tip]
> **Fonctionnalités principales** : 
> • **Très haute performance** : Peut gérer des millions de requêtes par seconde avec une latence très faible. 
> • **Adresse IP statique** : Peut associer des adresses IP élastiques, ce qui permet d'avoir des adresses IP fixes pour le load balancer. 
> • **Failover** : En cas de panne d'une zone de disponibilité, le NLB bascule le trafic automatiquement vers d'autres zones disponibles. 
> • **Health Checks au niveau de la cible** : Vérifie l’état de santé des cibles et assure que le trafic est dirigé uniquement vers celles qui sont en bon état.

> [!warning]
> 1- **Listener** : écoutent le trafic entrant sur un port spécifique (comme 80 pour HTTP ou 443 pour HTTPS/TLS). NLB supportent les protocoles de niveau 4, principalement TCP, TLS, et UDP. Cela permet de traiter des connexions réseau à faible latence sans analyse applicative. 
> 2- **Target Groups**: • Instances EC2 : Le NLB peut rediriger le trafic vers des instances EC2, souvent en lien avec des groupes d'Auto Scaling pour répondre à la demande. • Adresses IP : Les target groups peuvent aussi contenir des adresses IP, qui doivent être des IP privées dans le VPC. • ALB : La cible est un Application Load Balancer (ALB). 
> 3- **Elastic IPs** : Le NLB peut associer une IP fixe (Elastic IP) à chaque AZ pour les besoins de sécurité. 
> 4- **Cross-Zone Load Balancing** : Par défaut, le NLB ne répartit pas le trafic entre AZs, mais le Cross-Zone Load Balancing peut être activé pour une répartition uniforme du trafic.

# Cross Zone Load Balancing

Cette fonctionnalité répartit le trafic entrant de manière uniforme entre les instances situées dans différentes zones de disponibilité (AZ) au sein de la même région AWS. 
• **Distribution du Trafic** : Par défaut, si Cross-Zone Load Balancing est activé, le load balancer distribue le trafic de manière égale entre toutes les AZ enregistrées, assurant une utilisation équilibrée des ressources. Sans cette option, le trafic tend à être orienté vers les instances de la même AZ que le client. 

• **Load Balancer par Type** : 
	• **Application Load Balancer** : Activé par défaut (désactivable au niveau du Target Group), sans frais de transfert inter-AZ. 
	• **Network Load Balancer & Gateway Load Balancer** : Désactivé par défaut, des frais de transfert inter-AZ s’appliquent si activé. 
	• **Classic Load Balancer** : Désactivé par défaut, sans frais de transfert inter-AZ si activé.

# Connection Draining

• Une fonctionnalité des Elastic Load Balancers (ELB) d'AWS qui permet aux requêtes en cours d’être terminées avant que les instances ne soient retirées du service. 
• Lorsqu’une instance est marquée comme non saine ou doit être retirée d’un groupe Auto Scaling, Connection Draining gère les connexions existantes de manière à éviter les interruptions. 
• Nom des fonctionnalités: 
	• **Connection Draining** : pour CLB 
	• **Deregistration Delay** : pour ALB (Application Load Balancer) et NLB (Network Load Balancer)
	 
**Avantages** **(+)**: 
	• **Meilleure Expérience Utilisateur** : Les sessions ne sont pas coupées brutalement, ce qui évite des erreurs pour les utilisateurs (par exemple, en plein traitement de commande). 
	• **Disponibilité Continue** : Maintient une haute disponibilité de l’application en évitant les coupures brusques. 
	• **Transitions Fluides** : Facilite les mises à jour et la maintenance, car il permet de retirer progressivement les instances sans perturber l’expérience utilisateur.

# Health Check

• Les health checks dans un ELB (Elastic Load Balancer) vérifient l’état des instances ou cibles dans un Target Group pour s’assurer qu’elles sont aptes à recevoir du trafic. 
• Configuration des Health Checks pour un ELB : 
	• **URL et Port** : Le Load Balancer effectue les health checks sur un port et une URL spécifiques (ex. : /health) définis lors de la configuration. 
	• **Critères de Réponse** : La réponse attendue est généralement une réponse HTTP 200 (OK), indiquant que l’instance est prête. 
	• **Fréquence et Seuils** : La fréquence des checks est configurable, et un nombre défini de checks réussis ou échoués indique si l'instance est prête ou défaillante. 
	• **Résultats des Health Checks** : 
		• **Instance en Bon État** : Continue de recevoir du trafic. 
		• **Instance Défaillante** : Retirée du pool jusqu'à ce qu'elle repasse les checks avec succès.

---

# Auto Scaling Group

3. Minimum / Maximum / Desired Capacity : 
	1. • **Min Capacity** : Nombre minimal d'instances pour garantir la disponibilité. 
	2. • **Max Capacity** : Nombre maximal d'instances pour limiter les coûts et éviter le sur-provisionnement. 
	3. • **Desired Capacity** : Nombre d'instances que l'ASG maintiendra par défaut, sauf si une règle de scaling est déclenchée. 
	4. **Scaling Policies**: AWS propose plusieurs politiques pour gérer les groupes Auto Scaling (ASG) . 
			• **Bonnes métriques pour le scaling** : 
				• **CPUUtilization** : Utilisation moyenne du CPU sur l’ensemble des instances. 
				• **RequestCountPerTarget** : Assure un nombre stable de requêtes par instance EC2. 
				• **Network In / Out Moyen** : Pertinent si votre application dépend de la bande passante réseau. 
				• **Métrique personnalisée** : Toute métrique personnalisée envoyée via CloudWatch.

### Scaling Policies

1. **Manual Scaling** : Ajustement manuel les paramètres de l'ASG en ajoutant ou supprimant des instances. 
2. **Scheduled Scaling** : Scaling basé sur un calendrier, adapté aux charges prévisibles. 
3. **Dynamic Scaling** : Ajuste automatiquement la capacité en fonction de la demande : Utilise des **CloudWatch Alarms** pour surveiller les métriques et déclencher des actions avec des politiques de scaling : 
	1. **Target Tracking Scaling** : Ajuste la capacité en fonction d’une valeur cible pour une métrique. si la cible est une utilisation CPU de 50 %, AWS ajoutera ou supprimera des instances pour maintenir cette utilisation autour de 50 %. 
	2. **Step Scaling** : Ajuste selon la gravité de l'alarme si le CPU dépasse 70 %, une instance supplémentaire est ajoutée, mais si le CPU dépasse 90 %, deux instances sont ajoutées d’un coup. 
	3. **Simple Scaling** : Ajuste la capacité par un seul changement. si l’utilisation CPU dépasse un certain niveau, une instance supplémentaire sera ajoutée. Une fois l’ajustement fait, le système entre dans une période de cooldown (repos), durant laquelle aucun autre scaling ne peut être déclenché. 
4. **Predictive Scaling** : Permet de prévoir les besoins en capacité basés sur des modèles de trafic récurrents, en utilisant l'apprentissage automatique (machine learning) pour anticiper la demande et lancer des instances avant que le besoin n’apparaisse.