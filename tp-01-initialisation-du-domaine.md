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
Enable-ADOptionalFeature 'Recycle Bin Feature' -Scope ForestOrConfigurationSet -Target votredomain.fqdn
```

## Modification de la durée de rétention de la corbeille Active Directory

* Il est possible de modifier la valeur de rétention des objets dans la corbeille Active Directory. Cette option est réalisable soit via l'éditeur d'attribut ADSI, soit en powershell. Le code est le suivant :

```powershell
$FQDN = (Get-ADDOmain).distinguishedName
Get-ADOBjet "CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,$FQDN" | Set-ADOBject -Replace @{'msDS-DeletedObjectLifetime'=360}
```

> :memo: **Note:** Dans ce code, vous avez étendu la durée de rétention en corbeille à 360 jours.

## Création de l'arborescence

Inspirez-vous de l'image suivante afin de générez une arborescence semblable :

![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/61d8b835-305e-4431-9cc4-573895b89f31)

Dans le répertoire suivant : **AD/applications/Groupes**, ajoutez les groupes AD suivants :
* **AD_ADMIN_APPLICATIVEACCOUNTS** : sera dédié à l'administration des comptes applicatifs
* **AD_ADMIN_COMPUTERS** : sera dédié à l'administration des comptes "ordinateurs"
* **AD_ADMIN_GROUPES** : sera dédié à l'administration des groupes
* **AD_ADMIN_USERS** : sera dédié à l'administration des comptes utilisateurs

