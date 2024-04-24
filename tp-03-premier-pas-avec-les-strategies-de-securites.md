# TP 3 – Premier pas avec les stratégies de sécurités

## Configuration de PSO (Password Policy Container)

### Initialisation d’un PSO pour les utilisateurs du domaine

Vous allez réaliser dans cette partie le déploiement d’un PSO qui va définir une politique de mot de passe pour l’ensemble des comptes du domaine.

-	Accédez à la console **Centre d’Administration Active Directory**
  -	A l’aide du menu de navigation sur la gauche, cliquez sur la flèche, à droit du nom de votre domaine, puis sélectionnez « System », et enfin sélectionnez « Password Policy Container »
  -	Générez une politique de mot de passe appliquée à tous les utilisateurs du domaine, de priorité 2, selon la configuration suivante :
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

-	Accédez à la console « Centre d’Administration Active Directory »
-	A l’aide du menu de navigation sur la gauche, cliquez sur la flèche, à droit du nom de votre domaine, puis sélectionnez « System », et enfin sélectionnez « Password Policy Container »
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
