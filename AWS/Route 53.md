![[f0a1bb2c-a1bc-40ce-abde-6fb9d2a66ce8_1600x570.webp]]

# Types des serveurs DNS: 

1. **Serveurs DNS récursifs** : Résolution des requêtes DNS Un serveur DNS récursif est le point de départ pour résoudre une requête DNS. Il agit comme un intermédiaire entre le client (navigateur ou autre application) et les serveurs DNS autoritaires. Si le serveur récursif ne trouve pas l'adresse IP demandée dans son cache, il effectue une série de requêtes pour trouver la réponse. Exemples de serveurs récursifs populaires : Google Public DNS (8.8.8.8, 8.8.4.4), Cloudflare DNS (1.1.1.1). 
2. **Serveurs DNS racine (Root Servers)** : Point d'entrée de la hiérarchie DNS Les serveurs DNS racines sont au sommet de la hiérarchie DNS et servent de point de départ pour la résolution des noms de domaine. Ils ne stockent pas les enregistrements DNS complets, mais dirigent les requêtes vers les serveurs de noms des domaines de premier niveau (TLD). Exemple : Une requête pour www.example.com commence par un serveur racine qui renvoie une référence au serveur TLD .com. Types de DNS Servers 
3. **Serveurs TLD (Top-Level Domain Servers)** : Gestion des domaines de premier niveau Les serveurs TLD sont responsables de la gestion des domaines appartenant à un domaine de premier niveau spécifique. Ils stockent des informations sur les domaines enregistrés sous un TLD particulier et pointent vers les serveurs autoritaires responsables de ces domaines. Exemples de TLD : Génériques : .com, .org, .net, Nationaux : .fr, .uk, .ca, Sponsors : .gov, .edu 
4. **Serveurs DNS autoritaires** : Gestion des enregistrements DNS d'un domaine Les serveurs DNS autoritaires stockent et fournissent les enregistrements DNS pour des domaines spécifiques. Ils répondent directement aux requêtes DNS pour les domaines dont ils sont responsables, sans interroger d'autres serveurs. Exemples : Serveurs DNS fournis par AWS Route 53 ou d'autres hébergeurs.

![[Pasted image 20250121174524.png]]

![[Pasted image 20250121174544.png]]
# DNS Records

Amazon **Route 53** est un service DNS (Domain Name System) scalable, hautement disponible et entièrement géré par AWS. 
**Fonctionnalités principales:** 
	• **Gestion des DNS** : Permet de gérer les enregistrements DNS pour vos domaines. 
	• **Enregistrements DNS** : Route 53 permet de créer des enregistrements comme A, AAAA, CNAME, MX, TXT, etc. 
	• **Vérification de la santé des ressources** : Assure que vos ressources (serveurs, sites web) sont accessibles et fonctionnent correctement. Route 53 peut rediriger le trafic en fonction de l'état de santé des ressources.

• **A (Address Record)** : Associe un nom de domaine à une adresse IPv4. 
• **AAAA (IPv6 Address Record)** : Associe un nom de domaine à une adresse IPv6. 
• **CNAME (Canonical Name Record)** : Redirige un nom de domaine vers un autre nom de domaine. 
• **MX (Mail Exchange Record)** : Définit les serveurs de messagerie associés à un domaine. 
• **NS (Name Server Record)** : Spécifie les serveurs DNS autoritaires pour un domaine. 
• **PTR (Pointer Record)** : Utilisé pour la résolution inverse des adresses IP. 
• **TXT (Text Record)** : Contient des informations textuelles, souvent utilisées pour la vérification de domaine ou la configuration SPF pour les e-mails. 
• **SRV (Service Record)** : Utilisé pour spécifier des services dans le domaine, par exemple pour SIP ou LDAP.

# DNS Alias Records

Un **Alias record** est un type spécial d'enregistrement DNS dans **Amazon Route 53** qui permet de mapper un nom de domaine à une ressource AWS, comme une adresse IP d'un **Elastic Load Balancer (ELB)**, une distribution **CloudFront**, un **Bucket S3** ou d'autres ressources AWS, sans nécessiter une adresse IP explicite. 

**Caractéristiques** : 
	• **Pas d'adresse IP nécessaire** : Résolution automatique vers les ressources AWS sans spécifier d’IP. 
	• **Peut pointer vers des ressources AWS** : Comme ELB, CloudFront, S3, VPC Endpoint, etc. 
	• **Aucun coût pour les requêtes DNS** : Plus économique que les enregistrements CNAME. 
	• **Utilisable pour les domaines racine** : Contrairement à CNAME, il peut être utilisé pour des domaines comme example.com. 
	• **Propagation rapide** : Mise à jour instantanée des enregistrements DNS, idéale pour les changements AWS.

![[Pasted image 20250121175559.png]]

# Hosted Zones

Hosted Zone est un conteneur pour les enregistrements DNS qui définissent la manière dont le trafic doit être routé vers un domaine et ses sous-domaines. Il existe deux types principaux de zones hébergées : les zones publiques et les zones privées. 

**1- Zone Publique** : contient des enregistrements DNS qui spécifient comment diriger le trafic sur Internet vers un domaine public. Ces zones sont utilisées pour les noms de domaine accessibles au grand public sur Internet

**2- Zone Privée** : contient des enregistrements DNS qui définissent la manière de diriger le trafic au sein de **une ou plusieurs VPCs (Virtual Private Cloud)**. Ces zones sont utilisées pour gérer des noms de domaine privés, qui ne sont accessibles qu'à partir des ressources dans les VPCs (environnement interne).


