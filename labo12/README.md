# Laboratoire 11 - Scripts shell

## 1. Scripts shell (60 minutes)

Dans les exercices qui suivent, référez-vous à la section *Structures de
contrôles* du chapitre 6 du cours: [Scripts
shell](http://info.uqam.ca/~privat/INF1070/06b-script.pdf)

### Structures conditionnelles et tests

Faites un script shell POSIX `foobar` qui prend un seul argument et l'affiche à
l'écran. Si l'argument est le nombre 3, le script affiche `foo` à la place.  Si
l'argument est le nombre 4, le script affiche `bar` à la place.  Si l'argument
est le nombre 12, le script affiche `foobar` à la place.

Exemple:

```sh
$ ./foobar toto
toto
$ ./foobar 30
30
$ ./foobar 3
foo
$ ./foobar 4
bar
$ ./foobar 12
foobar
$ ./foobar foo
foo
```

* Utilisez `if` et `[` (ou `test`)

```sh
# Mettre dans le fichier script
#!/bin/bash
if [ "$1" = 3 ]; then
   echo "foo"
elif [ "$1" = 4 ]; then
   echo "bar"
elif [ "$1" = 12 ]; then
   echo "foobar"
else
   echo $1
fi
```

### Filtrage

Modifiez `foobar` de telle sorte que si l'argument n'est pas un nombre entier
positif, le programme affiche un message d'erreur sur la sortie standard
d'erreur et se termine avec un code de retour de 1.

Exemple:

```sh
$ ./foobar 12
foobar
$ echo $?
0
$ ./foobar toto
foobar: toto n'est pas un entier
$ echo $?
1
```

* Vous pouvez tester si l'argument est un nombre entier avec `grep` et une
  expression régulière.
  
  
```sh
# Mettre dans le fichier script
#!/bin/bash
if ! grep -Poq '^[0-9]+$' <<< $1; then
   echo "foobar: "$1" n'est pas un entier."
   exit 1
elif [ "$1" = 3 ]; then
   echo "foo"
elif [ "$1" = 4 ]; then
   echo "bar"
elif [ "$1" = 12 ]; then
   echo "foobar"
else
   echo $1
fi
```

### Développement arithmétique

Modifiez `foobar` pour que le programme affiche `foo` pour tous les multiples
de 3 (et plus seulement pour le nombre 3 lui-même), `bar` pour les multiples de
4 et `foobar` pour les multiples de 12.

Exemple:

```sh
$ ./foobar 50
50
$ ./foobar 3
foo
$ ./foobar 30
foo
$ ./foobar 8
bar
$ ./foobar 240
foobar
```

* Utilisez le développement arithmétique `$(())`
* Note: l'opérateur `%` (modulo) retourne le reste de la division entière.
  « `x%y` » vaut `0` si `x` est un multiple de `y`.

```sh
# Mettre dans le fichier script
#!/bin/bash
if ! grep -Poq '^[0-9]+$' <<< $1; then
   echo "foobar: "$1" n'est pas un entier."
   exit 1
elif [ $(($1%12)) = 0 ]; then
   echo "foobar"
elif [ $(($1%4)) = 0 ]; then
   echo "bar"
elif [ $(($1%3)) = 0 ]; then
   echo "foo"
else
   echo $1
fi
```
### Boucle `for`

Modifiez `foobar` pour qu'il traite chaque nombre entre 1 et l'argument.

Exemple:

```
$ ./foobar 13
1
2
foo
bar
5
foo
7
bar
foo
10
11
foobar
13
$ foobar toto
foobar: toto n'est pas un entier
```

* Utilisez une boucle `for`
* Utilisez la commande `seq` pour générer une séquence de nombres entiers

```sh
# Mettre dans le fichier script
#!/bin/bash
if ! grep -Poq '^[0-9]+$' <<< $1; then
   echo "foobar: "$1" n'est pas un entier."
   exit 1
fi
if [ "$1" = 0 ]; then
   echo $1
fi
for i in $(seq $1)
   do
		  if [ $(($i%12)) = 0 ]; then
			   echo "foobar"
			elif [ $(($i%4)) = 0 ]; then
				 echo "bar"
			elif [ $(($i%3)) = 0 ]; then
				 echo "foo"
			else
  		   echo $i
			fi
	 done
```

### Boucle `while` (avancé)

Remplacez la boucle `for` (et le `seq`) par une boucle `while` et utilisez le
développement arithmétique pour incrémenter une variable de boucle.

```sh
# Mettre dans le fichier script
#!/bin/bash
if ! grep -Poq '^[0-9]+$' <<< $1; then
	echo "foobar: "$1" n'est pas un entier."
	exit 1
fi
if [ "$1" = 0 ]; then
	echo $1
fi
n=1
while [ $n -le $1 ]
	do
	# echo "welcome £n"
		if [ $(( $n%12 )) = 0 ]; then
			echo "foobar"
		elif [ $(( $n%4 )) = 0 ]; then
			echo "bar"
		elif [ $(( $n%3 )) = 0 ]; then
			echo "foo"
		else
			echo $n
		fi
		((n++))
	done
```
