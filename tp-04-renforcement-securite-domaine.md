# TP 3 – Renforcement de la sécurité de votre domaine

Vous allez réaliser dans ce TP l’ensemble des opérations permettant de sécuriser votre domaine Active Directory.
Déploiement de PingCastle

> [!TIP]
> PingCastle est un outil permettant de réaliser des audits de sécurité sur un domaine Active Directory.
L’outil est disponible dans le support de cours. Il est disponible en téléchargement ici : [www.pingcastle.com](https://www.pingcastle.com/download/)

## Utilisation de l’outil d’audit

- Lancez l’exécutable PingCastle.exe
- Dans l’écran suivant, appuyez sur la touche « Entrée »

![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/a088c984-00b1-414e-aefe-b5afd8b26879)

•	Dans cet écran, appuyez sur « Entrée » pour valider le nom de domaine par défaut.
 ![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/d649f88b-346c-440c-a416-2f835b5c3ab8)

•	Un rapport au format html doit avoir été généré dans le répertoire contenant pingCastle.
![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/917518ec-e892-4955-8a6e-4d2940224696)


Ouvrez-le et parcourez-les différents points de sécurité.


## Déploiement des stratégies d’audit

- Générez et attachez une stratégie de sécurité COMPUTERS_DOMAINCONTROLERS_AUDIT au niveau du conteneur des contrôleurs de domaine.
-	Téléchargez le fichier de définition de stratégie d’audit (https://github.com/gerardlemetayerc/activedirectory-course/raw/main/sources/strategies/parametre-strategie-audit.csv).
-	Rendez-vous dans la stratégie COMPUTERS_DOMAINCONTROLERS_AUDIT

 	![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/69f2cece-326d-41ca-8315-2d0c1f456c79)

-	Cliquez droit sur « Stratégie d’audit », puis sur « Importer les paramètres »

 	![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/a1e67858-d90e-4b0e-a324-10d1c2bbec43)

-	Importez le fichier CSV.
  
### Déploiement de la stratégie de désactivation du PrintSpooler

Générez et attachez une stratégie de sécurité COMPUTERS_DISABLESPOOLER au niveau du conteneur des contrôleurs de domaine.

-	Rendez-vous dans Configuration ordinateur \ Préférences \ Paramètre du panneau de configuration \ Services.

 	![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/041e5509-d23c-40e1-a51b-3b29e76a352f)


-	Cliquez droit sur Services, sélectionnez « Nouveau », puis « Service »

 	![image](https://github.com/gerardlemetayerc/activedirectory-course/assets/33660847/017b5a43-7bce-4730-82c7-75d1ca453ee6)

-	Configurez le service de la manière suivante :
    -	**Type de démarrage** : Désactivé
    -	**Nom du service** : Spooler
    -	**Action du service** : Arrêter le service
 
## Désactivation du SMBv1

- Vérifiez l'état de SMBv1 sur votre contrôleur de domaine

```powershell
Get-WindowsFeature -Name *SMB*
```
Le résultat suivant devrait apparaître

```powershell
Display Name                                            Name
------------                                            ----
[ ] SMB Bandwidth Limit                                 FS-SMBBW
[X] Support de partage de fichiers SMB 1.0/CIFS         FS-SMB1
```

- Exécutez le code suivant :
```powershell
Remove-WindowsFeature -Name FS-SMB1 -Remove
```

Un redémarrage du serveur est néccessaire.
