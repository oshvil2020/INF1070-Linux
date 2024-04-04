# Laboratoire 10 - Scripts shell

## 1. Redirections et substitutions (30 minutes)

* Écrivez un script shell POSIX `ave` qui affiche « `Bonjour!` » sur la sortie
  standard et « `Au revoir!` » sur la sortie d'erreur standard
* Soit la commande shell « `./ave >b 2>a` »; que contiendrait les fichiers `a`
  et `b` ? Exécutez-la pour vérifier votre prédiction.
* Qu'afficherait la commande shell « `{ ./ave | rev ; } 2>&1 | rev` » ?
  Exécutez-la pour vérifier votre prédiction.
* Modifiez le script `ave` pour qu'il affiche le nom de l'utilisateur en plus
  en utilisant la variable d'environnement `USER`. Par exemple « `Bonjour
  privat!` » et « `Au revoir privat!` »
* Qu'afficherait maintenant la commande « `USER=Anonyme ./ave` » ?
  Exécutez-la pour vérifier votre prédiction.
* Modifiez le script `ave` pour utiliser la substitution de la commande
  « `id` » à la place de `USER`. Consultez les option de `id` pour avoir juste
  le nom de l'utilisateur.
* Qu'afficherait maintenant la commande « `USER=Anonyme ./ave` » ?
  Exécutez-la pour vérifier votre prédiction.

## 2. Évaluation des enseignements (interlude)

Faites l'évaluation en ligne des enseignements: http://www.evaluation.uqam.ca/

## 3. Scripts et types (30 minutes)

* Ouvrez un fichier texte vide nommé `script.sh` avec `vim` et écrivez la 
  conduite suivante,
  ```sh
  { echo "Bonjour!"; cat big_fail; } &>fichier
  ```
* Mettez les droits adéquats pour qu'il soit exécutable.
* Quel *shebang* faut-il mettre? Pourquoi?
* Écrivez un script qui liste seulement les *scripts shell* du répertoire 
  `/bin`. Pensez à `find`.
* Modifiez ce script pour qu'il affiche un titre 
  `Liste des scripts shell du répertoire /bin` puis la liste des scripts shell. 
  Puis, il affiche un autre titre 
  `Liste des exécutables du répertoire /bin` et la liste des exécutables 
  LSB seulement.

## 4. Support au TP2 

* Lecture complète et détaillée de la page de présentation du [TP2](https://gitlab.com/moussa.abdenbi/inf1070-tp2).
* Explication des contraintes et des exigeances.
* Un survole rapide du format Markdown.
* Le format du rapport `tp2.md` et les critères d'évaluation.
