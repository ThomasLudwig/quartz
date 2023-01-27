---
title: "BioInfo TP1 - Unix & Linux"
tags:
- m2ggb
- epidemiologie
---

###### tags #m2ggb #epidemiologie
*contact :* ludwig@univ-brest.fr 

## 1. Introduction

R est un système d’analyse statistique et graphique créé par Ross Ihaka et Robert Gentleman. R est à la fois un logiciel et un langage qualifié de dialecte du langage S créé par AT&T Bell Laboratories. Il y a des différences importantes dans la conception de R et celle de S.

R est distribué librement sous les termes de la GNU General Public Licence; son développement et sa distribution sont assurés par plusieurs statisticiens rassemblés dans le R Development Core Team.

R comporte de nombreuses fonctions pour les analyses statistiques et les graphiques; ceux-ci sont visualisés immédiatement dans une fenêtre propre et peuvent être exportés sous divers formats (jpg, png, bmp, ps, pdf, emf, pictex, xfig ; les formats disponibles peuvent dépendre du système d’exploitation). Les résultats des analyses statistiques sont affichés à l’écran, certains résultats partiels (valeurs de P, coefficients de régression, résidus, . . .) peuvent être sauvés à part, exportés dans un fichier ou utilisés dans des analyses ultérieures.

Le langage R permet, par exemple, de programmer des boucles qui vont analyser successivement différents jeux de données. Il est aussi possible de combiner dans le même programme différentes fonctions statistiques pour réaliser des analyses plus complexes. Les utilisateurs de R peuvent bénéficier des nombreux programmes écrits pour S et disponibles sur internet, la plupart de ces programmes étant directement utilisables avec R.

De prime abord, R peut sembler trop complexe pour une utilisation par un non-spécialiste. Ce n’est pas forcément le cas. En fait, R privilégie la flexibilité. Alors qu’un logiciel classique affichera directement les résultats d’une analyse, avec R ces résultats sont stockés dans un “objet”, si bien qu’une analyse peut être faite sans qu’aucun résultat ne soit affiché. L’utilisateur peut être déconcerté par ceci, mais cette facilité se révèle extrêmement utile. En effet, l’utilisateur peut alors extraire uniquement la partie des résultats qui l’intéresse. Par exemple, si l’on doit faire une série de 20 régressions et que l’on veuille comparer les coefficients des différentes régressions, R pourra afficher uniquement les coefficients estimés : les résultats tiendront donc sur une ligne, alors qu’un logiciel plus classique pourra ouvrir 20 fenêtres de résultats. On verra d’autres exemples illustrant la flexibilité d’un système comme R vis-à-vis des logiciels classiques.

## 2. Prise en main

### 2.1. Lancer R

```bash
R
```

### 2.2. Quitter R

```r
> q()
```

## 3. Fonctionnement de R
R est un langage interprété, c'est-à-dire que les commandes tapées au clavier sont directement exécutées sans qu'il soit nécessaire de construire un programme.

La syntaxe de R se veut simple et intuitive. Avec R, une fonction, pour être exécutée, s'écrit toujours avec des parenthèses, même si elle ne contiennent pas d'argument (exemple `ls()`). Si l'utilisateur tape le nom de la fonction sans parenthèses, R affichera le contenu des instructions de cette fonction. Ex:

```r
> plot
function (x, y, ...)
UseMethod("plot")
<bytecode: 0x2eefdd0>
<environment: namespace:graphics>
```
Quand R est utilisé, les variables, les données, les fonctions, les résultats, etc., sont stockés dans la mémoire de l’ordinateur sous forme d’*objets* qui ont chacun un *nom*. L’utilisateur va agir sur ces objets avec des opérateurs (arithmétiques, logiques, de comparaison, ...) et des *fonctions* (qui sont elles-mêmes des objets).

### 3.1 Opérations de base
L'affectation d'une valeur à un objet peut se faire de plusieurs façon équivalente
```r
> a <- 1
> 2 -> b
> d = 3
```

pour afficher le contenu d'un objet on tape simplement son nom 
```r
> a
[1] 1
> b
[1] 2
> d
[1] 3
```
Si l'objet existe déjà, sa valeur précédente est effacée.
```r
> n <- 5 + 7
> n
[1] 12
> n <- 6 * 8
> n
[1] 48
```
La fonction `ls()` donne la liste des objets en mémoire
```r
> ls()
[1] "a" "b" "d" "n"
```
Pour obtenir de l'aide, on utilise `?` ou `help()`
```r
> ?ls
> help("ls")
```

## 4. Les objets

### 4.1. Simples Valeurs

R manipule des objets : ceux-ci sont caractérisés par leur nom et leur contenu, mais aussi par des *attributs* qui vont spécifier le type de données représenté par un objet. Considérons une variable qui prendrait les valeurs 1, 2 ou 3 : une telle variable peut représenter une variable entière (par exemple, le nombre d’oeufs dans un nid), ou le codage d’une variable catégorique (par exemple, le sexe de crustacés : 1=mâle, 2=femelle ou 3=hermaphrodite).

Le traitement de cette variable ne sera pas le même dans les deux cas : les attributs de l’objet donnent l’information nécessaire. Plus techniquement, l’action d’une fonction sur un objet va dépendre des attributs de celui-ci.

Les objets ont tous deux attributs : le *mode* et la *longueur*. Le mode est le type des éléments d’un objet; il en existe quatre principaux :
- numérique
- caractère
- complexe
- logique (`FALSE` ou `TRUE`). 

D’autres modes existent qui ne représentent pas des données, par exemple *fonction* ou *expression*.
La longueur est le nombre d’éléments de l’objet. Pour connaître le mode et la longueur d’un objet on peut utiliser, respectivement, les fonctions `mode` et `length` :

```r
> x <- 1
> mode(x)
[1] "numeric"
> length(x)
[1] 1
> text="May the force be with you"
> mode(text)
[1] "character"
> length(text)
[1] 1
> test=TRUE
> mode(test)
[1] "logical"
> length(test)
[1] 1
> cmp=2i+1
> mode(cmp)
[1] "complex"
> length(cmp)
[1] 1
```

Dans tous ces exemple, la longueur est toujours 1. Mais R peut également manipuler des objets plus complexes, comme des vecteurs.
```r
> v <- c(17,-2.3)
> v
[1] 17.0 -2.3
> mode(v)
[1] "numeric"
> length(v)
[1] 2
```
Quelque soit le mode, les valeurs manquantes sont représentées par `NA` (*not available*).  Une valeur numérique très grande peut être spécifiée avec une notation exponentielle. Les valeurs $\pm\infty$ sont représentées par `Inf` et `-Inf`. Les résultats qui ne sont pas des nombres valides sont représentés par `NaN` (*Not a Number*).
```r
> N <- 2.1e23
> N
[1] 2.1e+23

> x <- 5/0
> x
[1] Inf

> exp(x)
[1] Inf

> exp(-x)
[1] 0

> x - x
[1] NaN
```

### 4.2. Vecteurs

On crée un vecteur avec la fonction `c` (*concatenate*).
```r
> c(1 ,2.6, 47, 98, 44.4)
[1]  1.0  2.6 47.0 98.0 44.4
```

il est possible d'isoler une ou plusieurs valeurs d'un vecteur avec `[]`
```r
> c <- c(1 ,2.6, 47, 98, 44.4)
> c[2]
[1] 2.6
> c[1] <- 1.1
> c
[1]  1.1  2.6 47.0 98.0 44.4
> c + 2
```

il est aussi possible de concaténer deux vecteurs
```r
> a <- c(1,3)
> b <- c(3,7)
> c(a,b)
[1] 1 3 3 7
> c(a,2:8,b)
[1] 1 3 2 3 4 5 6 7 8 3 7
```

### 4.3. Matrices

Les matrices peuvent être créées avec la fonction matrix:
```r
> A <- matrix( 1:10, ncol=5)
> A
[,1] [,2] [,3] [,4] [,5]
[1,] 1 3 5 7 9
[2,] 2 4 6 8 10
```

Comme pour les vecteurs, il est possible d’extraire une valeur, une ligne ou une colonne avec `[]`.
```r
> A[1,4]
[1] 7
> A[2, , drop=FALSE]
[,1] [,2] [,3] [,4] [,5]
[1,] 2 4 6 8 10
```

> [!info]
> :pencil: l’option `drop=FALSE` permet de garder le résultat sous forme de matrice et non de vecteur.

### 4.4. Listes

les listes d'objets peuvent être créées avec la fonction `list()`
```r
> l <- list(Prenom=c("Ophelie", "Annabelle"), Nom="Dupont")
> l
$Prenom
[1] "Ophelie" "Annabelle"
$Nom
[1] "Dupont"
```

les objets contenus dans la liste peuvent être extraits avec `$`.
```r
> l$Prenom
[1] "Ophelie" "Annabelle"
```

## 4.5. Dataframes

Un DataFrame est une liste particulière.
- c'est une liste de vecteurs de données
- ces vecteurs sont de même longueur
- ces vecteurs peuvent être nommés
- les lignes peuvent également être nommées
- on peut manipuler un dataframe comme une matrice, par indices `[ligne, colonne]`

```r
> annee <- c(1995,2001,1987,2000)
> taille <- c(172,153,190,168)
> sexe <- c("M","F","M",NA)
> frame <- data.frame(list(annee, taille, sexe))
> colnames(frame) <- c("Année","Taille","Sexe")
> rownames(frame) <- c("Eric", "Diane", "Michael", "Alex")
```

on peut obtenir des informations sur le data frame

```r
> frame
        Année Taille Sexe
Eric     1995    172    M
Diane    2001    153    F
Michael  1987    190    M
Alex     2000    168 <NA>
> class(frame)
[1] "data.frame"
> summary(frame)
     Année          Taille        Sexe
 Min.   :1987   Min.   :153.0   F   :1
 1st Qu.:1993   1st Qu.:164.2   M   :2
 Median :1998   Median :170.0   NA's:1
 Mean   :1996   Mean   :170.8
 3rd Qu.:2000   3rd Qu.:176.5
 Max.   :2001   Max.   :190.0
```

Accès aux variables

```r
> frame$Année #par nom
> frame[[1]] #format liste
> frame[,1] #format matrice
> frame[["Année"]]
> frame[,"Année"]
[1] 1995 2001 1987 2000

> frame[1] # par num.col
> frame["Année"]
        Année
Eric     1995
Diane    2001
Michael  1987
Alex     2000
```

Accès aux individus

```r
> frame[1,]
     Année Taille Sexe
Eric  1995    172    M
> frame[c(1,3),]
        Année Taille Sexe
Eric     1995    172    M
Michael  1987    190    M
> frame["Diane",]
      Année Taille Sexe
Diane  2001    153    F
> frame[c(1,3),2:3]
        Taille Sexe
Eric       172    M
Michael    190    M
> frame[c("Eric","Diane"), c("Année","Sexe")]
      Année Sexe
Eric   1995    M
Diane  2001    F
```

## 5. Lire des données dans un fichier

Pour les lectures et écritures dans les fichiers, R utilise le *répertoire de travail*. Pour connaître ce répertoire, on peut utiliser la commande `getwd()` (*get working dirctory*). Il est possible de changer ce répertoire avec `setwd("/NOUVEAU/REPERTOIRE")`.

R peut lire des données stockés dans des fichiers texte à l'aide de la fonction `read.table()`

La fonction `read.table()` a pour effet de créer un Data Frame et est donc le moyen principal pour lire des fichiers de données. Par exemple, si on a un fichier nommé *data.dat*, la commande 
```r
> mydata <- read.table("data.dat")
```
créera un tableau de données nommé *mydata*, et les variables, par défaut nommés *V1*,*V2*,...,*Vn*, pourront être accédées individuellement par
- `mydata$V1`, `mydata$V2`,...,`mydata$Vn` ou 
- `mydata["V1"]`, `mydata["V2"]`,..., `mydata["Vn"]`, ou encore 
- `mydata[, 1]`, `mydata[, 2]`,..., `mydata[, n]`.

Il y a plusieurs options dont voici les valeurs par défaut (c'est à dire celles utilisés par R si elles sont omises par l'utilisateur).
```r
read.table(file, header = FALSE, sep = "", quote = "'", dec = ".",
	row.names, col.names, as.is = FALSE, na.strings = "NA",
	colClasses = NA, nrows = -1,
	skip = 0, check.names = TRUE, fill = !blank.lines.skip,
	strip.white = FALSE, blank.lines.skip = TRUE,
	comment.char = "#")
```
| Option | Description |
|---|---|
| `file` | le nom du fichier (entre `""` ou une variable de mode caractère), éventuellement avec son chemin d’accès (le symbole `\` est interdit et doit être remplacé par `/`, même sous Windows), ou un accès distant à un fichier de type URL (`http://...`) |
| `header` | une valeur logique (`FALSE` ou `TRUE` indiquant si le fichier contient les noms des variables sur la 1$^{e}$ ligne |
| `sep` | le séparateur de champ dans le fichier, par exemple `sep="\t"` si c’est une tabulation |
| `quote` | les caractères utilisés pour citer les variables de mode caractère |
| `dec` | le caractère utilisé pour les décimales |
| `row.names` | un vecteur contenant les noms des lignes qui peut être un vecteur de mode character, ou le numéro (ou le nom) d’une variable du fichier (par défaut : `1`, `2`, `3`, ...) |
| `col.names` | un vecteur contenant les noms des variables (par défaut : `V1`, `V2`, `V3`, ...) |
| `as.is` | contrôle la conversion des variables caractères en facteur (si `FALSE`) ou les conserve en caractères (si `TRUE`) ; `as.is` peut être un vecteur logique, numérique ou caractère précisant les variables conservées en caractère |
| `na.strings` | indique la valeur des données manquantes (sera converti en `NA`) |
| `colClasses` | un vecteur de caractères donnant les classes à attribuer aux colonnes |
| `nrows` | le nombre maximum de lignes à lire (les valeurs négatives sont ignorées) |
| `skip` | le nombre de lignes à sauter avant de commencer la lecture des données |
| `check.names` | si `TRUE`, vérifie que les noms des variables sont valides pour R |
| `fill` | si `TRUE` et que les lignes n’ont pas tous le même nombre de variables, des *blancs* sont ajoutés |
| `strip.white` | (conditionnel à `sep`) si `TRUE`, efface les espaces (= blancs) avant et après les variables de mode caractère |
| `blank.lines.skip` | si `TRUE`, ignore les lignes *blanches* |
| `comment.char` | un caractère qui définit des commentaires dans le fichier de données, la lecture des données passant à la ligne suivante (pour désactiver cet option, utiliser `comment.char = ""`) |

## 6. Enregistrer les données dans un fichier

La fonction `write.table` écrit dans un fichier un objet, typiquement un tableau de données, mais cela peut bien être un autre type d'objet (vecteur, matrice, ...). Les arguments et options sont : 
```r
write.table(x, file = "", append = FALSE, quote = TRUE, sep = " ",
	eol = "\n", na = "NA", dec = ".", row.names = TRUE,
	col.names = TRUE, qmethod = c("escape", "double"))
```
| Option | Description |
|---|---|
| `x` | le nom de l’objet  écrire |
| `file` | le nom du fichier (par défaut l’objet est affiché à l’écran) |
| `append` | si `TRUE` ajoute les données sans effacer celles éventuellement existantes dans le fichier |
| `quote` | une variable logique ou un vecteur numérique : si `TRUE` les variables de mode caractère et les facteurs sont écrits entre `""`, sinon le vecteur indique les numéros des variables à écrire entre `""` (dans les deux cas les noms des variables sont écrits entre `""` mais pas si `quote = FALSE`) |
| `sep` | le séparateur de champ dans le fichier |
| `eol` | le caractère imprimé à la fin de chaque ligne (`"\n"` correspond à un retour chariot) |
| `na` | indique le caractère utilisé pour les données manquantes |
| `dec` | le caractère utilisé pour les décimales |
| `row.names` | une variable logique indiquant si les noms des lignes doivent être écrits dans le fichier |
| `col.names` | idem pour les noms des colonnes |
| `qmethod` | spécifie, si `quote=TRUE`, comment sont traitées les guillemets doubles `"` incluses dans les variables de mode caractère : si `"escape"` (ou `"e"`, le défaut) chaque `"` est remplacée par `\"`, si `"d"` chaque `"` est remplacée par `""` |

Pour écrire de façon plus simple un objet dans un fichier, on peut utiliser la commande `write.table(x, file="data.txt")` où `x` est le nom de l'objet.

## 7. Générer des données

### 7.1. Séquences régulières

Tous les nombres de 1 à 30
```r
> x <- 1:30
```

On a ainsi un vecteur `x` avec 30 éléments.
L'opérateur `:` est prioritaire sur les opérations arithmétiques au sein d'une expression : 
```r
> 1:10-1
[1] 0 1 2 3 4 5 6 7 8 9
> 1:(10-1)
[1] 1 2 3 4 5 6 7 8 9
```

La fonction `seq` peut générer des séquences de nombres réels de la manière suivante ;
```r
> seq(1, 5, 0.5)
[1] 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0
```
où le premier nombre indique le début de la séquence, le second la fin, et le troisième l'incrément.

La fonction `rep` crée un vecteur qui aura tous ses éléments identiques :
```r
> rep(1, 30)
[1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```
<!--   
La fonction `sequence` va créer une suite de séquences de nombres entières qui chacune se termine par les nombres donnés comme arguments :
```r
> sequence(4:5)
[1] 1 2 3 4 1 2 3 4 5
> sequence(c(10,5))
[1] 1 2 3 4 5 6 7 8 9 10 1 2 3 4 5
```
-->
### 7.2. Séquences aléatoires

Il est utile en statistique de pouvoir générer des données aléatoires, et R peut le faire pour un grand nombre de fonctions de densité de probabilité. Ces fonctions sont de la form `rfunc(n, p1, p2, ..., pn`), où `func` indique la loi de probabilité, `n` le nombre de données générées et `p1, p2, ..., pn` sont les valeurs des paramètres de la loi.

| loi | fonction |
|---|---|
| *Gauss* (normale) | `rnorm(n, mean=0, sd=1)` |
| exponentielle | `rexp(n, rate=1)` |
| gamma | `rgamma(n, shape, scale=1)` |
| *Poisson* | `rpois(n, lambda)` |
| *Weibull* | `rweibull(n, shape, scale=1)` |
| *Cauchy* | `rcauchy(n, location=0, scale=1)` |
| beta | `rbeta(n, shape1, shape2)` |
| *Student* (t) | `rt(n, df)` |
| *Fisher–Snedecor* (F) | `rf(n, df1, df2)` |
| *Pearson* ($\chi^2$) | `rchisq(n, df)` |
| binomiale | `rbinom(n, size, prob)` |
| multinomiale | `rmultinom(n, size, prob)` |
| géométrique | `rgeom(n, prob)` |
| hypergéométrique | `rhyper(nn, m, n, k)` |
| logistique | `rlogis(n, location=0, scale=1)` |
| lognormale | `rlnorm(n, meanlog=0, sdlog=1)` |
| binomiale négative | `rnbinom(n, size, prob)` |
| uniforme | `runif(n, min=0, max=1)` |
| statistiques de Wilcoxon | `rwilcox(nn, m, n)`, `rsignrank(nn, n)` |

## 8. Merge

La fonction `merge()` permet de joindre deux tables sur une clé (colonne) identique. Par défaut, la fonction renvoie l'intersection des deux tables, mais il est également possible d'obtenir l'union des tables, ou de compléter les données d'une des tables.
```r
> t1 <- read.table("/home/tludwig/master2ggb/table1.tsv", header=TRUE)
> t1
         Nom Age Sexe
1     Emilie  23    F
2       Jean  21    H
3 Christophe  23    H
4      Marie  21    F
5     Pierre  25    H
6     Sophie  24    F

> t2 <- read.table("/home/tludwig/master2ggb/table2.tsv", header=TRUE)
> t2
   Prenom    Ville
1   Aline   Angers
2    Jean    Brest
3 Caradoc   Vannes
4   Marie    Brest
5  Sophie Bordeaux
6  Marcel Toulouse

> merge(t1,t2,by.x="Nom",by.y="Prenom")
     Nom Age Sexe    Ville
1   Jean  21    H    Brest
2  Marie  21    F    Brest
3 Sophie  24    F Bordeaux

> merge(t1,t2,by.x="Nom",by.y="Prenom",all=TRUE)
         Nom Age Sexe    Ville
1 Christophe  23    H     <NA>
2     Emilie  23    F     <NA>
3       Jean  21    H    Brest
4      Marie  21    F    Brest
5     Pierre  25    H     <NA>
6     Sophie  24    F Bordeaux
7      Aline  NA <NA>   Angers
8    Caradoc  NA <NA>   Vannes
9     Marcel  NA <NA> Toulouse

> merge(t1,t2,by.x="Nom",by.y="Prenom",all.x=TRUE)
         Nom Age Sexe    Ville
1 Christophe  23    H     <NA>
2     Emilie  23    F     <NA>
3       Jean  21    H    Brest
4      Marie  21    F    Brest
5     Pierre  25    H     <NA>
6     Sophie  24    F Bordeaux


> merge(t1,t2,by.x="Nom",by.y="Prenom",all.y=TRUE)
      Nom Age Sexe    Ville
1    Jean  21    H    Brest
2   Marie  21    F    Brest
3  Sophie  24    F Bordeaux
4   Aline  NA <NA>   Angers
5 Caradoc  NA <NA>   Vannes
6  Marcel  NA <NA> Toulouse

```

## 9. Selection dans un vecteur

Il est possible de filtrer un tableau pour ne garder que certaines valeurs.
```r
> t1
         Nom Age Sexe
1     Emilie  23    F
2       Jean  21    H
3 Christophe  23    H
4      Marie  21    F
5     Pierre  25    H
6     Sophie  24    F

> subset(t1, t1$Sexe == "F")
     Nom Age Sexe
1 Emilie  23    F
4  Marie  21    F
6 Sophie  24    F

> t1[t1$Age < 24, "Age"]
[1] 23 21 23 21
```
## 10. Statistiques

R est un langage de programmation avant tout utilisé pour sa puissance dans le domaine des statistiques.

Nous verrons quelques fonctions de base dans ce domaine.
```r
> v <- (sample.int(101,size=100,replace=TRUE)-1)/100 # 100 nombre entre 0 et 1 arondi à 2 décimales
> max(v)
[1] 1
> min(v)
[1] 0
> mean(v)
[1] 0.5852
> median(v)
[1] 0.65
> quantile(v, .33)
   33%
0.4567
> quantile(v, c(.1,.2,.3,.4,.5,.6,.7,.8,.9))
  10%   20%   30%   40%   50%   60%   70%   80%   90%
0.139 0.332 0.440 0.536 0.650 0.718 0.796 0.852 0.931
> sum(v)
[1] 58.52
> sum(v >= 0.75) # combien de valeur >= 0.75 ?
[1] 37
> var(v) #variance
[1] 0.08217067
> sd(v) # standard deviation
[1] 0.2866543
> summary(v)
   Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
 0.0000  0.3925  0.6500  0.5852  0.8400  1.0000
> library(Hmisc)
> describe(v)
       n  missing distinct     Info     Mean      Gmd      .05      .10
     100        0       61        1   0.5852   0.3277   0.0890   0.1390
     .25      .50      .75      .90      .95
  0.3925   0.6500   0.8400   0.9310   0.9600
lowest : 0.00 0.02 0.03 0.07 0.09, highest: 0.95 0.96 0.97 0.98 1.00

> table(v) # valeurs et nombre d'occurrences

# en regardant les sortie de v et diff(v), que fait la fonction diff ?
> v
> diff(v) 
```

## 11. Graphiques

R propose plusieurs fonctions pour généré des graphiques.

### 11.1. Exemple

```r
> age <- read.table("/home/tludwig/master2ggb/age.tsv")
> plot(age$V1)
> hist(age$V1)
> hist(age$V1, breaks=seq(0,330,by=30) )
> hist(age$V1, breaks=seq(0,330,by=15) )
> data=c(1,3,6,4,9)
> colors=c("red","yellow","green","blue","purple")
> cat=c("Bad","Warning","Good","OK","Unknown")
> pie(data,main="Show", col=colors, labels=data, cex=0.8)
> legend(-1.5,0.5, cat, cex=0.8, fill=colors)
> boxplot(sample.int(101,size=100,replace=TRUE)-1) # boxplot de 100 valeurs au hasard entre 0 et 100
```

### 11.2. Export d'un graphique dans une image

```r
> age <- read.table("/home/tludwig/master2ggb/age.tsv")
> png(filename = "monhistogramme.png", bg = "white");
> hist(age$V1, breaks=seq(0,330,by=15) )
> dev.off();
```

![](http://lysine.univ-brest.fr/~tludwig/graphR.png)

### 11.3. Fonctions graphiques

| Fonction | Description |
|---|---|
| `plot(x)` | graphe des valeurs de `x` (sur l’axe des `y`) ordonnées sur l’axe des `x` |
| `plot(x, y)` | graphe bivarié de `x` (sur l’axe des `x`) et `y` (sur l’axe des `y`) |
| `sunflowerplot(x,y)` | idem que `plot()` mais les points superposés sont dessinés en forme de fleurs dont le nombre de pétales représente le nombre de points |
| `pie(x)` | graphe en camembert |
| `boxplot(x)` | graphe boites et moustaches |
| `stripchart(x)` | graphe des valeurs de `x` sur une ligne (une alternative à `boxplot()` pour des petits échantillons) |
| `coplot(x~y \| z)` | graphe bivarié de `x` et `y` pour chaque valeur (ou intervalle de valeurs) de `z` |
| `interaction.plot(f1,f2,y)` | si `f1` et `f2` sont des facteurs, graphe des moyennes de `y` (sur l’axe des `y`) en fonction des valeurs de `f1` (sur l’axe des `x`) et de `f2` (différentes courbes) ; l’option `fun` permet de choisir la statistique résumée de `y` (par défaut `fun=mean`) |
| `matplot(x,y)` | graphe bivarié de la 1$^{ère}$ colonne de `x` contre la 1$^{ère}$ de `y`, la 2$^{ème}$ de `x` contre la 2$^{ème}$ de `y`, etc. |
| `dotchart(x)` | si `x` est un tableau de données, dessine un graphe de Cleveland (graphes superposés ligne par ligne et colonne par colonne) |
| `fourfoldplot(x)` | visualise, avec des quarts de cercles, l’association entre deux variables dichotomiques pour différentes populations (`x` doit être un tableau avec `dim=c(2, 2, k)` ou une matrice avec `dim=c(2, 2)` si `k = 1`) |
| `assocplot(x)` | graphe de Cohen–Friendly indiquant les déviations de l’hypothèse d’indépendance des lignes et des colonnes dans un tableau de contingence à deux dimensions |
| `mosaicplot(x)` | graphe en *mosaïque* des résidus d’une régression log-linéaire sur une table de contingence |
| `pairs(x)` | si `x` est une matrice ou un tableau de données, dessine tous les graphes bivariés entre les colonnes de `x` |
| `plot.ts(x)` | si `x` est un objet de classe `"ts"`, graphe de `x` en fonction du temps, `x` peut être multivarié mais les séries doivent avoir les mêmes fréquence et dates |
| `ts.plot(x)` | idem mais si `x` est multivarié les séries peuvent avoir des dates différentes et doivent avoir la même fréquence |
| `hist(x)` | histogramme des fréquences de `x` |
| `barplot(x)` | histogramme des valeurs de `x` |
| `qqnorm(x)` | quantiles de `x` en fonction des valeurs attendues selon une loi normale |
| `qqplot(x, y)` | quantiles de `y` en fonction des quantiles de `x` |
| `contour(x, y, z)` | courbes de niveau (les données sont interpolées pour tracer les courbes), `x` et `y` doivent être des vecteurs et `z` une matrice telle que `dim(z)=c(length(x), length(y))` (`x` et `y` peuvent être omis) |
| `filled.contour (x, y, z)` | idem mais les aires entre les contours sont colorées, et une légende des couleurs est également dessinée |
| `image(x, y, z)` | idem mais les données sont représentées avec des couleurs |
| `persp(x, y, z)` | idem mais en perspective |
| `stars(x)` | si `x` est une matrice ou un tableau de données, dessine un graphe en segments ou en étoile où chaque ligne de `x` est représentée par une étoile et les colonnes par les longueurs des branches |
| `symbols(x, y, ...)` | dessine aux coordonnées données par `x` et `y` des symboles (cercles, carrés, rectangles, étoiles, thermomètres ou *boxplots*) dont les tailles, couleurs, etc, sont spécifiées par des arguments supplémentaires |
| `termplot(mod.obj)` | graphe des effets (partiels) d’un modèle de régression (`mod.obj`) |

## 12. Commandes du système d'exploitation

Il est possible de lancer, depuis R, des commandes appartenant au système d'exploitation (ici linux).

Cela peut-être utile en particulier pour demander l'exécution d'un programme externe, qui va lire/modifier/créer des fichiers dont les données pourront ensuite être exploitées par R. 
```r
> ta=read.table("generated.tsv") # erreur, le fichier n'existe pas
> system("/home/tludwig/master2ggb/generate_data.sh") #execution d'un programme qui genere des donnees
> ta=read.table("generated.tsv") #maintenant le fichier existe
> ta
```

Il est également possible de récuperer directement le résultat d'une commande.
```r
> existing=system("ls /home", intern=TRUE) #quels utilisateurs existent sur ce serveur ?
> existing
> loggedin=system("who | cut -d' ' -f1 | sort -u", intern=TRUE) #quels utilisateurs sont actuellement presents ?
> loggedin
```

## 13. Import

Pour ne pas avoir à retaper sans cesse les mêmes commandes et réécrire les mêmes fonctions, il est possible de les stocker dans des fichiers (monfichier.R) et de les charger en cas de besoin.

Dans ces fichiers, on pourra mettre des scripts (successions de commandes exécutées automatiquement) ou des fonctions que l'on a défini nous même et qui pourront être appelées quand nécessaire.

Il est également possible d'importer des librairies (ensemble de fichiers, publiquement disponibles) dans R.

Pour importer un fichier R, on utilise la fonction `source()`, tandis que pour importer des librairies, on utilise `library()`

```r
> library(ggplot2)
> source("/home/tludwig/master2ggb/script.R") #chargement d'un script, qui s'execute
> d<-getdata() #Error: could not find function "getdata"
> source("/home/tludwig/master2ggb/functions.R") #chargement du fichier qui contient la fonction
> d<-getdata() #maintenant la fonction est connue
> d
```

## 14. which

Pour un vecteur logique (qui contient des valeurs `TRUE`/`FALSE`), la commande which donne les indices du vecteurs dont la valeur est `TRUE` :

```r
> LETTERS #un vecteur qui contient chaque lettre de l'alphabet
> LETTERS == "U" #un vecteur qui contient TRUE si la lettre est "U"
> which(LETTERS == "U") #quelle est la position de U dans l'alphabet

> 1:150 #un vecteur qui contient les valeurs entre 1 et 150
> 1:150 %% 17 == 0 #un vecteur qui contient TRUE si la valeur est multiple de 17
> which(1:150 %% 17 == 0) #les multiples de 17 entre 1 et 150
```