# INF1070 Automne 2024 - Travail pratique 2

<!-- Sous linux installer grip pour visualiser markdown -->
**Ce travail est individuel (à faire seul).**

Ce travail porte principalement sur l'installation, l'administration et l'utilisation sommaire de [WordPress](https://wordpress.org/) avec réplication de la base de données MySQL. Ce travail nécessitera de lire la documentation technique et de suivre les procédures 
et les instructions données. Il est nécessaire de bien comprendre chaque étape parce  que l'installation et la configuration devront être personnalisées selon les informations 
spécifiques à chaque personne. Un Dockerfile ([DockerfileINF1070TP2_template](DockerfileINF1070TP2_template)) et un docker-compose.yml([docker-compose-tp2_template.yml](docker-compose-tp2_template.yml)) sont fournis avec cet énoncé.  **Il est obligatoire d'utiliser ces fichiers selon les instructions du présent TP**. 

## Instructions générales

- Ce travail permet d'expérimenter quelques tâches effectuées  habituellement par un administrateur système. L'exercice consiste à installer et configurer une application web avec les prérequis nécessaires. Pour réduire les risques de perte d'informations, on va configurer une réplication de la base de données.
  Pour cela, on demande de procéder selon les instructions données ci-après.

- Pour faciliter la reproduction des étapes importantes, on vous demande de créer un fichier `rapport-tp2.md` 
  selon le modèle fourni avec cet énoncé et d'y mettre les différentes commandes ainsi 
   que les résultats demandés. Chaque commande sera précédée d'un commentaire justifiant son utilisation. Ex: pourquoi on installe un paquet ou pourquoi 
   on active un module.

- Il faudra également mettre les extraits des fichiers de configuration modifiés dans le rapport `rapport-tp2.md`.

- Certaines commandes données dans l'énoncé peuvent nécessiter l'utilisation de `sudo` pour les exécuter


## Environnement de travail - Configuration initiale

L'installation et la configuration doivent se faire sur des conteneurs Docker créés selon les spécifications suivantes. 

### Préparation de l'environnement spécifique

 Dans cet énoncé:

- `<codepermanent>` réfère à votre code permanent écrit en minuscules, `XYZ` correspond aux 3 derniers chiffres du code permanent et `ABCD` correspond aux 4 premières lettres du code permanent. 
   - Par exemple, si votre code permanent est `TRAP12345678`, 
        - Remplacez `<codepermanent>` par `trap12345678` 
        - `XYZ` de ce code permanent est `678` 
        - Si on demande de remplacer `<4XYZ>` par exemple, dans le cas de cet exemple-ci, on écrira `4678`
        - Si on demande de remplacer `<dbXYZ>` par exemple, dans le cas de cet exemple-ci, on écrira `db678`
        - Si on demande de remplacer `<ABCDnet>` par exemple, dans le cas de cet exemple-ci, on écrira `trapnet`

- `<codems>` est votre code ms et `UVW` les 3 derniers chiffres du code ms. **Si dans votre cas, ces 3 chiffres sont identiques avec `XYZ` de votre code permanent, alors utilisez les 3 premiers chiffres du code ms pour `UVW`**.
    - Par exemple, si votre code ms est `ab123456`
        - Remplacez `<codems>` par `ab123456`
        - `UVW` de ce code ms est alors `456`

- Avec vos informations de code ms et code permanent, créez un fichier nommé `info-<codems>.sed` avec les commandes de substitution `sed` pour les motifs suivants. Votre commande doit s'assurer de **remplacer toutes les occurrences (bien lire le manuel de `sed`)**. Ce fichier sera utilisé pour faire des substitutions dans des fichiers qui peuvent contenir plusieurs occurrences d'un motif dans une même ligne.

    - `<codepermanent>`
    - `<codems>`
    - `<4XYZ>`
    - `<4UVW>`
    - `<wpXYZ>`
    - `<webXYZ>`
    - `<masterXYZ>` 
    - `<dbXYZ>`
    - `<slaveUVW>` 
    - `<dbUVW>`
    - `<ABCDnet>` 
    - `<webUVW>`
    - `<tmprepXYZ>`
    - `<tmprepUVW>`
    - `<repUVW>`


Le fichier `info-<codems>.sed` devra faire partie de la remise. 

###  Dockerfile et docker-compose pour conteneur Ubuntu 22.04

**L'utilisation de cet environnement est obligatoire.**

- Un [`conteneur`](https://www.docker.com/resources/what-container) est une unité standard contenant l'installation minimale (librairies, paquets, configuration, etc.) nécessaire pour faire fonctionner une application. Dans ce TP, un `conteneur` docker pourra être utilisé comme une machine Linux (terminal seulement - pas d'environnement graphique).

- Pour travailler avec cet environnement, il faut avoir [installé Docker](https://docs.docker.com/install/). (Pour les 
  anciennes versions de Windows ou Mac voir [ici](https://docs.docker.com/toolbox/overview/).)
    - **Windows**: [instructions d'installation](https://docs.docker.com/docker-for-windows/install/) (Si vieille version Windows voir 
[ici](https://docs.docker.com/toolbox/overview/))
    - **Mac OS**: [instructions d'installation](https://docs.docker.com/docker-for-mac/install/)
    - **Linux Ubuntu**: [instructions d'installation](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
    - **Linux Fedora**: [instructions d'installation](https://docs.docker.com/install/linux/docker-ce/fedora/) 
    - Sous Linux, il faut s'assurer d'installer les paquets `docker.io, docker-compose`

**Note:** Docker verion 24 ou plus récente est requise pour ce TP

- Exécuter successivement les commandes qui font les opérations suivantes.
    - Renommer le fichier `DockerfileINF1070TP2_template` fourni en `DockerfileINF1070TP2_<codems>`
    - Renommer le fichier `docker-compose-tp2_template.yml`  fourni  en `docker-compose-tp2_<codems>.yml`
    - Renommer le fichier `mysql-replicate_template.sql` fourni en `mysql-replicate_<codems>.sql`

- Exécuter successivement les commandes qui doivent prendre le fichier `info-<codems>.sed` en argument et produire les effets suivants. **Ne pas utiliser de redirection, ni de tube (pipe)**
    - Effectuer toutes les substitutions de `info-<codems>.sed` sur `DockerfileINF1070TP2_<codems>` et en même temps, sauvegarder une copie du fichier original (avant substitutions) dans `DockerfileINF1070TP2_<codems>_template` 
    - Effectuer toutes les substitutions de `info-<codems>.sed` sur `docker-compose-tp2_<codems>.yml` et en même temps, sauvegarder une copie du fichier original (avant substitutions) dans `docker-compose-tp2_<codems>.yml_template`
    - la même chose pour `mysql-replicate_<codems>.sql`

- Donner les commandes pour trouver chacun des nombres suivants ( **Pas plus d'un tube par ligne de commande**)
    - Nombre d'occurrences de votre code ms dans le fichier  `DockerfileINF1070TP2_<codems>`
    - Nombre d'occurrences de votre code permanent dans le fichier  `DockerfileINF1070TP2_<codems>`
    - Nombre d'occurrences de votre code ms dans le fichier `docker-compose-tp2_<codems>.yml`
    - Nombre d'occurrences de votre code permanent dans le fichier `docker-compose-tp2_<codems>.yml`

**Note:** Si vous n'arrivez pas à trouver les commandes pour faire ces substitutions, vous  pouvez le noter dans le rapport puis éditer les fichiers manuellement.
Dans ce cas, il faut veiller à respecter les indentations du fichier `docker-compose-tp2_<codems>.yml`

## Installation du sytème d'exploitation

### Création du réseau pour les conteneurs

- Exécuter la commande suivante sur un terminal
~~~csh
docker network create <ABCDnet>
~~~
Rappel: Remplacer `<ABCDnet>` selon votre code permanent et les explications données au début de l'énoncé.


- Plus tard, lorsque les conteneurs seront démarrés, vous donnerez le résultat de la commande suivante dans le rapport

~~~csh
docker network inspect <ABCDnet>
~~~

### Création des images et conteneurs docker

**Note**: Pour cette section, assurez-vous d'avoir une bonne connexion Internet et d'avoir fait toutes substitutions expliquées au début du fichier. 

- Pour cette partie, il faut se mettre dans le répertoire où se trouvent les fichier `DockerfileINF1070TP2_<codems>` et `docker-compose-tp2_<codems>.yml`.

- Pour créer les conteneurs, exécuter la commande suivante:
~~~csh
docker compose -f docker-compose-tp2_<codems>.yml create
~~~
- Précisions:
    - Ne pas oublier de mettre la commande exacte (après substitution du <codems>) dans le rapport
    - Ici on créer les images et les conteneurs une fois pour toutes
    - Pour plus d'information sur la commande docker compose, tapez ```docker compose --help```
    - Voir aussi [https://docs.docker.com/reference/cli/docker/compose/](https://docs.docker.com/reference/cli/docker/compose/)
    - Autrement, vous pouvez créer et démarrer les conteneurs directement avec la commande suivante.

>>>~~~csh
>>> docker compose -f docker-compose-tp2_<codems>.yml up -d
>>>~~~


- Taper la commande suivante pour donner la liste des images docker créées


~~~csh
docker compose -f docker-compose-tp2_<codems>.yml images
~~~


- Pour voir si les conteneurs ont bien été créés

~~~csh
docker ps -a -f name=<webXYZ> -f name=<dbXYZ> -f name=<dbUVW>
~~~

- Précisions:
    - Donnez le résultat dans le rapport.
    - Normalement, vous devriez voir 3 conteneurs avecs les noms:
        - `<webXYZ>` (sera utilisé pour le serveur web) 
        - `<dbXYZ>`  (sera utilisé pour la base de données principale `master`)
        - `<dbUVW>`  (Sera utilisé pour la base de données de relève `slave`)

- Pour démarrer les conteneur 

~~~csh
docker compose -f docker-compose-tp2_<codems>.yml start
~~~

- Vérifier que les conteneurs sont bien démarrés avec la commande ci-dessous.
Elle devrait aussi montrer les ports utilisés.

~~~csh
docker ps -a -f name=<webXYZ> -f name=<dbXYZ> -f name=<dbUVW>
~~~

- **Avertissement et arrêt des conteneurs en cas de besoin**

    - La création/le démarrage des conteneurs peut donner lieu à la création des certains sous répertoires. 
        - Ces répertoires sont "montés" dans les conteneurs pour la persistances des données et certaines configurations. 
        - Ces répertoires/fichiers vous seront utiles si vous deviez recommencer. 
        - Vous allez les rendre avec votre solution.
    - Au besoin, vous pouvez arrêter les conteneurs puis les démarrer plus tard. Pour cela utilisez la commande ci-dessous.
    Plus tard, vous pourrez les démarrer avec la commande donnée plus haut.

~~~csh
docker compose -f docker-compose-tp2_<codems>.yml stop
~~~


- **Attention:** 
>- La commande `docker compose -f docker-compose-tp2_<codems>.yml down` **est à éviter pour ce TP2 parce qu'elle supprime aussi les conteneurs!**. N'utilisez cette commande que quand vous voulez réellement supprimer les conteneurs ex (pour recommencer).
>- La commande `docker compose -f docker-compose-tp2_<codems>.yml create --build`  ou `docker compose -f docker-compose-tp2_<codems>.yml up --build` permettent de créer à nouveau les images docker en cas de besoin (après modification des fichier Dockerfile ou docker-compose par exemple)
>- Pour supprimer les images docker (Voir [https://gitlab.info.uqam.ca/inf1070/labs/-/tree/master/docker?ref_type=heads](https://gitlab.info.uqam.ca/inf1070/labs/-/tree/master/docker?ref_type=heads)) 



### Connexion aux conteneurs

- Serveur web: `docker exec -it -u <codems> <webXYZ> /bin/bash`
- Serveur base de données `master`: `docker exec -it -u <codems> <dbXYZ> /bin/bash`
- Serveur base de données `slave` : `docker exec -it -u <codems> <dbUVW> /bin/bash`


On sera connecté sur le conteneur avec le compte suivant:

- Nom d'utilisateur : `<codems>`
- Mot de passe : `INF1070pass`

Le mot de passe configuré dans le fichier docker va servir essentiellement pour les commandes `sudo`.

- Mettre dans le rapport le résultat de la commande suivante

~~~csh
 id <codems>
~~~

## Identification du système d'exploitation

Se connecter sur chaque conteneur et donner les informations suivantes.

- Commencer par mettre  les informations d'identification du système d'exploitation dans le fichier `rapport-tp2.md`. Pour cela, exécuter la commande suivante pour obtenir ces informations (pour plus d'info voir [https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/](https://www.cyberciti.biz/faq/how-to-check-os-version-in-linux-command-line/)). 

~~~csh
lsb_release -a
~~~

- En utilisant la commande suivante (sur le conteneur), enregistrer la liste des paquets installés initialement dans chaque conteneur:
 
~~~csh
apt list --installed >/inf1070/paquets0<webXYZ>.txt
apt list --installed >/inf1070/paquets0<dbXYZ>.txt
apt list --installed >/inf1070/paquets0<dbUVW>.txt
~~~

- En utilisant les commandes `tail` et `wc` ainsi que les 3 fichiers, dites dans le rapport combien des paquets sont installés initialement pour chaque conteneur.

## Apache -- Installation et configuration

L'installation et la configuration d'Apache se fait sur le conteneur `<webXYZ>`. À l'aide du gestionnaire de paquets `apt`:

- Installer le serveur web `apache2`
- Vérifier si le service `apache2` est démarré. Sinon, démarrez-le.
- Remplir le rapport  (`rapport-tp2.md`) en indiquant le chemin du répertoire (`DocumentRoot`) par défaut du site web et les commandes utilisées pour vérifier le statut du service `apache2` et pour le démarrer.
- **Attention:**  
    - Par défaut, dans le conteneur le site est accessible sur le port 80 pour http et 443 pour https.
        - *Note:* Ne modifiez pas les ports ni le `DocumentRoot`
    - Sur la machine locale, le serveur est accessible sur le port <4XYZ> pour http et <4UVW> pour https.
- Visitez le site ([http://localhost:4XYZ/](http://localhost:4XYZ/))
- Fournir une capture d'écran (doit montrer l'URL utilisé) de cette page d'acceuil dans `rapport-tp2.md` 
- Enregistrer encore une nouvelle liste des paquets installés (`paquets1<webXYZ>.txt`) et  dire, au total, combien de paquets sont installés maintenant?

## PHP -- Installation et configuration sur `<webXYZ>`

- Installer php 8.1 et les paquets suivants

~~~csh
  php8.1 php8.1-mysql php8.1-ssh2 php8.1-cgi php8.1-xml libapache2-mod-php8.1 libapache2-mod-xforward \
  libapache2-mod-upload-progress php8.1-cli php8.1-yaml libphp8.1-embed php8.1-apcu php8.1-bz2 php8.1-common \
  php8.1-curl php8.1-fpm php8.1-gd php8.1-igbinary php8.1-http php8.1-imagick php8.1-imap  php8.1-intl \
  php8.1-ldap php8.1-mailparse php8.1-mbstring php8.1-memcached php8.1-oauth php8.1-pgsql php8.1-ps php8.1-readline \
  php8.1-soap php8.1-uuid php8.1-xmlrpc php8.1-zip php8.1-raphf
~~~

- Activer le module php et redémarrer le service `apache2`.
- Documenter les problèmes eventuels (s'il y en a eu) dans le fichier `rapport-tp2.md`
- Mettre le fichier `phpinfo.php` dans le répertoire <webXYZ> créé suite à création des conteneur puis visiter les site [http://localhost:4XYZ/phpinfo.php](http://localhost:4XYZ/phpinfo.php)
- Faire encore une copie de la liste des paquets installés (`paquets2<webXYZ>.txt`) et donner le nombre total dans le rapport
- Ne pas oublier d'enregistrer les commandes utilisées dans le rapport, `rapport-tp2.md`.


## Installation de MySQL sur `<dbXYZ>` (`master`)  et `<dbUVW>` (`slave`)

Effectuer les opérations suivantes sur chacun des deux conteneurs des bases de données

- Se connecter sur le conteneur
- Installer les paquets suivants

~~~csh
mysql-server-8.0 mysql-server-core-8.0 mysql-client-8.0 mysql-client-core-8.0
~~~
>- Cela peut prendre quelques temps et pourrait afficher quelques avertissements. Mysql devrait 'installer correctement.

- Vérifier si le service `mysql` est démarré. Sinon, il faut le démarrer. 
- Extraire les listes des paquets installés (`paquets3<dbXYZ>.txt` et `paquets3<dbUVW>.txt` respectivement) et dire le total à ce niveau pour chacun des conteneurs.
- Se connecter sur MySQL en tant que `root` avec la commande suivante

~~~csh
 sudo mysql -u root
~~~

- **Note:** 
    - Ici `root` est le super utilisateur de `mysql`. Il ne faut pas le confondre avec le super utilisateur `root` de Linux.
    - Par défaut, il n'y a pas de mot de passe pour une connexion locale avec l'utilisateur `root` (de MySQL). Pour ce TP, on va laisser comme ça.
    - Pour se déconnecter de mysql, il faut taper la requête `exit;`

- Une fois connecté sur mysql, mettre dans le rapport le résultat de la requête suivante:

~~~csh
 show databases;
~~~

## Configuration de la réplication la base de données

La réplication permet de copier les données d'un serveur de base de données dite `source` (ou `maitre` ou encore `master`) à un (ou plusieurs) autre serveur dit `destination` (ou `esclave` ou encore `slave`).  Le réplication permet notamment:

- De répartir la charge en permettant l'écriture seulement sur le `master` mais en autorisant la lecture sur le `slave`.
- D'assurer une plus grande disponibilité. Si on a un problème avec un serveur `master` par exemple, on peut alors basculer sur l'autre serveur `slave` sans perdre trop de temps. 
- Pour plus d'information [https://dev.mysql.com/doc/refman/8.0/en/replication.html](https://dev.mysql.com/doc/refman/8.0/en/replication.html).

**Note:** Dans le cadre de ce travail, l'accent est mis essentiellement sur les tâches habituellement effectuées par un administrateur système. Ce sera spécifiquement l'installation et la configuration de la réplication sans entrer dans le fonctionnement de la base de données proprement dite.


- MySQL 8.0 a plusieurs approches de réplication.
Dans le cadre de ce travail, nous allons configurer l'approche utilisant les positions du journal binaire ( `binary log file positions`).
Dans cette approche, le `master` écrit les événements dans le journal puis le `slave` lit le journal puis reproduit ces événements sur sa base de données locale.
Chaque serveur `slave` enregistre le nom du fichier journal se trouvant sur le `master` et la position qu'il a déjà lue et traitée.

- Pour activer la replication sur le `master`, connectez-vous sur le conteneur `<dbXYZ>` puis éditez la section `[mysqld]`
   du fichier `/etc/mysql/mysql.conf.d/mysqld.cnf`  et assurez-vous d'avoir les valeurs suivantes

~~~csh
server-id               = <XYZ>
replica_load_tmpdir = /tmprep<XYZ>
bind-address            = *
innodb_flush_log_at_trx_commit = 1
sync_binlog = 1
~~~

- **Précisions**:
    - Ces lignes doivent-être **après** la ligne `[mysqld]`
    - Pour rappel, dans ce fichier de configuration,  `#` met le reste de la ligne en commentaire
    - Par défaut, `log_bin` est déjà activé sur mysql 8. 
    - Il faut redemarrer le service `mysql` après ces modifications.

- Activer la replication sur le `slave` en se connectant sur `<dbUVW>` pour éditer la section `[mysqld]`
   du fichier `/etc/mysql/mysql.conf.d/mysqld.cnf` en s'assurant d'avoir les valeurs suivantes.

~~~csh
server-id               = <UVW>
replica_load_tmpdir = /tmprep<UVW>
bind-address            = *
~~~

- Redémarrer le service `mysql` après ces modifications.

**Note**: la valeur de `server-id` doit-être unique. Deux serveurs ne peuvent pas avoir la même valeur pour la replication.

- Création de compte mysql sur `master` (<dbXYZ>) pour la replication 

>- Se connecter sur le `master` (`<dbXYZ>`)
>- Se connecter en tant que `root` sur mysql

>>~~~csh
>> sudo mysql -u root
>>~~~

>- Exécuter la requête suivante sur mysql pour créer l'utilisateur `<repUVW>` avec le mot de passe `INF1070pass`

>>~~~csh
>> CREATE USER '<repUVW>'@'%' IDENTIFIED BY 'INF1070pass';
>>~~~

>- Exécuter la requête suivante pour donner à cet utilisateur les permissions pour la replication

>>~~~csh
>> GRANT REPLICATION SLAVE ON *.* TO '<repUVW>'@'%';
>>~~~

>- Vérifier que l'utilisateur a bien été créé avec les permissions qu'il faut sur `master`.

>>~~~csh
>> select user,host,repl_slave_priv from mysql.user;
>>~~~


- Virification de compte sur `slave` de connexion de l'utilisateur  `<repUVW>` sur `master` à partir de `slave`

>- Se connecter sur un terminal linux de `<dbUVW>`

>- Vérifier que le compte `<repUVW>` n'existe pas encore sur `<dbUVW>`
>>- se connecter en tant que `root` sur mysql
>>>~~~csh
>>> sudo mysql -u root
>>>~~~
>>- Lister les compte existant actuellement sur `<dbUVW>`
>>>~~~csh
>>> select user,host from mysql.user;
>>>~~~

>>>- Valider que `<repUVW>` n'et pas dans la liste
>>- Se déconnecter de mysql local

>- Exécuter la commande suivante pour se connecter sur sur mysql du serveur `<dbXYZ>` avec le compte `<repUVW>`

>>~~~csh
>> mysql -u <repUVW> -h <dbXYZ> -p
>>~~~

>>>- À la demande, saisir le mot de passe assigné lors de la création du compte ( `INF1070pass`)
>>>- Pour se deconnecter, taper la commande `exit;`

- Prendre les coordonées du journal sur le serveur `master` (`<dbXYZ>`)

>- Avec un premier terminal, Se connecter en  `root` sur  mysql de `master` (`<dbXYZ>`)
>- Exécuter la requête suivante

>>~~~csh
>> FLUSH TABLES WITH READ LOCK;
>>~~~
  

>>>- **Restez connecter sur mysql avec ce terminal pour maintenir le lock**. C'est pour éviter toute modification avant de prelever les coordonées du journal.

>- Avec un deuxième terminal, Se connecter en  `root` sur  mysql de `master` (`<dbXYZ>`)
>- Exécuter la requête suivante

>>~~~csh
>> SHOW MASTER STATUS;
>>~~~

>>>- Bien noter particulièrement les valeurs des colonnnes `File` et `Position`. Ça constitue les coordonnées du journal à partir desquelles la replication va commencer.


- Avec le premier terminal de `<dbXYZ>` sur mysql, exécuter la requête suivante pour débloquer le lock de lecture

>>~~~csh
>> UNLOCK TABLES;
>>~~~

- Quitter la session mysql sur le premier terminal, **après** le prélèvement des coordonnées du journal

- Démarrage de la réplication sur le serveur `slave` (`<dbUVW>`)

>- Vérifier que la version mysql est supérieure à 8.0.23 d'une des manières suivantes (Quelle versioon vous utilisez?)
>>- Exécuter la reqête `select version();` sur mysql en tant que `root`
>>- À parti d'un terminal Linux, exécuter la commande: `sudo mysql -V` 
>- Montrez la liste des bases des données et utilisateur existant actuellement sur `<dbUVW>`

>- Editer le fichier `mysql-replicate_<codems>.sql` (Ceci est un script sql) avec le nom de fichier et la position du journal prélevé du `master`

>> Pour rappel, Avant substitution, avait pour modèle (template)
>>>~~~csh
>>> CHANGE REPLICATION SOURCE TO
>>>          SOURCE_HOST='<dbXYZ>',
>>>          SOURCE_USER='<repUVW>',
>>>          SOURCE_PASSWORD='INF1070pass',
>>>          SOURCE_LOG_FILE='valeur obtenu de master',
>>>          SOURCE_LOG_POS=valeur_position_obtenue_demaster;
>>>~~~
>>>>- **Ne pas oublier le ';' à la fin de la dernière ligne seulement**

>- À partir d'un terminal Linux sur `<dbUVW>` (`slave`) exécutez la commande suivante.

>>~~~csh
>> sudo mysql -u root -h localhost < mysql-replicate_<codems>.sql 
>>~~~

>- Se connecter à mysql sur <dbUVW> (`slave`) evec le compte `root` et vérifier le status de la réplication avec la requête suivante.

>>~~~csh
>> show replica status \G
>>~~~

>>>- Vérifier que la valeur de `Source_Server_Id:` correspond bien à la valeur de `server-id` sur `<dbXYZ>` (voir  `mysqld.cnf`)

- Arrêt/démmarage de la réplication en cas de besoin

>- En cas de besoin, se connecter à mysql sous `root` sur `<dbUVW>`
>>- Pour stoper la réplication, tapez la requête: `stop replica \G`
>>- Pour vérifier le status de la replication: `show replica status \G`
>>- Pour Démarrer la réplication: `start replica \G`

- Quelques minutes après le démarrage de la réplication, vérifiez les bases de données sur `<dbUVW>` avec la requête suivante exécutée en tant que `root` sur mysql

~~~csh
 show databases\G
~~~ 

## Installation WordPress sur <webXYZ>

### Creer un utilisateur et une base de données sur `<dbXYZ>`

- Se connecter sur `<dbXYZ>`
- Créer une base de données mysql nommée `<wpXYZ>` sur `<dbXYZ>`
>>~~~csh
>> CREATE DATABASE <wpXYZ>;
>>~~~

- Créer un utilisateur mysql `<wpXYZ>` sur `<dbXYZ>`

>>~~~csh
>> CREATE USER '<wpXYZ>'@'%' IDENTIFIED BY 'INF1070pass';
>>~~~
>>- **Note:** Dans la vraie vie, pour des raisons de sécurité, le mot de passe devrait être spécifique. Dans ce TP on utilise le même pour simplifier le travail et éviter des problème en cas d'oubli.

- Attribuer toutes les permissions à l'utilisateur `<wpXYZ>` sur la base de données `<wpXYZ>`

>>~~~csh
>> GRANT ALL PRIVILEGES ON <wpXYZ>.* TO '<wpXYZ>'@'%';
>>~~~

### Installer wordPress 6.5 sur le conteneur `<webXYZ>`


- Se connecter sur le conteneur `<webXYZ>`
- Aller dans le répertoire `/var/www/html`
- Renommer le fichier `index.html` se trouvant dans le répertoire en `index.html_old`

- Utiliser la commande `curl`, télécharger WordPress 6.5 de la manière suivante (connexion Internet requise pour le téléchargement).
>~~~csh
> curl -LO https://github.com/WordPress/WordPress/archive/refs/heads/6.5-branch.zip
>~~~

- Utiliser la commande unzip pour décompresser le fichier `6.5-branch.zip`

- En utilisant une expression glob, tapez une commande pour déplacer tout le contenu du répertoire `WordPress-6.5-branch` dans le répertoire `/var/www/html`
- Copier le fichier le fichier `wp-config-sample.php` en `wp-config.php` (dans `/var/www/html`)
- Editer le fichier `wp-config.php` avec les informations de la base de données et de  d'utilisateur mysql `<wpXYZ>` créé précédemment sur `<dbXYZ>` qu'il faut aussi utiliser comme nom de serveur de base de données.
- Installez wordpress via l'interface web:
>- Avec fureteur web graphique (Chrome, Firefox ou Edge), Allez sur [http://localhost:4XYZ](http://localhost:4XYZ) 
>- Mettez les informations suivantes pour la configuration:
>>- Titre du site: `TP1-INF1070-<codems>`
>>- Compte administrateur (qui sera créé sur l'application WordPress): `<adminXYZ>`
>>- Pour mot de passe de `<adminXYZ>`, mettre `INF1070pass<codepermanent>`
>>>- N'oubliez pas de mettre le nom d'utilisateur admin et le mot de passe dans le rapport
>- Après installation, si l'application vous demande de vous connecter sur l'interface web, utilisez le compte `<adminXYZ>`

- Les urls utilisés:

>- Site public WordPress: [http://localhost:4XYZ](http://localhost:4XYZ)
>- Administration du site WordPress: [http://localhost:4XYZ/wp-admin](http://localhost:4XYZ/wp-admin). C'est l'url qu'il faut utiliser pour créer des pages et administrer le site WordPress.


## Vérification de la réplication sur `<dbUVW>`

- Se connecter avec mysql `root` sur `<dbUVW>`
- Vérifier le status de la replication (Si la réplication ne fonctionne pas, la redemarrer)
- Vérifer les bases de données disponible:

>>~~~csh
>> show databases\G
>>~~~ 
>>>- Assurez vous quel la base de données `<wpXYZ>` créé sur `<dbXYZ>` a bien été repliquée ici sur `<dbUVW>`

- Vérifier les utilisateurs

>>~~~csh
>> select user from mysql.user\G
>>~~~ 

- À Partir d'un terminal Linux de `<dbXYZ>`, essayez de vous connecter à mysql sur `<dbUVW>` avec le compte `<wpXYZ>`

>>~~~csh
>> mysql -u <wpXYZ> -h <dbUVW> -p
>>~~~ 

>- En cas de succès, exécutez la requête suivante.

>>~~~csh
>> select user()\G
>>~~~


## Utilisation de WordPress

- Se connecter sur l'interface d'administration ([http://localhost:4XYZ/wp-admin](http://localhost:4XYZ/wp-admin)) de wordPress avec le compte administrateur créé précédemment (au moment de l'installation)
- Dans le menu "apparence", choisir un "thème" de votre choix pour l'apparence du site. N'oubliez pas de publier la page et de dire dans le rapport, le thème choisi. 
- Créez une page d'acceuil et dites comment avez-vous apprécier ce TP 2?
- Faites une capture d'écran à mettre dans le rapport (la capture d'écran doit montrer l'URL utilisé).


## Backups de la base de donnée à partir de `slave` (`<dbUVW>`)

- Se connecter sur un terminal Linux du conteneur `<dbUVW>`
- Se connecter sur mysql en tant que `root` et arreter la replication
- Se déconnecter de mysql, mais rester sur un terminal Linux de `<dbUVW>`
- Faire la sauvegarde (backup) de la bse de donnée `<wpXYZ>` avec la commande suivante
>~~~csh
> sudo mysqldump  <wpXYZ> >/home/<codems>/<wpXYZ>.sql
>~~~
>> Fichier à rendre dans le rapport
>>- Plus d'info sur la commande [https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)
- Se connecter connecter à nouveau en tant que `root` sur mysql et relancer la replication 


## Réseau <ABCDnet>

- Sur un terminal  de votre machine locale taper la commande suivantes pour afficher le réseau 

>~~~csh
> docker network inspect <ABCDnet>  > <ABCDnet>.info
>~~~

- En se connectant sur chaque conteneur, donnez les informations suivantes et les comande utilisées dans le rapport:
>- Nom
>- Adresse IPv4
>- Adresse Physique (Adresse Mac)
>- À partir du conteneur dd serveur web, quelle commande peut-on utiliser pour vérifier respectivement les conteneur des serveurs de base de données `master` et `slave` sont allumés


## Modalités de remise

- Créer une archive `tar` compressée avec `gzip`, `tp2.tar.gz`, qui contient tout le répertoire du TP2 avec les fichiers et répertoires crées.
- Les captures d'écran demandées doivent-être dans l'archive même s'il n'y a pas un lien qui les affiche dans le rapport markdown.

Votre travail doit être remis au plus tard le **23 avril 2024 à 23h55** par l'intermédiaire de la plateforme [Moodle](https://www.moodle.uqam.ca/). Vous  devez remettre qu'un seul fichier tar.gz nommé exactement `tp2.tar.gz` et contenant les fichiers suivants:

- Le rapport `rapport-tp2.md` en format Markdown
- Les listes de paquets
- Le script SQL de backup
- Le script SQL de lancement de replication
- Le fichier Dockerfile avec les substitutions faites
- Le fichier Docker-compose avec les substitutions faites
- Le fichier sed contenant les commandes de substitution
- Les captures d'écran
- Les répertoires créés lors de la création des conteneurs
- Tous les fichiers templates fournis et les sauvegarde produits lors de la substitution
- Autres fichiers pertinents éventuels (optionnel)


**Aucune remise ne sera acceptée par courriel ou après la date de remise**, peu importe le motif. Plus précisément, si vous envoyez votre travail par courriel, il sera considéré comme non remis. Il est donc de votre responsabilité de vous assurer d'être capable de faire une remise à  temps par l'intermédiaire de Moodle.

Il vous revient de vérifier que votre travail a été bien remis sur Moodle. Peu importe le motif, aucune mise à jour ou modification des fichiers remis ne sera permise après la date butoir pour la remise du TP.


## Le format Markdown

- La documentation est disponible sur Internet.
    - ex: [https://en.support.wordpress.com/markdown-quick-reference/](https://en.support.wordpress.com/markdown-quick-reference/)
- Votre rapport doit être en format Markdown.

## Barème de correction

Les points suivants seront considérés lors de l'évaluation:

- Travail **corrigé sur 100** points
- **5** points: Les  fichiers des listes de paquets.
- **3** points: Fichier docker file avec toutes les substitutions
- **3** Points: fichier docker-compose avec toutes les substitutions
- **5** Points: fichier `mysql-replicate_<codems>.sql` sql avec toutes les substitutons
- **6** Points: fichier des commandes `sed`
- **5** Points: Le fichiers MySQL de backup
- **5** points: Les captures d'écrans.
- **3** Points: les fichiers templates produits suite à la substitution
- **5** Points: Répertoires générés avec la création des conteneurs et leurs contenus
- **10** Points: copies des données deux bases de données (`master` et `slave`) contiennent bien les données et sont au chemins relatifs attendus
- **50** points: le conteu de **rapport-tp2.md**
    * **5** points: commande  des substitution produisant un backup
    * **2.5** points: Tous les conteneurs bien créés
    * **2.5** points: Identification
    * **5** points: Apache
    * **2.5** points: PHP
    * **2.5** points: MySQL
    * **10** points: WordPress (Version 6.5)
    * **10** points: Réplication
    * **5** points: Fichier `<ABCDnet>.info` et les informations réseau demandées pour chaque conteneur
- **5** points: Appréciation globale et possibilité de reproduire l'expérience avec les informations et les fichiers fournis.
