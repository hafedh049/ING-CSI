> [!important]
> • Un service AWS global qui n'est pas limité par les régions. Tout utilisateur, groupe, rôle ou politique est accessible à l'échelle mondiale. 
> • Il permet de gérer les accès et les permissions pour les ressources AWS. 
> • Les nouveaux utilisateurs n'ont aucune autorisation lors de la création de leur compte.

> [!success]
> • **Users**: tout utilisateur final individuel tel qu'un employé, un architecte de système, un directeur technique, etc. 
> • **Groups**: tout ensemble de personnes similaires disposant d'autorisations partagées, telles que les administrateurs système, les employés des ressources humaines, les équipes financières, etc. Chaque utilisateur appartenant à un groupe donné hérite des autorisations définies pour le groupe. 
> • **Roles**: est une identité AWS avec des autorisations, similaire à un utilisateur. Contrairement aux utilisateurs, les rôles n'ont pas d'informations d'identification permanentes. Ils peuvent être assumés par des humains, des instances EC2, du code, ou des services AWS. Lorsqu'assumé, le rôle fournit des identifiants temporaires pour une session, offrant un accès sécurisé. 
> • **IAM policies**: ensembles de règles documentées qui sont appliquées pour accorder ou limiter l'accès. Pour que les utilisateurs, les groupes ou les rôles puissent définir correctement les autorisations, ils utilisent des règles. Les règles sont écrites en JSON.

### Niveaux de priorité pour la gestion des autorisations

• **Dénégation explicite (Explicit Deny)** : Refuse l'accès à une ressource spécifique, et cette décision ne peut pas être contournée. Si une politique inclut un refus explicite, l'accès est bloqué, peu importe d'autres autorisations. 

• **Autorisation explicite (Explicit Allow)** : Autorise l'accès à une ressource donnée, à condition qu'il n'y ait pas de refus explicite associé. Si aucune dénégation explicite n'est présente, cette autorisation permet l'accès. 

• **Dénégation par défaut (Default Deny)** : Par défaut, toutes les identités IAM n'ont accès à aucune ressource. L'accès doit être explicitement accordé via une politique, sinon il est implicitement refusé.

### Best Practices

• **Least Privilege Principle**: N'accorder aux utilisateurs que les autorisations nécessaires à l'accomplissement de leurs tâches. 

• **Use Roles Instead of Long-Term Access Keys**: Éviter d'intégrer des clés d'accès à long terme dans les applications. Utilisez les rôles IAM pour un accès sécurisé et temporaire. 

• **Enable MFA**: renforcer la sécurité en exigeant le MFA pour les utilisateurs ayant des privilèges de haut niveau. 

• **Regularly Review Permissions**: Auditer et ajuster périodiquement les autorisations pour s'assurer qu'elles sont toujours appropriées.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::example-bucket/*"
        }
    ]
}
```