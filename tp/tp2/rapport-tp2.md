# Rapport du travail pratique 2

- Ce fichier est un modèle pour vous guider dans la rédaction du rapport. 
- Certaines sections sont détaillées pour  donner une idée du rapport attendu. 
- S'assurer de metre toutes les informations demandées dans l'énoncé
- Vous pouvez ajouter des sections ou toute autre information demandée dans l'énoncé
- Donner les extraits des tous les fichiers de configuration modifiés
- Donner toutes les commanes importantes et leurs résultats. Au besoin, donnez une brève explication


## Identification du groupe

- Cours      : Utilisation et administration des systèmes informatiques
- Sigle      : INF1070
- Session    : Hiver 2024
- Enseignant : `Johnny Tsheke` ou `Ryan Kavanagh`
- Auteur     : `<votre nom>` (`<votre code permanent>`)(`<votre code ms>`) (`<votre courriel UQAM>`)


## Identification du système d'exploitation

- Système d'exploitation

~~~csh
# Coller le resultat de la commande "lsb_release -a"
~~~

- Au depart, le contneur comprend déjà les packatages repris dans le fichier dockerfile. Taper la commande: 
  
~~~csh
# Indiquer la commande réellement utilisée. 
# Ici on montre un exemple
apt list --installed >paquets0<codems>.txt
~~~

- Au total, on a `<nombre>` paquetages. J'ai  trouvé ce nombre avec la commande suivante.

~~~csh
# Mettre une commande utilisant "tail" et/ou "wc" avec "paquets0<codems>.txt" comme argument.
# Si on exécute cette commande, on devrait trouver ce même chiffre.
~~~

- Problèmes eventuels
  
> * Ex 1 : Nous n'avons rencontré aucun problème particulier à cette étape
> * Ex 2 : Notre distribution Linux n'utilise pas le gestionnaire des paquetages "apt", alors on a dû chercher et trouver la bonne commande sur le site [https://inf1070.com/exemple](https://inf1070.com/exemple)

  
 

## Apache -- Installation et configuration

- Vérification: Nous avons commencé par vérifier si apache est déjà installé:

~~~csh
apt-cache policy apache2
# On a obtenue le resultat suivant.
# coller le résultat de la commande ici
~~~

- Installation : Nous avons installé (ou mis à jour) apache  avec la commande ci-après.

~~~csh
# Mettre la commande ici
~~~

Mettre les commentaires eventuels s'il y en a

- Selon les fichiers de configuration, le dossier `Documentroot` est: 

- Vérification des permissions. 

~~~csh
# Mettre le résultat de la commande  suivante ici
# stat /var/www/html
~~~

- Lecture et exécution pour tous.

~~~csh
# Si pas lecture et exécution pour tous à l'étape précédente:
# Mettre la commande pour avoir lecture et exécution pour tous
~~~



<!-- ci-apres un exemple de d'insertion d'une image (logo UQAM) -->

>   ![UQAM](uqam.png) 

Ne pas oublier d'enlever le logo UQAM donné ici en exemple.


- Après cette installation,  les paquets du fichier [`paquets1<codems>.txt`](`paquets1<codems>.txt`) sont maitenant installés. On a obtenu cette liste avec la commande: 

~~~csh
# Mettre la commande ici
~~~

- Au total, on a maintenant `<nombre>` paquetss installés sur la machine.




- Problèmes eventuels:

> * Rapporter les problèmes eventuels.

## PHP -- Installation et configuration

- Vérifier les versions PHP disponibles avec le gestionaires des paquetages:

~~~csh
# Mettre le résultat de la commande suivante ici
apt-cache policy php
~~~

- Vérifier le module php pour apache

~~~csh
# Mettre le résultat de la commande suivante.
apt-cache policy libapache2-mod-php
~~~

- Installation: nous avons installé les paquatages demandés avec les commandes suivantes:

~~~csh
# Mettre les diférentes commandes d'installation ici
~~~




- phpinfo : Une capture d'écran de phpinfo

- Paquetages: nom du fichier contenant la liste et le nombre total des paquetages.

- Problèmes eventuels.
  
## Mysql 8.0 -- Installation et configuration

- Vérification

- Installation

- Configuration (donner extrait fichiers modifiers.)

- Problèmes eventuels
  
## Replication -- Installation et configuration

- Vérification

- Installation

- Configuration

- Problème eventuel
  
## WordPress -- Installation et configuration

- Téléchargement

~~~csh
# Commande pour télécharger
~~~

- Installation
 
 ~~~csh
 # Extrait wp-config.php modifié
 ~~~

- Autre configuration

> Captures d'écrans

- Installation Modules supplémentaires


## Réseau -- Informations de configuration


- Vérification: 
 
 ~~~csh
 # Mettre résultat de la commande
 ~~~



- Problèmes eventuels
  

## backup 

  
- Base de données