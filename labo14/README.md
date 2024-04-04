# Laboratoire 13 - révision pour l'examen final

## Expressions régulières étendues

* Quels mots de la liste suivante correspondent à l'expression régulière
  `(le?|o[am])s` ?

~~~
réglementèrent
naturalisasse
sporotrichose
entraperçusses
ravaudiez
chitine
tuberculinerez
pénètreriez
augurales
compactasses
transistoriserions
inonderont
~~~

* Donnez des expressions régulières étendues qui trouvent une correspondance
  dans chacun des mots de la liste précédente mais dans aucun des mots de la
  liste suivante

~~~
contemplées
assonais
réserviez
commuait
logeât
inexcusable
toilettassiez
sous-paies
battront
quimpâmes
nourrissaient
exilée
~~~

Note: Il existe une expression régulière valide de 6 caractères

## Processus et tâches

* À l'aide de la commande `ps`, affichez tous les processus dont le parent est
  le processus de PID = 1.
* Lequel parmi les processus suivants consomment le plus de CPU? De mémoire?
* Dans deux terminaux différents, lancez en arrière-plan deux programmes (par
  exemple `chromium`, `xeyes`, `firefox`, etc.).
* À partir d'un des deux terminaux, utilisez la commande `ps` pour retrouver
  les PID des processus lancés dans l'autre terminal.
* Est-ce que la commande `jobs` vous permet de retrouver les processus lancés
  dans un autre terminal?
* Lancez un signal de terminaison avec `kill` au processus de PID = 1. Que se
  passe-t-il?
* Lancez un signal de terminaison avec `kill` à un processus qui appartient à
  un autre utilisateur que vous-même. Que se passe-t-il?
* Même question avec les processus qui ont été lancés dans l'autre terminal,
  puis ceux lancés dans votre terminal.
* Lancez la commande `kill` sur le shell correspondant au terminal courant. Que
  se passe-t-il?

## Shell avancé

* Pour chacune des commandes suivantes, essayez de deviner ce qui
  sera affiché sur la sortie standard, puis vérifiez votre réponse :

```sh
$ echo pomme | echo banane
$ echo pomme | { echo banane; tail; }
$ echo pomme | { echo banane; rev; }
$ echo pomme >&2 | { echo banane; rev; }
$ echo pomme >&2 | { echo banane; rev; } | tac
$ { echo pomme >&2; echo banane; } | rev | tac
$ f=fraise; echo $f
$ f=fraise; cat <<< $f
$ args=-als; ls $args
$ args=-als; ls -- $args
```

Consultez les fichiers `fruits` et `legumes` disponibles dans ce répertoire.
Quel sera le résultat de la commande suivante :

```sh
$ tac <(rev fruits) <(tr a-z A-Z < legumes)
```

## Réseau

Allez sur la page `http://info.uqam.ca/~privat/INF1070/`.

* Quelle est l'adresse IP du serveur web ?
* Quel est le numéro de port utilisé sur le serveur web ?
* Que se passe-t-il si on exécute la commande suivante? Expliquez ce qui s'est
  passé ?

```sh
echo "GET /~privat/INF1070/" | nc info.uqam.ca http | grep Semaine
```

Connectez vous sur `java.labunix.uqam.ca` en SSH

* Assurez-vous de vous connecter en utilisant les clés (sans mot de passe) !
* Quelles sont les interfaces réseau de la machine, ainsi que leurs addresses
  IP et MAC
* Faites la même chose sur la machine `zeta2.labunix.uqam.ca`

Exécutez la commande suivante

```sh
nc -l 8888 | sed -u 's/localhost:8888/info.uqam.ca/' | nc info.uqam.ca http
```

* Laissez la commande s'exécuter
* Puis, ouvrez le lien http://localhost:8888/~privat/INF1070 dans votre
  navigateur
* Qu'est-ce qui s'affiche dans le navigateur ? Qu'est-ce qui s'affiche dans la
  console ? Pourquoi ?
