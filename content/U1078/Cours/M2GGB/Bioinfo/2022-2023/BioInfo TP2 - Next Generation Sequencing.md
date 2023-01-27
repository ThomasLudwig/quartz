---
title: "BioInfo TP2 - Next Generation Sequencing"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

Au cours de cette séance de TP nous allons chercher les mutations *de novo* d’un enfant dont l’exome ainsi que celui de ses deux parents ont été séquencés par la technologie Illumina. Il vous faudra aligner les reads sur un génome de référence, extraire les mutations et annoter ces variations.

## 1. Récupération des données FASTQ

### 1.1. Téléchargez et extrayez le fichier zip
```bash
mkdir ~/TP2
cd ~/TP2
wget http://lysine.univ-brest.fr/~tludwig/master2/DATA.zip
unzip DATA.zip
```
Dans le répertoire `DATA` se trouvent les fichiers FASTQ contenant les short-reads pour father/mother/child.
```bash
DATA/child.R2.ab.fq.gz
DATA/child.R2.aa.fq.gz
DATA/father.R1.aa.fq.gz
DATA/father.R2.aa.fq.gz
DATA/mother.R2.aa.fq.gz
DATA/mother.R1.ab.fq.gz
DATA/mother.R2.ab.fq.gz
DATA/father.R1.ab.fq.gz
DATA/father.R2.ab.fq.gz
DATA/child.R1.aa.fq.gz
DATA/child.R1.ab.fq.gz
DATA/mother.R1.aa.fq.gz
```

Nous travaillons en **paired-end** : il y a donc un fichier **R1** et **R2** pour chaque individu. 

![](http://lysine.univ-brest.fr/~tludwig/paired-end-read.jpg)

Par ailleurs on peut disposer de plusieurs jeux de données pour un même individu. C'est notament le cas lorsqu'on a utiliser plusieurs *lanes* pour un individu. Dans notre exemple, chaque individu pocède des données `aa` et `ab`.

### 1.2. Comptez le nombre de lignes `R1.aa` chez l'enfant; Vérifiez que c'est un multiple de 4.
```bash
zcat DATA/child.R1.aa.fq.gz | wc -l
```

### 1.3. Comptez le nombre de lignes `R2.aa` chez l'enfant; Pourquoi ce même nombre ?
```bash
zcat DATA/child.R2.aa.fq.gz | wc -l
```

### 1.4. Comparez les 10 premiers noms de reads de l'enfant entre le fichier R1 et R2, quelle est la différence ?
```bash
zcat DATA/child.R1.aa.fq.gz | paste - - - - | cut -f 1 | head
zcat DATA/child.R2.aa.fq.gz | paste - - - - | cut -f 1 | head
```

### 1.5. Quelle est la longueur des short-reads R1 chez l'enfant ?
```bash
zcat DATA/child.R1.aa.fq.gz | paste - - - - | cut -f 2 | head
zcat DATA/child.R1.aa.fq.gz | paste - - - - | cut -f 2 | head -1 | tr -d '\n'| wc -c
```

### 1.6. Dans les short-reads R1 de l'enfant, extrayez les bases des séquences d'ADN, quelle est la proportion de ces bases (nombre de A, nombre de T, nombre de G, etc.) ?  n'y a-t-il que A-T-G-C ?
```bash
zcat DATA/child.R1.aa.fq.gz | paste - - - - | cut -f 2 | grep -o . | sort | uniq -c
```

ici `grep` utilise une *expression régulière* ou *Regex*. C'est un language qui permet de décrire un texte. Le caractère `.`  signifie *n'importe quel caractère*

### 1.7. Quels sont les caractères ascii de qualité que l'on trouve dans les lignes de qualité ?
```bash
zcat DATA/child.R1.aa.fq.gz | paste - - - - | cut -f 4 | grep -o . | sort | uniq | paste - - - - - -
```

## 2. Récupération du génome de référence
Pour aller vite, nous nous contenterons des chromosomes 22 et de l'ADN mitochondrial. 

### 2.1. Téléchargez les séquences `.fa.gz` **chr22** et **chrM** de la version **hg19/GRCh37** du génome humain.
```bash
wget "http://lysine.univ-brest.fr/~tludwig/master2/hg19/chr22.fa.gz"
wget "http://lysine.univ-brest.fr/~tludwig/master2/hg19/chrM.fa.gz"
```

### 2.2. dézippez les deux fichiers.
```bash
gunzip chr22.fa.gz chrM.fa.gz
```

### 2.3. Quelle est la taille du chromosome 22 ? Quelle est la taille du chrM ?
```bash
grep -v '>' chr22.fa | tr -d '\n' | wc -c
grep -v '>' chrM.fa  | tr -d '\n' | wc -c
```

### 2.4. Quelles sont les premières bases du chromosomes 22 ?
```bash
less chr22.fa
```

<!-- N

there are still hundreds of unresolved gaps which account for about 5% of the total sequence length (>150Mb).

Gaps au sein des chromosome (region en repeat TTAATATTAATATTAATA)
Gaps au niveau des telomeres et centromeres : l'ADN n'est pas totalement décompacté au cours du séquençage dans ces régions.

-->

### 2.5. Pourquoi observez-vous cela ? Combien de lignes y-a-t-il avant que cela change ?
```bash
cat -n chr22.fa | grep -v '>'  | grep -v 'N' | head 
```

### 2.6. A l'aide de cat, concaténez ces deux fichiers dans un fichier que vous nommerez `ref.fa`
```bash
cat chr22.fa  chrM.fa > ref.fa
```

<!-- Majuscule/Minuscule 
Lower case letters are most commonly used to represent “soft-masked sequences”

"Soft-masked" assembly sequence in one file. Repeats from RepeatMasker and Tandem Repeats Finder (with period of 12 or less) are shown in lower case; non-repeating sequence is shown in upper case.

There are three types of Ensembl reference genomes: unmasked, soft-masked and masked. Generally speaking, it's recommended to use unmasked reference genomes builds for alignment. Masking is used to detect and conceal interspersed repeats and low complexity DNA regions so that they could be processed properly by alignment tools.
-->

## 3. Prise en main de BWA
*BWA* est l'outil qui va nous permettre de mapper les reads sur le génome de référence. *BWA* lui-même contient une série de sous programmes que l'on peut lister en tapant `bwa`
```
bwa

Program: bwa (alignment via Burrows-Wheeler transformation)

Usage:   bwa <command> [options]

Command: index         index sequences in the FASTA format
         mem           BWA-MEM algorithm
(...)
On peut obtenir l'aide de chaque sous-programme en invoquant le nom du sous-programme.
bwa index

Usage:   bwa index [-a bwtsw|is] [-c] <in.fasta>

Options: -a STR    BWT construction algorithm: bwtsw or is [auto]
         -p STR    prefix of the index [same as fasta name]
         -6        index files named as <in.fasta>.64.* instead of <in.fasta>.* 
```

## 4. Prise en main de SAMTOOLS
*samtools* est le couteau suisse des formats en NGS. Tout comme *bwa*, *samtools* lui-même contient une série de sous programmes que l'on peut afficher en tapant `samtools`
```
samtools

Program: samtools (Tools for alignments in the SAM format)
Version: 0.1.19-44428cd

Usage:   samtools <command> [options]

Command: view        SAM<->BAM conversion
         sort        sort alignment file
         mpileup     multi-way pileup
         depth       compute the depth
         faidx       index/extract FASTA
         tview       text alignment viewer
         index       index alignment
         idxstats    BAM index stats (r595 or later)
         fixmate     fix mate information
         flagstat    simple stats
         calmd       recalculate MD/NM tags and '=' bases
         merge       merge sorted alignments
         rmdup       remove PCR duplicates
         reheader    replace BAM header
         cat         concatenate BAMs
         bedcov      read depth per BED region
         targetcut   cut fosmid regions (for fosmid pool only)
         phase       phase heterozygotes
         bamshuf     shuffle and group alignments by name

(...)
```
On peut également obtenir l'aide de chaque sous-programme en invoquant le nom du sous-programme.
```
samtools view

Usage:   samtools view [options] <in.bam>|<in.sam> [region1 [...]]

Options: -b       output BAM
         -h       print header for the SAM output
         -H       print header only (no alignments)
         -S       input is SAM
         -u       uncompressed BAM output (force -b)
         -1       fast compression (force -b)
         -x       output FLAG in HEX (samtools-C specific)
         -X       output FLAG in string (samtools-C specific)
         -c       print only the count of matching records
         -B       collapse the backward CIGAR operation
         -@ INT   number of BAM compression threads [0]
         -L FILE  output alignments overlapping the input BED FILE [null]
         -t FILE  list of reference names and lengths (force -S) [null]
         -T FILE  reference sequence file (force -S) [null]
         -o FILE  output file name [stdout]
         -R FILE  list of read groups to be outputted [null]
         -f INT   required flag, 0 for unset [0]
         -F INT   filtering flag, 0 for unset [0]
         -q INT   minimum mapping quality [0]
         -l STR   only output reads in library STR [null]
         -r STR   only output reads in read group STR [null]
         -s FLOAT fraction of templates to subsample; integer part as seed [-1]
         -?       longer help
```

## 5. Construction de l'index du génome de référence
### 5.1. Construire l'index à l'aide de bwa index
```bash
bwa index ref.fa
```
devrait afficher quelque chose dans le genre :
```
[bwa_index] Pack FASTA... 0.51 sec
[bwa_index] Construct BWT for the packed sequence...
[BWTIncCreate] textLength=102642274, availableWord=19222168
[BWTIncConstructFromPacked] 10 iterations done. 31707250 characters processed.
[BWTIncConstructFromPacked] 20 iterations done. 58575122 characters processed.
[BWTIncConstructFromPacked] 30 iterations done. 82451330 characters processed.
[BWTIncConstructFromPacked] 40 iterations done. 102642274 characters processed.
[bwt_gen] Finished constructing BWT in 40 iterations.
[bwa_index] 32.52 seconds elapse.
[bwa_index] Update BWT... 0.26 sec
[bwa_index] Pack forward-only FASTA... 0.40 sec
[bwa_index] Construct SA from BWT and Occ... 7.81 sec
[main] Version: 0.7.12-r1039
[main] CMD: bwa index ref.fa
[main] Real time: 41.542 sec; CPU: 41.500 sec
```

### 5.2. Vérifiez que des fichiers d'index ont été créés
```bash
ls ref.fa.*
```

## 6. Mapping des séquences fastq sur la référence
Maintenant que l'index du génome de référence a été créé, nous cherchons a connaitre la position des reads sur ce génome de référence.

:::info
***synopsis*** de la commande *bwa* pour mapper les reads 
```bash
bwa mem  -R '@RG\tID:SAMPLENAME-aa\tSM:SAMPLENAME' reference.fa > sample.R1.gz sample.R2.gz > sample.sam
```
L’option `-R` permet de créer un **READ-GROUP** pour associer les reads a un échantillon (donc ici, remplacez SAMPLENAME par '*child*').
:::warning
:warning: ne pas taper cette commande :warning: 
:::


La sortie standard de cette commande est un '*fichier texte*' **SAM**.

### 6.1. Mappez les reads de l'enfant avec cette commande
Utilisez en entrée `child.R1.aa.fq.gz` et `child.R2.aa.fq.gz` et redirigez la sortie standard vers un fichier `child_unsorted.aa.sam` (à l'aide de l'opérateur `>`)
```bash
bwa mem  -R '@RG\tID:child-aa\tSM:child' ref.fa DATA/child.R1.aa.fq.gz DATA/child.R2.aa.fq.gz > child_unsorted.aa.sam
```

### 6.2. Que renvoie la commande suivante  ?
```bash
file child_unsorted.aa.sam
```

### 6.3. Combien y-a-t-il de lignes dans ce fichier sam ?
```bash
wc -l child_unsorted.aa.sam 
```

<!--
cut -f1 child_unsorted.aa.sam | sort | uniq -c | sort -nr | head
      3 HWI-1KL149:97:C6Y6VACXX:5:1215:8773:68642
      3 HWI-1KL149:97:C6Y6VACXX:4:1306:15197:40857

-->

### 6.4. Combien y-a-t-il de lignes dans l'en-tête ?
```bash
grep '^@' child_unsorted.aa.sam | wc -l
```

En *regex* le caractère `^` signifie *commence par*

### 6.5. Observez les lignes de l'en-tête commençant par `@SQ` , y a-t-il une différence avec le nombre de bases que vous aviez observé dans les séquences Fasta des chr22 et chrM ?

### 6.6. Retrouvez la ligne `@RG` du 'read-group' dans l'en-tête, vérifiez que tous les reads dans ce fichier portent une référence à ce read groupe dans la colonne de métadonnées.

### 6.7. Combien y-a-t-il d'alignements ?

### 6.8. Comparez au nombre de reads dans les fichiers `child.R1.aa.fq.gz` et `child.R2.aa.fq.gz`

### 6.9. Identifiez les colonnes d'un fichier SAM:

|#|Colonne|
|---|---|
|1 | READ NAME|
|2 | FLAG|
|3 | CHROMOSOME|
|4 | POS|
|5 | MAPPING QUALITY|
|6 | CIGAR *(représentation compacte de l'alignement)*|
|7 | MATE CHROMOSOME|
|8 | MATE POS|
|9 | DISTANCE READ-MATE *(négatif si mate placé devant)*|
|10 | DNA SEQ|
|11 | QUALITIES|

```bash
grep -v '^@' child_unsorted.aa.sam | head -1 | tr "\t" "\n" | cat -n
```

La deuxième colonne contient le *SAM FLAG*: des méta-informations sur le mapping des reads. 

### 6.10. Quel(s) est(sont) le(s) SAM Flag retrouvé(s) le plus souvent ?

### 6.11. Cherchez sa(leur) signification(s)
Référez vous à [http://broadinstitute.github.io/picard/explain-flags.html](http://broadinstitute.github.io/picard/explain-flags.html)

<!--
83/163
12484 83 : en pair, la paire est correcte : 1er / reverse
12484 163 : en pair, la paire est correcte : 2e / forward
10672 99 : en pair, la paire est correcte : 1er / forward
10672 147: en pair, la paire est correcte : 2e / reverse

48 97/145 : (99/147) mais la paire N'est PAS correcte
45 81/161 : (83/163) mais la paire N'est PAS correcte
22 73/133 : ce read/son mate n'est pas mappé
20 121/181 : ce read/son mate n'est pas mappé (les 2 sont en reverse)
4 69/137 : ce read/son mate n'est pas mappé (les 2 sont en foward)

2 2109 : aucun des 2 n'est mappé, les 2 sont en reverse | supplementary alignment

1 117/185 : la paire N'est PAS correcte, ce read/son mate n'est pas mappé
1 2131 : 83 | supplementary alignment
1 2179 : la paire est correcte | supplementary alignment
1 2209 | supplementary alignment

-->

### 6.12. Prenez les reads suivants et vérifiez que vous retrouvez la même séquence & la même qualité dans le fichier FASTQ originel

| # | ID | Flag | ??|
|---|---|---|---|
| #1326 | `HWI-1KL149:97:C6Y6VACXX:5:1107:14195:70766` | 163 | R2 - Forward |
| #3533 | `HWI-1KL149:97:C6Y6VACXX:5:2113:7047:14280` | 83 | R1 - Reverse |

> [!info] Pour extraire le read `1440` du BAM et du fastq, il faut taper
> 
> - Pour voir l'identifiant, la séquence et la qualité:
> ```bash
> grep -v "^@" child_unsorted.aa.sam | head -1440 | tail -1 | cut -f1,2,10,11 | tr "\t" "\n"
> ```
> - Pour voir la séquence original dans les fastq:
> ```bash
> # récurérer l'identifiant du read, ici
> # HWI-1KL149:97:C6Y6VACXX:5:1206:9505:92003
> zcat DATA/child.R1.aa.fq.gz DATA/child.R2.aa.fq.gz | grep -A3 "HWI-1KL149:97:C6Y6VACXX:5:1206:9505:92003"
> 
> # ou directement avec la commande
> 
> zcat DATA/child.R1.aa.fq.gz DATA/child.R2.aa.fq.gz | grep -A3 `grep -v "^@"  child_unsorted.aa.sam | head -1440 | tail -1 | cut -f1`
> ```

### 6.13. Observez et décrivez les CIGAR des reads suivants

Pour le fichier `child_unsorted.aa.sam`

> [!info] Adaptez la première commande de la section précedante, mais ne récupérez que la 6$^e$ colonne. Utilisez les commandes `grep -v "^@"`, `head -XXXX`, `tail -1`, `cut -f6`

- #10
- #6
- #110
- #1915
- #11391
- #842

| Letter | Description |
|---|---|
|M| Alignment **M**atch *(can be a sequence match or mismatch)* |
|I| **I**nsertion to the reference |
|D| **D**eletion from the reference |
|S| **S**oft clipping *(clipped sequences present in SEQ)* |
|H| **H**ard clipping *(clipped sequences **NOT** present in SEQ)* |
|\*| No known bases in the read |

<!-- 
Soft Clipping : Les bases en 5'/3' du Reads ne sont a alignées, la longueur du read ne change pas
Hard Clipping : Les bases en 5'/3' du Reads sont supprimées, la longueur du read est réduite

-->

## 7. Conversion de SAM en BAM
Pour permettre aux algorithmes d'aller plus vite, nous utilisons samtools pour convertir le fichier en format *texte*/**SAM** `child.aa.sam` en format *binaire*/**BAM** `child_unsorted.aa.bam`. 
Les fichiers **BAM** sont en fait des fichiers **SAM** compressés avec `gzip`. Ainsi, ils prenent 6 $\times$ mois de place sur les fichiers **BAM**. Leur traitement devrait en revanche être plus long, car ils doivent être décompressés pour être lu, mais grace à l'utilisateur d'*index*, l'accès à des données précises être extrémement plus rapide.

> [!info] ***Synopsis***
> ```bash
> samtools view -bS -o out.bam in.sam
> ```
> - Option `-S` : l'entrée est du SAM
> - Option `-b` : la sortie est du binaire (BAM)
> - Option `-o` : nom du fichier de sortie
> 
> > [!warning] ne pas taper cette commande :warning: 

### 7.1. Générez le fichier `child_unsorted.aa.bam`
```bash
samtools view -bS -o child_unsorted.aa.bam child_unsorted.aa.sam
```

### 7.2. Que renvoie la commande suivante ? 
```bash
file child_unsorted.aa.bam
```

### 7.3. Affichez les options de samtools view
```bash
samtools view
```

### 7.4. Affichez le corps du BAM
```bash
samtools view child_unsorted.aa.bam | less
```

### 7.5. Affichez l'en-tête du BAM
```bash
samtools view -H child_unsorted.aa.bam
```

### 7.6. Affichez le bam complet avec
```bash
samtools view -h child_unsorted.aa.bam | less
```

### 7.7. Comptez les reads qui étaient à l'origine dans le fichier `child.R1.aa.fq.gz` (a.k.a *first in pair*)
Aidez-vous de [http://broadinstitute.github.io/picard/explain-flags.html](http://broadinstitute.github.io/picard/explain-flags.html) et de l'option `-f flag` de `samtools view`.

## 8. Tri du BAM sur la position génomique

Pour l'instant, les alignements dans le **BAM** sont dans le même ordre que les *reads* dans les **FASTQ**, c'est à dire qu'ils sont totalement mélangés.

Pour aller plus vite, les algorithmes détectant les mutations (et autres) ont besoin que les reads soient triés en fonction de leurs coordonnées génomiques. 

Pour cela on utilise `samtools sort`

> [!info] ***Synopsis***
> ```bash
> samtools sort -o out.bam -T out in-unsorted.bam
> ```
> - `-T PREFIX` Write temporary files to `PREFIX.nnnn.bam`
> - `-o FILE` Write final output to `FILE` rather than *standard output*
> 
> > [!warning] ne pas taper cette commande :warning: 


### 8.1. Triez `child_unsorted.aa.bam` pour produire un fichier `child_sorted.aa.bam`
```bash
samtools sort -T tmp -o child_sorted.aa.bam child_unsorted.aa.bam
```

### 8.2. Vérifiez que le fichier est trié
La première ligne de son en-tête qui doit contenir le mot *coordinate*. 
```bash
samtools view -H child_sorted.aa.bam 
```
En utilisant `samtools view`, en observant la position croissante CHROM/POS des reads dans le fichier **BAM**.
```bash
samtools view  child_sorted.aa.bam | cut -f 3,4 | head -100
```

## 9. Mapping de la deuxième paire `child.R1.ab.fq.gz` et `child.R2.ab.fq.gz`

### Procédez pour la seconde paire, comme pour la première
Mappez les fichiers `child.R1.ab.fq.gz` et `child.R2.ab.fq.gz` de manière à produire un fichier BAM `child_sorted.ab.bam`

> ***NOTE:***  Dans un environnement parallélisé (cluster...), le mapping de `child_sorted.aa.bam` et `child_sorted.ab.bam` pourrait être fait en parallèle...

Afin de ne pas avoir à retaper/adapter toutes les commandes, et éviter les fautes de frappes, un script est disponible. Il générera également toutes les données précédentes (et les corrigera si nécessaire).
```bash
/home/tludwig/master2ggb/tp2.step9.sh
```
<!--cp -R /home/tludwig/master2ggb/TP2/step9/-->

## 10. Fusion des BAM pour le même échantillon.
Maintenant que nous avons `child_sorted.aa.bam` et `child_sorted.ab.bam`, nous pouvons les fusionner en un seul fichier **BAM** `child.bam` pour cela on utilise la commande `samtools merge`

> [!info] ***Synopsis***
> ```bash
> samtools merge -f  out.bam in1.bam in2.bam in3.bam ....
> ```
> - option `-f` : *forcer* la création du fichier.
> 
> > [!warning] ne pas taper cette commande :warning: 


### 10.1. Fusionnez les fichiers
Fusionnez `child_sorted.aa.bam` et `child_sorted.ab.bam` pour produire child.bam.

A l'aide de `samtools view`, vérifiez que le fichier est toujours trié et que le nombre d'alignements total est conservé.

```bash
samtools merge -f child.bam child_sorted.aa.bam child_sorted.ab.bam
```

## 11. Indexation du BAM
L'indexation consiste à créer un fichier d'*index* `.bai` à partir d'un bam trié. Cet index fonctionne comme une sorte d'annuaire qui permet d'accéder rapidement aux reads mappés dans une région génomique précise. Ici on se sert de la commande `samtools index`

> [!info] ***Synopsis***
> ```bash
> samtools index in.bam
> ```
> 
> > [!warning] ne pas taper cette commande :warning: 

### 11.1. Indexez le fichier child.bam

```bash
for i in {1..10}; 
do 
    echo "J'ai copié/collé la commande sans réflechir au lieu d'adapter le synopsis" ; 
done
```

### 11.2. Vérifiez le fonctionnement de l'index
Effectuez des requêtes par région
```bash
samtools view child.bam chr22 | head 
samtools view child.bam chrM | head 
samtools view child.bam chr22:18300000-18900000 | head
```

## 12. Indexation du genome de reference avec samtools
<!-- skip section -->

Samtools doit pouvoir accèder rapidement à des régions du génome de reference. Pour cela il lui faut préalablement créer un petit fichier d'index pour la séquence de reference.

> [!info] ***Synopsis***
> ```bash
> samtools faidx reference.fa
> ```
> 
> > [!warning] ne pas taper cette commande :warning: 

### 12.1. Indexez votre génome de reference.
La commande doit générer un fichier `.fai` qui contient les informations nécessaires (offset de la séquence dans le fichier, taille des lignes,.... ) pour accèder à des sous-séquences.

### 12.2. Extrayez la sequence chrM:50-200

Une fois indexé, on peut rapidement obtenir des sous-régions du genome de référence au format fasta.

> [!info] ***Synopsis***
> ```bash
> samtools faidx reference.fa chr:start-end
> ```
> 
> > [!warning] ne pas taper cette commande :warning: 

## 13. Visualisation interactive avec tview
La commande `samtools tview` permet de visualiser interactivement les reads dans une région donnée. Il faut que le BAM et la séquence de référence soient indexés.

> [!info] ***Synopsis***
> ```bash
> samtools tview sorted.bam ref.fa
> ```

### 13.1. Affichez les premiers reads mappés dans `child.bam`

A l'aide de `samtools view`.
Notez la position génomique du 1$^{er}$ read mappé.
### 13.2. Affichez `child.sam`
```bash
samtools tview child.bam ref.fa
```

### 13.3. Naviguez dans le fichier

Appuyez sur la touch `g` (pour *go to*) et tapez la position `chr22:22707361` du read identifier précedemment.

Utilisez les flêches pour naviguer et `?` pour obtenir de l'aide, `Q` pour quitter.

Vous pouvez notamment jouer avec les touches `m`,`n`,`b` et `.`

```bash
22707361  22707371  22707381  22707391  22707401  22707411  
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
Y...........................................................
T agccacacagcctgattctgactcttgtgtaaaagatcactaaaaaaaatattacctt
C  GCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
C  gccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
TCA ccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
TCAGCCA   AGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAG  TGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCC GATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCT ATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTG TTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
CCAGCACCTCAGCCTGA TCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGA   TGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACT  TGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCT GTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTT TGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTG CAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAA ATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAG TCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCA TAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCAC AAAAAAAATATTACCTT
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCT AAAAAAATATTACCTT
ccagcacctcagcctgattctgactcttctggcaaagatccct AAAAAAATATTACCTT
tcagccacacagcctgattctgactcttgtgtcaaagatcact  AAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTNA AAAAATATTACCTT
ccagcacctcagcctgattctgactcttctggcaaagatccctaa AAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAA AAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAA aaatattacctt
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAA AATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAA  ATTACCTT
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCTAAAAAA atattacctt
ccagcacctcagcctgattctgactcttctggcaaagatccctaaaaaa  TATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAAT   acctt
tcagccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatatt CCTT
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCTAAAAAA  tattacctt
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCTAAAAAA   ATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACC t
tcagccacacagcctgattctgactcttg*gtcaaagatcactaaaaaaaatattacct 
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCTAAAAAA    TTACCTT
CCAGCACCTCAGCCTGATTCTGACTCTTCTGGCAAAGATCCCTAAAAAA    TTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
ccagcacctcagcctgattctgactcttctggcaaagatccctaaaaaa      acctt
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
tcagccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
tcagccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
tcagccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
tcagccacacagcctgattctgactcttgtgtcaaagatcactaaaaaaaatattacctt
TCAGCCACACAGCCTGATTCTGACTCTTGTGTCAAAGATCACTAAAAAAAATATTACCTT
```

## 14. Mapping des fichiers parentaux.
### 14.1. Refaire tout le TP pour les reads de la mère et du père

A partir des fichiers

- `DATA/mother.R1.aa.fq.gz`
- `DATA/mother.R1.ab.fq.gz`
- `DATA/mother.R2.aa.fq.gz`
- `DATA/mother.R2.ab.fq.gz`
- `DATA/father.R1.aa.fq.gz`
- `DATA/father.R1.ab.fq.gz`
- `DATA/father.R2.aa.fq.gz`
- `DATA/father.R2.ab.fq.gz`

Afin de ne pas avoir à retaper toutes les commandes, et éviter les fautes de frappes, un script est disponible. Il générera également toutes les données précédentes (et les corrigera si nécessaire).
```bash
/home/tludwig/master2ggb/tp2.step14.sh
```

## 15. Calling des mutations
Le calling des mutations se fait à l'aide de 

- `bcftools mpileup` qui va générer les bases trouvées à toutes les positions pour les 3 bams. Cet outil génére un fichier *binaire* de variants **BCF**.
- `bcftools call` va transformer le **BCF** (*binaire*) en format *texte* **VCF** (Variant Call Format).


> [!info] ***Synopsis***
> ```bash
 > bcftools mpileup --fasta-ref ref.fa --output mutations.bcf  file1.bam file2.bam ... fileN.bam 
> bcftools call  --multiallelic-caller --variants-only --output-type z -o result.vcf.gz --format-fields GQ,GP input.bcf
> ```
> 
> > [!warning] ne pas taper ces commandes :warning: 

## 16. Fichier VCF
### Réperez la ligne commençant par `#CHROM`
C'est l'en-tête standard d'un fichier VCF, dont voici les colonnes

|#  | Colonne|
|---|---|
|1  | CHROM|
|2  | POS|
|3  | ID|
|4  | REF|
|5  | ALT|
|6  | QUAL|
|7  | FILTER|
|8  | INFO|
|9  | FORMAT|
|10 | child|
|11 | mother|
|12 | father|

Le contenu d'un fichier VCF sera étudié en détail lors de la prochaine séance.

## 17. Recherche du variant causal
### 17.1. Calling des variants
```bash
bcftools mpileup --fasta-ref ref.fa --output mutations.bcf  child.bam father.bam mother.bam
bcftools call  --multiallelic-caller --variants-only --output-type z -o result.vcf.gz --format-fields GQ,GP mutations.bcf
```

### 18.1. Combien y a t-il de variants dans le fichier ?
```bash
zcat result.vcf.gz | grep -v "^#" | wc -l
```

### 18.2. Extraction des variants *de novo*
On regarde quels variants (lignes) sont absents chez les 2 parents (homozygotes à la référence). Combien y en a t-il ?
```bash
zcat result.vcf.gz | awk '($1 ~ "^#" || ($11 ~ "0/0" && $12 ~ "0/0")) {print $0;}' > denovo.vcf
```

### 18.3. Annotation du fichier
<!--
:::danger
:no_entry_sign: ***Indisponible sur la Virtual Box***
:::
-->
L'annotation ajoute un certain nombre d'informations utiles dans le fichier. Le nombre de variants ne change pas, mais chaque ligne est enrichie.
```bash
vep denovo.vcf annotated.vcf
```

### 18.4. Extraction des variants délétaires
On va rechercher, grace aux annotations, les variants ayant un impact fort (***HIGH***) ou modéré (***MODERATE***) sur la protéine traduite. 

Combien y en a t'il ?
```bash
cat annotated.vcf | grep -v "^#" | grep -E '(HIGH|MODERATE)' > variants.list
```

### 18.5. Recherche des gènes impactés
On extrait la liste des gènes impactés. Un même variant peut impacter plusieurs protéines. Il peut également avoir plusieurs impacts sur une même protéine (notamment en impactant différents transcrits). Combien de gènes sont touchés ? A quelle famille de gènes appartiennent-ils ?
```bash
cat variants.list | cut -f8 | tr ';' '\n' | grep "^CSQ" | tr ',' '\n' | cut -d '|' -f4 | sort -u
```
