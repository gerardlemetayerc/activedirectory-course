# TP 2 - Configuration de la sauvegarde

* Dans votre domaine Active Directory, générez un compte svc_backupad
* Ajoutez le dans le compte utilisateur svc_backupad dans les groupes suivantes :
  * **Backup Operator** *(ou Opérateurs de sauvegardes en Français)* (dans **CN=Builtin,DC=votread,DC=local**) 
  * **Protected Users** (dans **CN=Users,DC=votread,DC=local**)
* A l’aide de PowerShell, installez la fonctionnalité **Wbackup (Windows Backup)**

```powershell
Install-WindowsFeature -Name Windows-Server-Backup -IncludeManagementTools
```

* Ajoutez un disque supplémentaire à votre VM (de 20 Go), formattez le disque et mapper le avec la lettre D:

* Vérifiez avec votre compte administrateur l’état de la dernière sauvegarde AD
```powershell
Repadmin /showbackup
```
## Pour tester

* Sur votre poste de travail, configurez à l'aide d'une fenêtre CMD (et non powershell) winrm à l'aide des commandes suivantes :
 ```bash
winrm quickconfig
winrm set winrm/config/client @{TrustedHosts="*"}
```

* Lancez Powershell en tant que compte **svc_backupad**, puis exécutez la commande suivante, avec le compte **svc_backupad** avec une console powershell à privilège:
```powershell
Enter-PSSession -ComputerName <votreip>  -Credential $(Get-Credential <domain>\svc_backupad)
$WBpolicy = New-WBPolicy
Add-WBSystemState -Policy $Wbpolicy
$WBtarget = New-WBBackupTarget -VolumePath 'D:'
Add-WBBackupTarget -Policy $wbpolicy -Target $Wbtarget
Start-WBBackup -Policy $WBpolicy
```
