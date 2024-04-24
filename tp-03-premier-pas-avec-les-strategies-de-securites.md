# TP 3 – Premier pas avec les stratégies de sécurités

## Configuration de PSO (Password Policy Container)

### Initialisation d’un PSO pour les utilisateurs du domaine

Vous allez réaliser dans cette partie le déploiement d’un PSO qui va définir une politique de mot de passe pour l’ensemble des comptes du domaine.

-	Accédez à la console **Centre d’Administration Active Directory**
  -	A l’aide du menu de navigation sur la gauche, cliquez sur la flèche, à droit du nom de votre domaine, puis sélectionnez **System**, et enfin sélectionnez **Password Policy Container**
  -	Générez une politique de mot de passe appliquée à tous les utilisateurs du domaine, de priorité **2**, selon la configuration suivante :
    *	**Nom** : Politique_Globale
    *	**Priorité** : 2
    *	**Applique la longue minimale du mot de passe** : 12
    *	**Appliquer l’historique des mots de passe** : 24
    *	**Le mot de passe doit respecter des exigences de complexité**
    *	**Age minimum du mot de passe** : 1
    *	**Age maximum du mot de passe** : 90
    *	**Appliquer la stratégie de verrouillage des comptes** :
        *	**Nombre de tentatives de connexion échouées** : 5
        *	**Réinitialiser le compteur de connexions échouées au bout de** : 15
    * **Le compte va être verrouillé pendant une durée de** : 20
 
 
### Initialisation d’un PSO pour les comptes administrateurs du domaine

Vous allez réaliser dans cette partie le déploiement d’un PSO qui va définir une politique de mot de passe pour l’ensemble des administrateurs du domaine.

-	Accédez à la console **Centre d’Administration Active Directory**
-	A l’aide du menu de navigation sur la gauche, cliquez sur la flèche, à droit du nom de votre domaine, puis sélectionnez **System**, et enfin sélectionnez **Password Policy Container**
-	Générez une politique de mot de passe appliquée à tous les utilisateurs du domaine, de priorité 2, selon la configuration suivante :
    *	**Nom** : Politique_Super_Administrateur
    *	**Priorité** : 1
    *	**Applique la longue minimale du mot de passe** : 15
    *	**Le mot de passe doit respecter des exigences de complexité**
    *	**Age minimum du mot de passe** : 1
    *	**Age maximum du mot de passe** : 42
    *	**Appliquer la stratégie de verrouillage des comptes** :
        * **Nombre de tentatives de connexion échouées** : 5
        * **Réinitialiser le compteur de connexions échouées au bout de** : 30
    * **Le compte va être verrouillé pendant une durée de** : 30
  
Ici, la configuration des historiques de mots de passe n’a pas été réalisé. Par conséquent, la politique qui s’appliquera sur ce paramètre sera celle appliquée de priorité 2, soit un historique de 24 mots de passes.



## Déploiement de modèles de stratégie

Vous allez déployer dans cette partie deux modèles de stratégies de groupe : Windows 10 et Google Chrome.

**Les sources sont disponibles ici :**
- [Administrative Templates (.admx) for Windows 10 October 2022 Update.msi](https://github.com/gerardlemetayerc/activedirectory-course/blob/d2bf46af7b63dafad53421fc6b977b68688af48c/sources/admx/Administrative%20Templates%20(.admx)%20for%20Windows%2010%20October%202022%20Update.msi)
- [chrome_configuration_admx.zip](https://github.com/gerardlemetayerc/activedirectory-course/blob/c4cae65334ae9e3b48ac46564c0d851d157353d5/sources/admx/chrome_configuration_admx.zip)

### Déploiement des ADMX

- Copiez le répertoire **C:\Windows\PolicyDefinitions** dans **C:\Windows\SYSVOL\sysvol\votreDomaine\Policies**.

#### Déploiement des modèles de stratégies Windows 10

- Exécutez le programme **Administrative Templates (.admx) for Windows 10 [...]** et installez-le.
- Rendez-vous dans le répertoire de déploiement de l’application : **C:\Program Files (x86)\Microsoft Group Policy\Windows 10 [...]**.
- Copiez le répertoire PolicyDefinitions dans **C:\Windows\SYSVOL\sysvol\votreDomaine\Policies**.

#### Déploiement des modèles de stratégie Google Chrome

- Dézippez le contenu de votre répertoire de **chrome_configuration_admx.zip**. Dans ce répertoire, dézippez le répertoire **policy_templates.zip**.
- Copiez le contenu de windows\admx dans le répertoire **C:\Windows\SYSVOL\sysvol\votreDomaine\Policies\PolicyDefinitions**

#### Vérification de la disponibilité des modèles de stratégies
- Démarrez la console de gestion des stratégies de groupes. Générez une nouvelle stratégie de groupe nommée **COMPUTERS_WINDOWS10**.
 
### Déploiement de LAPS et de sa stratégie de sécurité

- Installation de LAPS sur votre contrôleur de domaines
> Prérequis : générez un compte administrateur du domaine supplémentaire.
Déployez l’application LAPS sur votre contrôleur de domaine, en sélectionnant toutes les options disponibles.
 

Une fois LAPS installé, démarrez PowerShell et exécutez la ligne de commande suivante (à réaliser avec un compte administrateur du schéma) :

```powershell
Update-AdmPwdADSchema
```
### Déploiement de la stratégie

- Générez une stratégie **COMPUTERS_LAPS**, et liez-la à la racine du domaine.
- Editez là et rendez-vous dans la partie LAPS.
 

Configurez les paramètres suivants :
-	Complexité du mot de passe
 
-	Le refus de permettre un mot de passe plus vieux que la stratégie de mot de passe locale
-	L’activation du paramétrage
 

