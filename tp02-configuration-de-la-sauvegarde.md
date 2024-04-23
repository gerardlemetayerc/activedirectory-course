# TP02 - Configuration de la sauvegarde

* Dans votre domaine Active Directory, générez un compte svc_backupad
* Ajoutez le dans le groupe **BUILTIN\Backup Operator** et **Protected Users**
* A l’aide de PowerShell, installez la fonctionnalité **Wbackup (Windows Backup)**

```powershell
Install-WindowsFeature -Name Windows-Server-Backup -IncludeManagementTools
```

* Ajoutez un disque supplémentaire à votre VM (de 20 Go), formattez le disque et mapper le avec la lettre D:

* Vérifiez avec votre compte administrateur l’état de la dernière sauvegarde AD
```powershell
Repadmin /showbackup
```

* Lancez Powershell en tant que compte **svc_backupad**, puis exécutez la commande suivante, avec le compte **svc_backupad** avec une console powershell à privilège:
```powershell
$WBpolicy = New-WBPolicy
Add-WBSystemState -Policy $Wbpolicy
$WBtarget = New-WBBackupTarget -VolumePath "C:« 
Add-WBBackupTarget -Policy $wbpolicy -Target $Wbtarget
Start-WBBackup -Policy $WBpolicy
```
