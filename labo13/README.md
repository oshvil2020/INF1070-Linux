# Laboratoire 12 - Réseau

## 1. Adresses IP (10 minutes)

- En ligne de commande, récupérez votre adresse IP privée à l'aide de la
  commande `ip`. Prenez-la en note.
- Ensuite, à l'aide de la commande `dig`, récupérez votre adresse IP publique.

## 2. *Socket* (30 minutes)

### 2.1. Bases de netcat

- Dans un terminal, entrez la commande `nc -l 3333 | rev`.
- Ouvrez un fureteur et rendez-vous à `localhost:3333`.
- Que voyez-vous apparaître dans le terminal?
- Rafraîchissez la page du fureteur. Est-ce que le terminal affiche de
  nouvelles informations? Pourquoi?
- Refaites le même exercice en ajoutant l'option `-k` à la commande `nc`.
  Au besoin, consultez le manuel (`man nc`).

### 2.2. Communications réseau

En équipe de deux étudiants, transmettez vous des messages par l'intermédiaire
de la commande netcat:

- Récupérez d'abord vos adresses IP respectives.
- Puis un premier étudiant ouvre une socket en mode « écoute » sur un port
  spécifique (par exemple 3333).
- Le deuxième étudiant envoie un message à l'aide de
  `echo message | nc adresse_ip`.
- Inversez ensuite les rôles.

## 3. Tester les connexions, la commande `ping` (20 minutes)

Comme on l'a vu en cours, cette commande permet de tester l'acheminement des
trames sur les réseaux et, accessoirement, de vérfier qu'une machine est bien
présente sur le réseau.

À l'aide du manuel de `ping`, à partir de votre machine, tester l'accessibilité
et la présence :

- de son interface `loopback` (une des adresses `127.n.m.k`)
- de la machine d'un de vos camarades dans le laboratoire
- de toutes les stations du réseau local accessibles en *broadcast*. Pour cela, 
préciser uniquement **2** tentatives
- de `www.etudier.uqam.ca`. Quelle est son adresse IP?

## 4. Le cache ARP (30 minutes)

- Qu'est-ce que le cache ARP? 
- À quel moment est-il utilisé par la suite de protocoles TCP/IP?
- À l'aide de la commande `arp`, obtenir toutes les associations présentes dans
le cache ARP de votre machine.
- À l'aide de la commande `arp`, prendre en note quelques entrées de votre cache
ARP, puis effacer-les.
- Effectuer maintenant des `ping` vers ces adresses et visualiser le cache ARP 
après chaque `ping`. Que s'est-il passé?


## 5. Téléchargement (30 minutes)

### 5.1. Interrompre et relancer un téléchargement

- À l'aide de la commande `curl`, lancez le téléchargement de la ressource
  https://cdn.kernel.org/pub/linux/kernel/v4.x/linux-4.18.9.tar.xz
- Interrompez-le avec `Ctrl-C`
- Puis relancez le téléchargement à l'aide de l'option `-C`.

### 5.2. Charger les locaux d'un cours

Sur le site de l'UQAM, il est possible d'afficher les locaux d'un cours en se
rendant sur la page https://etudier.uqam.ca. Par exemple, pour visualiser
l'horaire du cours INF1070, il suffit d'aller à l'adresse
https://etudier.uqam.ca/cours?sigle=inf1070.

Écrivez un petit script nommé `cours` qui prend en paramètre le sigle d'un
cours de l'UQAM (par exemple `inf1070`) et qui affiche sur la sortie standard
les locaux dans lesquels le cours se déroule.

Par exemple, on s'attend à ce que la commande

```sh
./cours inf1070
```

affiche le résultat

```sh
SH-R810
SH-3420
SH-3420
```
