> [!note]
> **EC2 = Elastic Compute Cloud = Infrastructure as a Service**
> Un service d'Amazon Web Services (AWS) qui fournit des machines virtuelles (ou instances)

# Amazon Machine Images (AMIs)

**L'Amazon Machine Image (AMI)** définit le logiciel initial qui sera présent sur une instance lorsqu'elle est lancée. Une AMI détermine chaque aspect de l'état logiciel au lancement de l'instance : 
	**• Le système d'exploitation (OS) et sa configuration.** 
	**• L'état initial de tous les correctifs.** 
	**• Les logiciels applicatifs ou systèmes.** 

**Il existe quatre sources d'AMIs :** 
	**• Publiées par AWS.** 
	**• Le AWS Marketplace (magasin en ligne pour les AMIs).** 
	**• Générées à partir d'instances existantes.** 
	**• Serveurs virtuels téléchargés (en utilisant le service AWS VM Import/Export).** 

> [!tip]
> **Les AMIs sont régionales. Elles ne peuvent être lancées que dans la région où elles sont stockées.**

# EC2 User Data

Exécution des Scripts personnalisés automatiquement lors du lancement ou du redémarrage d'une instance : Des scripts en Bash (Linux) ou en PowerShell (Windows). 
• **Cas d’utilisation courants** : 
	**– Installer des paquets ou mises à jour automatiques.** 
	**– Déployer des applications ou fichiers de configuration.** 
	**– Configurer des services comme SSH, NTP, ou des applications spécifiques.** 
	**– Configurer les logs ou le monitoring dès le lancement**

```shell
#!/bin/bash
yum update -y                      # Update the system (Amazon Linux)
yum install -y httpd                # Install Apache web server
systemctl start httpd                # Start Apache
systemctl enable httpd               # Enable Apache to start at boot
echo "<h1>Welcome to AWS EC2</h1>" > /var/www/html/index.html
```

```powershell
<powershell>
Set-ExecutionPolicy Bypass -Scope Process -Force
Install-WindowsFeature -Name Web-Server -IncludeAllSubFeature
New-Item -Path C:\inetpub\wwwroot\index.html -ItemType File -Value "<h1>Welcome to AWS EC2</h1>"
Restart-Service W3SVC
</powershell>
```

# Elastic IP

> [!warning]
> • Une adresse IP statique et fixe fournie par Amazon Web Services (AWS)

# Security Groups

> [!warning]
> AWS permet de contrôler le trafic entrant et sortant des instances via des pare-feu virtuels appelés groupes de sécurité.

> [!tip]
> **Security Groups** permettent de contrôler le trafic en fonction du **port**, du **protocole** et de la **source/destination**. 
> • Security Groups sont associés aux instances lors de leur lancement. Chaque instance doit avoir au moins un groupe de sécurité, mais peut en avoir plusieurs. 
> • Lorsqu'une instance est associée à plusieurs groupes de sécurité, les règles sont agrégées, et tout le trafic autorisé par chacun des groupes individuels est également autorisé. 
> • Les groupes de sécurité sont appliqués au niveau de l'instance. 
> • Les groupes de sécurité dans AWS ne contiennent que des règles d'autorisation (règles d'autorisation).

> [!example]
> **Inbound Rule** : définit le type de trafic qui est autorisé à entrer dans la ressource (EC2 Instance, RDS Database etc.) en provenance de sources externes. 
> **Outbound Rule**: définit le type de trafic qui est autorisé à sortir de la ressource (EC2 Instance, RDS Database etc.) vers des destinations externes. 
> **• All inbound traffic is blocked by default** 
> **• All outbound traffic is authorised by default**

![[Pasted image 20250121151232.png]]

# EC2 Hibernate

> [!tip]
> L'hibernation EC2 permet de mettre en pause une instance en préservant l'état de la mémoire (RAM), ce qui permet de reprendre l'instance plus rapidement sans redémarrer le système d'exploitation. Lors de l'hibernation, l'état de la RAM est sauvegardé sur le volume EBS racine, qui doit être chiffré. 
> • Une instance ne peut PAS être mise en hibernation plus de 60 jours. 
> • Taille de la RAM de l'instance : doit être inférieure à 150 GB. 
> 
> Cas d'utilisation : 
> 	- Traitements de longue durée 
> 	- Sauvegarde de l'état de la RAM 
> 	- Services nécessitant du temps pour l'initialisation

