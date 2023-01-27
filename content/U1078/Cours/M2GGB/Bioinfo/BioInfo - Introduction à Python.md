---
title: "BioInfo - Introduction à Python"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

## Python

Python est un langage qui peut être
- Interprété (on tape une commande et elle est immédiatement executé). Pour cela il suffit de rentrer dans l'interpréteur python, en tapant `python3` (ctrl+D pour quitter)
- scripts. On ecrit dans un fichier une liste de commandes qui seront executées séquentiellement. Pour cela il faut tape `python3 LeNomDuFichier.py`

## Commandes

### Opérations Mathématiques simples
```python
3+6
```
```python
58-29
```
```python
4*8
```
```python
51/17
```

### Texte

```python
print("Bonjour je m'appelle XXXXXXX")
```

Concaténation avec ","
print("J'ai 27 ans. Ca fait",27*365.25,"jours")

## Variables

Une variable est un objet défini par un nom et qui contient une valeur.

L'affectation d'une valeur à une variable se fait avec `=`

```python
pi=3.141592
```
L'appelle à une variable est remplacé par sa valeur

```python
pi
```

```python
print("Ma variable contient la valeur",pi)
```

```python
rayon=17
hauteur=33
airebase=rayon*rayon*pi
volumecylindre=airebase*hauteur
print("Une cylindre de rayon",rayon,"et de hauteur",hauteur)
print("L'aire de sa base est",airebase)
print("Le volume du cylindre est",volumecylindre)
```

On peut réaffecter une nouvelle valeur à une variable, ce qui va écraser la précédente. 

```python
x=27
x
x=33
x
x=x+x
x
```

Avec `""` une variable peut également contenir du text

```python
name="Bond"
firstname="James"
print("My name is",name,firstname,name)
```

La valeur d'une variable peut être demandé à l'utilisateur interactivement avec `input("Un petit texte")`

```python
name=input("Bonjour, quel est ton nom ? ")
```
S'il s'agit d'un nombre, une consertion text -> number est requise avec `int()` pour un nombre entier ou `float()` pour un nombre à virgule
```python
age=int(input("Quel est ton age ? "))
```
Les variables sont bien mémorisées
```python
print("Enchanté",name,"En 2060 ans tu auras",age+2060-2022,"ans !")
```

## Programme 

Toutes les commandes peuvent être stockées dans un fichier NomDuFichier.py et executé sous la forme d'un programme avec python3 NomDuFichier.py

Exemple : Ouvrir un editeur de texte en tapant `gedit&`

Dedans copier les commandes que nous venons de voir:

```python
name=input("Bonjour, quel est ton nom ? ")
age=int(input("Quel est ton age ? "))
print("Enchanté",name,"En 2060 ans tu auras",age+2060-2022,"ans !")
```
Cliquer sur "save" et enregistrer le fichier sous `welcome.py`

Taper la commande suivante pour executer votre programme : `python3 welcome.py`

:::success
**Exercice 1 Volume** 
Ecrire un programme qui demande
- la longueur
- la largeur
- la hauteur

d'une boite et affiche son volume.
:::

## Conditionnelles

Les conditionnelles permettent d'executer certaines commandes d'un programme que sous certaines conditions.

La syntaxe est la suivante :warning: l'indentation (les espaces en début de lignes) est importante en python

```python
if CONDITION:
    code executé
    uniquement si
    CONDITION est vrai
```

ou

```python
if CONDITION:
    code executé
    uniquement si
    CONDITION est vrai
else:
    code executé si 
    CONDITION est faux
```

### Les opérateurs de comparaisons

Comme `=` est utilisé pour l'affectation, écrire `a=b` va remplacer la valeur de `a` par celle de `b`. Voici quelques opérateurs
- Equals: `a == b`
- Not Equals: `a != b`
- Less than: `a < b`
- Less than or equal to: `a <= b`
- Greater than: `a > b`
- Greater than or equal to: `a >= b`

Dans l'interpreteur python, taper:
```
a=13
b=24
a==b
a!=b
a<b
a<=b
a>b
a>=b
```

Python répond `True` que l'opération est vrai et `False` quand elle est fausse

Dans **gedit** 
Ecrire le programme suivant
```python
a=int(input("Valeur pour a:"))
b=int(input("Valeur pour b:"))

if a==b:
    print("a et b sont égaux. Leur valeur est",a)
else:
    print("a et b sont différents")
```

Enregistrer le programme sous le nom `if.py` et executer le 2 fois (avec des valeurs identiques et différentes)

## Boucles

## Fonctions