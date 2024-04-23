# TP 1 : Initialisation du domaine

Ce TP a pour objectif de fournir les étapes néccessaire afin d'initialiser votre environnement **Active Directory**.

| Variable         | Description                                          |
| -----------------| ---------------------------------------------------- |
| votredomain.fqdn | Votre nom de domaine. Par exemple : votre-domain.lab |


## Installation d'Active Directory

* Lancez PowerShell en tant qu'administrateur
* Lancez la ligne de commande ci-dessous

```powershell
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

Powershell installera l'ensemble des services pré-requis pour Active Directory.


## Installation du service Active Directory

* Maintenance que les services sont déployés, vous pouvez maintenant initaliser votre domaine dans sa première forêt. Pour ce faire, exécutez le code ci-dessous.

```powershell
Install-ADDSForest -DomainName "votredomain.fqdn" -InstallDNS
```

* Renseignez les différentes informations qui vous seront demandés (nottament le mot de passe de restauration de l'Active Directory).
* Une fois l'installation terminée, le serveur redémarrera.

## Initialisation de la corbeille Active Directory

* Comme présenté dans le cours, Active Directory dispose d'une fonctionnalité de corbeille. Afin de l'activer, procédez à la configuration suivante :

```powershell
Import-Module ActiveDirectory
Enable-ADOptionalFeature 'Recycle Bin Feature' -Scope ForestOrConfigurationSet -Target $(Get-ADDOmain).distinguishedName
```

## Modification de la durée de rétention de la corbeille Active Directory

* Il est possible de modifier la valeur de rétention des objets dans la corbeille Active Directory. Cette option est réalisable soit via l'éditeur d'attribut ADSI, soit en powershell. Le code est le suivant :

```powershell
$FQDN = (Get-ADDOmain).distinguishedName
Get-ADOBject "CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,$FQDN" | Set-ADOBject -Replace @{'msDS-DeletedObjectLifetime'=360}
```

> :memo: **Note:** Dans ce code, vous avez étendu la durée de rétention en corbeille à 360 jours.

## Création de l'arborescence

Inspirez-vous de l'image suivante afin de générez une arborescence semblable :

![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/61d8b835-305e-4431-9cc4-573895b89f31)

Dans le répertoire suivant : **Ecoles/Groupes/applications/AD**, ajoutez les groupes AD suivants (en laissant les paramètres par défaut) :
* **AD_ADMIN_APPLICATIVEACCOUNTS** : sera dédié à l'administration des comptes applicatifs
* **AD_ADMIN_COMPUTERS** : sera dédié à l'administration des comptes "ordinateurs"
* **AD_ADMIN_GROUPES** : sera dédié à l'administration des groupes
* **AD_ADMIN_USERS** : sera dédié à l'administration des comptes utilisateurs

## Délégation des permissions Active Directory

Vous allez désormais réaliser des autorisations par type d'objet dans votre **Active Directory**.

* Réalisez un clic droit sur l'unité d'organisation "compteApplicatif"
![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/d6e736a5-f18f-4dbf-920f-231d632eb185)

* Cliquez sur "Ajouter le groupe", puis recherchez le groupe **AD_ADMIN_APPLICATIVEACCOUNTS**
![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/8ca0e66b-7e02-4e70-9de0-7ccb641650e5)

* Cliquez sur "**Créer une tâche personnalisée à déléguer**" 
![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/e9a96d16-951f-41b2-a65b-db8215bd8117)

* Sélectionnez "Objet utilisateurs", ainsi que "Créer les objets..." et "Supprimer les objets..."
![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/704d8dbb-dc87-49f1-a7a8-5bd2864ab7b3)

* Donnez le contrôle total
 ![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/354a519d-c935-48a9-8e90-9e5d7d621e52)

**Réalisez la même opération pour les autres groupes, en sélectionnant les objets AD appropriés (groupe pour AD_ADMIN_GROUPES, ordinateur pour AD_ADMIN_COMPUTERS...).**
