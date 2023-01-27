---
title: "Bioinfo - Programmer en AWK"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

> [!success] ***AWK***
> ***AWK*** est un langage pour traiter des fichiers *textes*, ligne-par-ligne. La ligne peut être séparée en *champs* (colonnes ou mots).

Prennez les fichiers d'entrées

```bash
mkdir ~/awk
cd ~/awk
cp /home/tludwig/master2ggb/tableawk.tsv .
cp /home/tludwig/s.omalleysbar.txt .
wget http://lysine.univ-brest.fr/~tludwig/master2/tp3/1.variants.vcf
```

### 1. Un programme simple

Affiche chaque ligne en entrée, sans modification:

```bash
cat tableawk.tsv | awk '{print $0}'
``` 

> [!info]
> - `print` est la fonction qui permet d'afficher du texte.
> - `$0` signifie *toute la ligne*

### 2. Accéder à un champ 

```bash
cat tableawk.tsv | awk '{print $2}'
``` 

> [!info]
> - `$i` signifie *la i$^{e}$ colonne*

> [!warning]  les espaces et tabulations ne se mélangent pas bien

### 3. Quelques variables globales

```bash
cat s.omalleysbar.txt | awk '{print NF}' | sort -n | uniq -c
cat tableawk.tsv | awk '{print NR": "$0}'
```

> [!info]
> - `NF` (*Number of Fields*) indique le nombre de colonne dans la ligne actuelle
> - `NR` (*Number of Reccords*) indique le nombre de ligne lu jusqu'à présent

### 4. Utiliser les variables

```bash
cat s.omalleysbar.txt | awk '{print $NF}' | head
```

> [!info]
>  `$NF` signifie la dernière colonne (c'est-à-dire la NF$^{e}$ colonne) 

### 5. BEGIN / END

- La section `BEGIN{}` est exécutée intégralement avant de lire la première ligne.
- La section `END{}` est exécutée intégralement après avec traité la dernière ligne.

```bash
cat tableawk.tsv | awk '
    BEGIN{
        print "Lets-a-gooo"
    }
    {
        print $0
    }
    END{
        print "Il y avait "NR" lignes dans ce fichier."
    }'
``` 

### 6. Multiples instructions

Les instructions sont séparées par des point-virgules `;`

```bash
cat tableawk.tsv | awk '
    {
        print NR; 
        print $0
    }'
```

### 7. Conditionnelles

- `if(condition){block}` permet d'exécuter `block` si `condition` est vrai

```bash
cat tableawk.tsv | awk '
    {
        if($0 ~ "^#"){
            print "il y a "NF" colonnes"
        }
    }'
```

> [!info]
>  `~ "^#"` indique les lignes qui commencent par `#`

- `else{block2}` permet d'exécuter `block2` quand `condition` est faux

```bash
cat tableawk.tsv | awk '
    {
        if($0 ~ "^#"){
            print "il y a "NF" colonnes"
        } 
        else{
            print $0
        }
    }'
```

On ne garde que les filles (et l'en-tête)

```bash
cat tableawk.tsv | awk '
    {
        if($0 ~ "^#" || $3 == "F"){
            print $0
        }
    }'
```


### 8. Définir et utiliser des variables

- `nom_de_variable = 18` ou `nom_de_variable = "Texte"` permet d'assigner une valeur à une variable

```bash
cat tableawk.tsv | awk '
    BEGIN{
        male=0
    }
    {
        if($3 == "M")
        {
            male=male+1
        }
    }
    END{
        print "There are "male" men"
    }'
```

> [!warning] Ne pas confondre
> - `a = 2` $\rightarrow$ affecte la valeur 2 à la variant `a` 
> - `b == 3` $\rightarrow$ renvoie `TRUE` si la variable `b` est égale à 3 

On peut utiliser une variable dans un calcul

```bash
cat tableawk.tsv | awk '
    BEGIN{
        height=0
    }
    {
        if($0 !~ "^#"){
            height=height+$4
        }
    }
    END{
        mean=height/(NR-1); 
        print "mean height: "mean
    }'
```

### 9. Tableaux

- `x[]` indique un tableau du nom de `x`

```bash
cat tableawk.tsv | awk '
    BEGIN{
        height["M"]=0;
        height["F"]=0;
        nb["M"]=0;
        nb["F"]=0;
    }
    {
        height[$3]=height[$3]+$4;
        nb[$3] = nb[$3] + 1;
    }
    END{
        mean["M"]=height["M"]/nb["M"]; 
        mean["F"]=height["F"]/nb["F"]; 
        print "mean height.";
        print "for men "mean["M"];
        print "for women "mean["F"];
    }'
```

- Les *cases vides* des tableaux contiennent 0. On peut donc simplifier en 

```bash
cat tableawk.tsv | awk '
    {
        height[$3]=height[$3]+$4;
        nb[$3] = nb[$3] + 1;
    }
    END{
        mean["M"]=height["M"]/nb["M"]; 
        mean["F"]=height["F"]/nb["F"]; 
        print "mean height.";
        print "for men "mean["M"];
        print "for women "mean["F"];
    }'
```


### 10. Parcourir un tableau

- `for (variable in tableau){block}` permet d'affecter à `variable` succéssivement chaque clé d'un tableau et d'exécuter `block` pour cette clé.

```bash
cat tableawk.tsv | awk '
    {
        if($0 !~ "^#"){
            age[$2]=age[$2] + 1;
        }
    }
    END{
        for(a in age){
            print "number of "a" years old : "age[a];
        }
    }'
```

### 11. Un peu de bioinfo ?

On considère le VCF `1.variants.vcf` et on veut connaître le première et le dernier variant de chaque chromosome.

1. On va évider de traiter les lignes d'en-tête grace à `if($0 !~ "^#")`
2. On va stoquer les variants dans des tableaux `first[]` et `last[]`
3. On va parcourir ces tableaux et afficher

- :warning: On va chercher les minimums et maximum des positions. Les *cases vides* des tableaux contiennent 0.
- `||` signifie ***OU***
- `if(cond1 || cond2)` vérifie que `cond1` ou `cond2` (ou les deux) est VRAI

```bash
cat 1.variants.vcf | awk '
    {
        if($0 !~ "^#"){
            chr=$1; 
            pos=$2;
            if(pos < first[chr] || first[chr] == 0){
                first[chr] = pos;
            }
            if(pos > last[chr]){
                last[chr] = pos;
            }            
        }
    }
    END{
        for(chr in last){
            print "Chromosome "chr":"first[chr]"-"last[chr];
        }
    }'
```
### 12. :smiling_imp: Welcome to my world of pain :smiling_imp:

On veut afficher tous les variants pour lesquels l'enfant est homozyogote et les parents hétérozygotes.

#### Rappel du format

```bash
cat 1.variants.vcf | grep "^#" | tail -1
```

#### Eviter les en-têtes

```bash
cat 1.variants.vcf | grep -v "^#" | head
```

#### On ne prend que les colonnes qui nous intéressent

- chrom (1)
- pos (2)
- id (3)
- ref (4)
- alt (5)
- enfant (10)
- maman (11)
- papa (12)

```bash
cat 1.variants.vcf | grep -v "^#" | head | cut -f-5,10-
```

#### On s'intéresse au génotype, mais pas aux annotations qui suivent

On transforme les colonnes en lignes

```bash
cat 1.variants.vcf | grep -v "^#" | head | cut -f-5,10- | tr "\t" "\n"
```

On enlève se qui suit les doubles points `:`

```bash
cat 1.variants.vcf | grep -v "^#" | head | cut -f-5,10- | tr "\t" "\n" | cut -f1 -d':'
```

On reforme les lignes (il y avait 8 colonnes)

```bash
cat 1.variants.vcf | grep -v "^#" | cut -f-5,10- | tr "\t" "\n" | cut -f1 -d':' | paste - - - - - - - - > variants.tsv
cat variants.tsv | head 
```

#### C'est parti

Les génotypes sont dans les colonnes
- enfant : 6
- maman : 7
- papa : 8

On affiche tout

```bash
cat variants.tsv | awk '
    {
        if($6 == "1/1" && $7 == "0/1" && $8 == "0/1")
        {
            print $0
        }
    }' 
```

#### Solution alternative pour garder toute la ligne et faire un VCF valide avec en-tête

```bash
cat 1.variants.vcf | awk '
    {
        if($1 ~ "^#"){
            print $0
        }
        
        if($10 ~ "^1/1" && $11 ~ "^0/1" && $12 ~ "^0/1")
        {
            print $0
        }
    }' 
```

> [!warning] Ces solutions ne marchent que dans le cas des variants bialleliques. 
> Pour un variant de type 1/1 | 0/1 | 1/2, il faut séparer les chromosomes

