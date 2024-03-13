# Laboratoire 8 - Administration

## 1. SSH et Tmux (40 minutes)

### 1.1. Connexion aux serveurs de Labunix

- À l'aide de la commande `ssh`, connectez-vous à la machine
  `java.labunix.uqam.ca`.
- Quels sont les autres utilisateurs connecté, et de quelle machines
  viennent-ils ? Utilisez la commande `who`.
- Explorez sommairement le système de fichiers disponible dans votre répertoire
  personnel.
- Quel est le résultat de `ls /home/`? Croyez-vous qu'il soit sécuritaire
  d'avoir accès à cette information?
- À l'aide de la commande `cd`, essayez de visiter le répertoire d'un autre
  utilisateur. Que se passe-t-il?
- Consultez les permissions des répertoires contenus dans `/home`.
- Déconnectez-vous du serveur `java`.
- Connectez-vous maintenant à la machine `zeta2.labunix.uqam.ca`. Est-ce que les
  mêmes fichiers sont disponibles dans votre répertoire personnel? Pourquoi?

### 1.2. Génération de clés

- Générez une paire de clés RSA privée/publique en lançant la commande
  interactive `ssh-keygen` sans argument et en suivant les instructions.
- Affichez le contenu des clés publique et privée sur la sortie standard à
  l'aide de la commande `cat`.
- À l'aide de la commande `ssh-copy-id`, transférez votre clé publique
  (`id_rsa.pub`) sur le serveur `java`.
- Confirmez qu'il est maintenant possible de vous connecter sur `java` sans
  devoir saisir votre mot de passe.

### 1.3. Tmux

En consultant les dernières diapositives du chapitre 7 (réseau),
familiarisez-vous avec le logiciel Tmux:

- Connectez-vous en SSH sur le serveur `java`.
- Puis lancez une nouvelle session avec `tmux`.
- Créez plusieurs panneaux avec `Ctrl-B %` et `Ctrl-B "`.
- Changez la disposition des panneaux avec `Ctrl-B o` et `Espace`.
- Affichez l'heure avec `Ctrl-B t`.
- Détachez-vous de la session avec `Ctrl-B d`.
- Créez une deuxième session en relançant `tmux` puis quittez-la avec
  `Ctrl-B d`.
- Entrez la commande `tmux ls` pour lister les sessions existantes. Repérez les
  deux sessions que vous venez de créer.
  ```
   # tmux ls                                                                                  0: 1 windows (created Tue Mar 12 09:43:39 2024) 
   1: 1 windows (created Tue Mar 12 10:14:34 2024) 
   
  ```
- Chargez la première session que vous aviez créée avec `tmux a -t <nom>` où
  `<nom>` est remplacé par le nom de la session. Renommez-la session courante
  avec un nom plus facile à retenir et vérifiez le changement avec `Ctrl-B s`.
  ```
  tmux a -t 0 
  # ou
  tmux a -t 1
  ```
- Lancez le script [`ecrit`](./ecrit) disponible dans ce dépôt, détachez-vous
  de la session et déconnectez-vous du serveur `java`.
- Reconnectez-vous sur Java et chargez à nouveau la session `tmux` dans
  laquelle vous aviez lancé le script. Vérifiez que le script a bien continué
  de tourner malgré votre déconnection.

## 2. Docker (80 minutes)

### 2.1 Lancer un conteneur

Dans un terminal lancez la commande suivante :
```
sudo docker run -it ubuntu /bin/bash
```
- Qu'est ce qui se passe?
    - expliquez toute la ligne de commande;
    - lisez attentivement la sortie standard et la nouvelle invite de commande.
- Ouvrez un autre terminal et exécutez la commande `sudo docker ps`. Que voyez-vous? 
    - expliquez toutes les colonnes.
- Quittez le conteneur et tapez de nouveau `sudo docker ps`, que voyez-vous? ajouter l'option `-a` au `ps`, que voyez-vous? 
- Redémarrez (`docker start`) le conteneur, en utilisant l'ID ou le nom du conteneur. Vérifiez ensuite que ça a bien redémarré avec `sudo docker ps`. 
   - Remarquez que vous n'avez plus accès au terminal du conteneur.
   - Pour récupérer le terminal _rattachez-vous_ au conteneur avec `sudo docker attach ID`.
   - Ensuite installez `vim` et ouvrez le fichier `/etc/passwd`.
- Ouvrir un autre terminal et lancer la même commande `run` plus haut. Faites un `sudo docker ps` dans un autre terminal qu'est ce que vous voyez?
    - qu'est-ce que vous remarquez quand vous essayer d'utiliser `vim` dans ce nouveau conteneur?
- Arrêtez le deuxième conteneur et supprimer-le.
- Retournez dans le premier conteneur et installer `curl` et `apache2`, puis lancer `apache2`.
- En vous assurant qu'il n'y a aucun serveur web qui roule sur votre machine, allez sur votre fureteur et tapez `localhost:80`, que voyez-vous? Pourquoi?
- Créez un répertoire nommé `vim` sur votre machine;
    - téléchargez le fichier `.vimrc` founi dans ce dépôt et enregistrez-le dans le répertoire `vim`;
- Créez (`docker create`) un nouveau conteneur avec les spécifications suivantes :
    - utiliser l'option `--mount` dans le processus de création pour monter le répertoire `vim` dans le répertoire `/root` du conteneur;
    - lier les ports entre le conteneur (port `80`) et l'hôte (port `8888`) avec l'option `--publish`.
- Démarrez le conteneur nouvellement créé. 
- Refaites les installations de `vim`, `curl` et `apache2`.
    - Exécutez `vim /etc/passwd` quelle différence remarquez-vous par rapport à l'affichage précedent?
    - Lancer `apache2` et aller dans votre fureteur, que voyez-vous sur `localhost:80`? Que voyez-vous sur `localhost:8888`? Pourquoi?

### 2.2 Dockerfile
- Téléchargez le fichier `Dockerfile` fourni dans ce dépôt.
    - Expliquez toutes les lignes de ce fichier.
- Construisez l'image avec `docker build`.
- Créez un conteneur qui exécute directement `vim` avec `docker run` en remplaçant `/bin/bash` par le chemin de `/usr/bin/vim`.
- Créez un deuxième conteneur qui exécute `/bin/bash` avec `docker run` et lancez le serveur `apache2`.
