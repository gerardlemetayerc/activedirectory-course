# TP 4 – Cluster Active Directory et manipulation des rôles FSMO

Vous allez, dans ce TP, ajouter un contrôleur de domaine supplémentaire, déplacer quelques rôles de votre contrôleur de domaine initial, puis arrêter votre nouveau contrôleur de domaine.
Vous réaliserez ensuite un transfert forcé des rôles vers le contrôleur actif. Puis vous redémarrerez votre contrôleur de domaine arrêté.

## Installation d’un contrôleur de domaine supplémentaire

- Installez un serveur Windows supplémentaire, et intégrez-le au domaine.
- Lancez PowerShell en tant qu’administrateur.
- Puis, exécutez les lignes de commandes suivantes :

```powershell
Get-WindowsFeature *AD-Domain-Services* | Install-WindowsFeature -IncludeManagementTools
Install-ADDSDomainController -DomainName votredomain.local -installDns
```

- Redémarrez votre nouveau contrôleur de domaine.

## Vérification du serveur portant les rôles FSMO actuels et transfert

- Lancez PowerShell en tant qu’administrateur sur votre nouveau serveur et assurez-vous que votre compte soit membre du groupe « administrateur du schéma ». Dans le cas contraire, ajoutez l’utilisateur et redémarrez la session.
- Exécutez la ligne de commande suivante :

```powershell
netdom query fsmo
```
Vous devriez avoir la liste des rôles FSMO, associé au nom du serveur portant actuellement le rôle.

- Transférez à présent vos rôles vers votre nouveau serveur. Exécutez les lignes de commandes suivantes :
```powershell
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole SchemaMaster -Identity $(hostname)
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole DomainNamingMaster -Identity $(hostname)
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole RIDMaster -Identity $(hostname)
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole PDCEmulator -Identity $(hostname)
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole InfrastructureMaster -Identity $(hostname)
```

- A la question « Voulez-vous déplacer le rôle … », répondez oui.
- Vérifiez que vos rôles FSMO soient bien actif sur votre nouveau serveur.
```powershell
netdom query fsmo
```
> [!IMPORTANT]  
> Le nom de votre serveur doit apparaître pour chacun des rôles.
 
- A présent, arrêtez le serveur portant les rôles.

## Transfert forcé de rôles FSMO

> [!WARNING]  
> Situation inconfortable : vous avez perdu tous vos rôles FSMO.

- Générez un nouveau compte utilisateur en exécutant la ligne de commande suivante via PowerShell:
```powershell
New-ADUSer -SamAccountName utest12 -AccountPassword (ConvertTo-SecureString -AsPlainText "SuperM0tDeP@sse2022!" -Force) -DisplayName "User Test" -Name "User Test" -Enabled:$true
```

- Votre nouveau compte doit normalement apparaître dans la liste des utilisateurs du domaine.
- On constate qu’il est toujours possible de générer de nouveaux comptes. Depuis PowerShell, en tant qu’administrateur, lancez les lignes de commandes suivantes :
```powershell
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole PDCEmulator -Identity $(hostname) -Force -Confirm:$false
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole DomainNamingMaster -Identity $(hostname) -Force -Confirm:$false
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole InfrastructureMaster -Identity $(hostname) -Force -Confirm:$false
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole RIDMaster -Identity $(hostname) -Force -Confirm:$false
Move-ADDirectoryServerOperationMasterRole -OperationMasterRole SchemaMaster -Identity $(hostname) -Force -Confirm:$false
```
- Confirmez le transfert des rôles FSMO.
```powershell
netdom query fsmo
```

> [!IMPORTANT]  
> Vous devriez voir votre serveur en tant que maître sur l’ensemble des rôles.

- Démarrez votre serveur secondaire (anciennement porteur des zones). Vérifiez qui porte les rôles FSMO : il devrait avoir automatiquement mis à jour sa base d’information et transféré les rôles à son partenaire.
Conclusion : le transfert des rôles FSMO est une opération simple, en l’état qui est réservée aux administrateurs du domaines et du schéma (pour le rôle de maître de schéma). Il est possible de forcer le transfert d’un rôle, les systèmes (recommandé version minimale 2016) supportent plutôt bien le « seize » (action de forcer le transfert de rôle). Forcer un rôle ne doit être réalisé QUE si nécessaire.

