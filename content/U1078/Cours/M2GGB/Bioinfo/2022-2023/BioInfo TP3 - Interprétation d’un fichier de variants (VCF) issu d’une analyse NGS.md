---
title: "BioInfo TP3 - Interprétation d’un fichier de variants (VCF) issu d’une analyse NGS"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

> [!success] **Objectif**: Analyser un fichier de variants (VCF) dans le but d’identifier le variant causal d’une pathologie.
> *Remarque*: pour simplifier, le fichier VCF est limité à quelques régions seulement.

## Rappel du format VCF

### Colonnes

| CHROM | POS | ID | REF | ALT | QUAL | FILTER | INFO | FORMAT | Samples... |
|---|---|---|---|---|---|---|---|---|---|

### Numéro des Allèles
- 0 : Allele de Reférence (REF)
- 1,2,...,n : Alleles alternatifs (ALT$_1$,ALT$_2$,...ALT$_n$)

### Génotypes

> [!info] *Exemple* : REF = A; ALT = C,G
> - 0 = A
> - 1 = C
> - 2 = G

- 0/0 = A/A
- 0/1 = A/C
- 1/1 = C/C
- 1/2 = C/G
- ./. ou . = missing

### Phase
`/` pour un génotype non phasé et `|` pour un génotype phasé.
- Si un individu possède un variant A/C puis un variant G/T, on sait que l'individu possède un allèle A et un allèle C, et plus loin un allèle G et un allèle T
- Si un individu possède un variant A|C puis un variant G|T, on sait que les allèles A et G sont sur le même chromosome et que C et T sont sur l'autre chromosome. AG et CT forment donc des haplotypes

## 1. Comprendre le format VCF

Pour répondre, utilisez les outils unix (`more`, `grep`, `wc`, ...) pour analysez le fichier VCF:

Téléchargez le fichier [http://lysine.univ-brest.fr/~tludwig/master2/tp3/1.variants.vcf](http://lysine.univ-brest.fr/~tludwig/master2/tp3/1.variants.vcf)

### 1.1. Quelle est la version (format) de ce fichier VCF ?

### 1.2. Quelle version de GATK a été utilisée pour identifier les variants ?

### 1.3. Contre quel génome de référence les reads ont-il été alignés ?

### 1.4. Que signifient MQ, DP, GT ?

### 1.5. Combien y-a-t'il d'individus (samples) dans ce fichier ?

### 1.6. Combien le fichier compte-t'il de variants ?

C'est à dire
- Positions variantes (*facile*)
- Allèles variants (*plus difficile*)
- 
### 1.7. Comment distingue-t'on les SNVs, les insertions et les délétions ?

### 1.8. Examinez le variant 1:156706559 (rs8658) :

1. Combien y-a-t'il de substitutions, d'insertions et de délétions à cette position ? 
2. Combien d'individus portent des alléles A, C et G ?
3. Combien y-a-t'il d'allèles A, C, G ?
4. Combien y-a-t'il d'individus au génotype homozygote / hétérozygote ?
5. Quelle est la qualité de génotypage de chaque individu à cette position ?

## 2. UCSC Browser

Visualisez le variant `rs4619` dans le browser UCSC. 

[https://genome-euro.ucsc.edu/cgi-bin/hgGateway](https://genome-euro.ucsc.edu/cgi-bin/hgGateway)

Choisir :
- Human Assembly : Feb. 2009 (GRCh37/hg19)
- All Short Genetic Variants from dbSNP Release 153


### 2.1. Sur quel chromosome et quelle bande se trouve ce variant ?
### 2.2. Dans quel gène ? 

L'info est visible directement, mais ne pas hésiter à afficher les données gencode
Dans `Genes and Gene Predictions`, passez `GENCODE` de `hide` a `show` et cliquez `refresh`

### 2.3. Donnez au moins 1 identifiant ensembl d'un transcrit contenant ce variant ?

Dans `Genes and Gene Predictions`, passez `Ensembl Genes` de `hide` a `pack` et cliquez `refresh`

### 2.4. Où se trouve ce variant

- Chromosome
- Position
- Bande

### 2.5. Quel est l'acide aminé impacté ? 

Cliquez sur Zoom In `3x`

### 2.6. Cet acide aminé est-il très conservé entre l'humain et les autres espèces ?

## 3. Annotation fonctionnelle avec vep

Allez sur l'URL suivante pour effectuer une annotation avec vep (***V**ariant **E**ffect **P**redictor*):

[http://grch37.ensembl.org/Homo_sapiens/Tools/VEP](http://grch37.ensembl.org/Homo_sapiens/Tools/VEP)

Remplissez les champs suivants : 

| Clé | Valeur |
|---|---|
| Species | `Homo_sapiens` |
| Name for this job | TP3.1_`votre_nom` |
| Input data, provide file URL | [`http://lysine.univ-brest.fr/~tludwig/master2/tp3/1.variants.vcf`](http://lysine.univ-brest.fr/~tludwig/master2/tp3/1.variants.vcf) |
| Additional configurations, Filtering options | `Show one selected consequence per variant` |

En vous basant sur le tableau à cette URL [https://m.ensembl.org/info/genome/variation/prediction/predicted_data.html](https://m.ensembl.org/info/genome/variation/prediction/predicted_data.html), répondez aux questions suivantes.

![](https://m.ensembl.org/info/genome/variation/prediction/consequences.jpg)

Les 2 premières questions peuvent être traitée pendant que le job est `Queued` ou `Running`

### 3.1. Quelles sont les différentes classes d'*IMPACT* ? A quoi servent-elles ?

### 3.2. A quelle classe appartient l'annotation *missense_variant* ?

### 3.3. Utiliser les filtres pour répondre. 

Dans votre fichier, combien y-a-t'il  de variants :
1. synonymous_variant
2. 3_prime_UTR_variant
3. frameshift_variant

Utiliser `Filters`, `Consequence`, `is`, `XXXX`

### 3.4. Examinez la *gravité* des variants missense au niveau protéique

- Utiliser `Filters`, `Consequence`, `is`, `missense_variant`
- Cliquez sur `Show/hide columns` et cochez `SIFT`,`PolyPhen` et `AF`
- Triez les variants en fonction de `AF`

## 4. Fréquences alléliques en population

Pour cette partie, remplacez XXXXXX par un variant dans la liste suivante :

|  |  |  |  |
|---|---|---|---|
| rs4619 | rs10508 | rs697212 | rs719925 |
| rs1005974 | rs1661712 | rs1874344 | rs2278959 |
| rs73571431 | rs75306265 | rs75555675 | rs77914199 |
| rs144225791 | rs149864119 |  rs762194 | rs7323(  C ) |

### 4.1. Quelle est la fréquence dans 1000 genomes de XXXXXX ?

[http://grch37.ensembl.org/Homo_sapiens/Info/Index](http://grch37.ensembl.org/Homo_sapiens/Info/Index)

### 4.2. Quelle est la fréquence dans GnomAD genomes de XXXXXX ?

[https://gnomad.broadinstitute.org/](https://gnomad.broadinstitute.org/)

### 4.3. Sur combien d'individus est calculée cette fréquence ?

### 4.4. Quelle est la fréquence en Europe (hors Finlande), et sur combien d'individus est-elle calculée ?

### 4.5. Quelle sont les fréquences dans FrEx des variants rs8658 (C, G) ?

[http://lysine.univ-brest.fr/FrExAC/](http://lysine.univ-brest.fr/FrExAC/)

### 4.6. Déduisez en la fréquence de l'allèle de référence (A) ?

### 4.7. Ces fréquences sont globalement les mêmes dans 1000 genomes. 

Que concluez-vous en examinant les fréquences de A, C et G ?

### 4.8. Quelle est (environ) la profondeur de couverture moyenne du variant XXXXXX dans FrEx ?

## 5. Recherche du variant causal

On a filtré pour ne garder que les variants de très faible fréquence dans la population générale, et compatible avec une mutation *de novo*. On a également éliminé les variants non-exoniques ou n'ayant pas d'impact sur la protéine. On suppose que la pathologie étudiée est une sévère déficience intellectuelle.

Allez sur l'URL suivante pour effectuer une annotation avec vep (***V**ariant **E**ffect **P**redictor*):

[http://grch37.ensembl.org/Homo_sapiens/Tools/VEP](http://grch37.ensembl.org/Homo_sapiens/Tools/VEP)

Remplissez les champs suivants : 

| Clé | Valeur |
|---|---|
| Species | `Homo_sapiens` |
| Name for this job | TP3.2_`votre_nom` |
| Input data, provide file URL | [`http://lysine.univ-brest.fr/~tludwig/master2/tp3/2.denovo.vcf`](http://lysine.univ-brest.fr/~tludwig/master2/tp3/2.denovo.vcf) |
| Variants and frequency data | Décochez `1000 Genomes global minor allele frequency` et Cochez `gnomAD (exomes) allele frequencies` |
| Additional configurations, Filtering options | `Show all results` |

Cochez uniquement les colonnes
- Uploaded Variant
- Location
- Allele
- Consequence
- Symbol
- Exon
- Protein position
- Amino Acids
- SIFT
- PolyPhen
- gnomAD_AF

Et triez sur la colonne `Location`

### 5.1. Combien y-a-t'il de variants ? 

Ne répondez pas trop vite :wink: 

### 5.2. Combien sont nouveaux  ?

C'est à dire, inconnu dans ***dbSNP***

### 5.3. Combien y a t-il de variants *missense* ?

Utilisez les filtres, pour ne voir que les variant missense.

### 5.4. Quel variant pouvez vous éliminer ?

La pathologie étant extrêmement rare et supposée causée par un unique variant, quel variant pouvez-vous éliminer immédiatement de la liste des candidats ?

### 5.5. Quels sont les 3 variants les plus *graves* ?

En vous basant sur SIFT/PolyPhen, quels sont les variants ayant un effet grave sur la protéine.

### 5.6. Dans quels gènes sont situés ces variants ?

### 5.7. Quel semble être **le** variant causal ? Pourquoi ?

En regardant rapidement la description des gènes impactés et les phénotypes de la pathologie.

### 5.8. Dans quel exon est situé de ce variant ?

### 5.10. Quel est son impact sur la séquence protéique ?

### 5.11. Quel est son effet sur la protéine ?

D'après SIFT et PolyPhen 

### 5.12. Quelle est sa position sur la référence GRCh38 ?

- Cliquez sur le lien dans la colonne *Location*
- Notez bien que la position est paddée de 50bp de chaque cotée.
- Cherchez dans le menu de gauche

### 5.13. Quel est le score de conservation GERP à cette position ? 

Voir UCSC

