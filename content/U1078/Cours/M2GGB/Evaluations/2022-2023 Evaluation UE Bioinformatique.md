---
title: "2022-2023 Evaluation UE Bioinformatique"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

> [!success] Envoyer les réponses par mail sous format PDF. 
> 
> Les réponses justifiées et présentées proprements seront mieux notées.

## Unix

1. Que fait la commande suivante `cat fichier | cut -f3 | grep "BRCA1" | wc -l`
2. Ecrire une commande qui ne garde que les lignes qui **ne** contiennent **pas** pas le texte *SUCCESS*
3. Vous avez un fichier par gène dans un répertoire. Chaque fichier est nommé `nom_du_gene.gene`. Il n'y a pas d'en-tête dans le fichier et les lignes du tableaux suivent le format suivant :

| chromosome  | position (entier) | nom_du_gene | score (entier) |
|-----|----------|------|-------|
| 17  | 41197210 | CFTR | 117   |
| 17  | 41197231 | CFTR | 4     |
| 17  | 41197300 | CFTR | 67814 |
| ... | ...      | ...  | ...   |
| 17  | 41215385 | CFTR | 325   |
| 17  | 71223165 | CFTR | 6487  |

Ecrire une suite de commandes (avec `|`) pour trouver le nom du gène avec le variant qui possède le plus grand score.

## FastQ

4. Que fait la commande suivante. Commentez son résultat:
    ```
    cat monfichier.fastq | wc -l
    12007
    ```
5. Dans un FastQ, une base a une qualité $Q=17$, qu'en pensez-vous ?
6. Quelle est la probabilité d'erreur pour une base de qualité `B` ?
7. Ecrire une commande qui compte le nombre de reads dans un fichier fastq


## SAM/BAM

8. Quels sont les fichiers d'entrée nécessaires pour obtenir un alignement BAM/SAM ?
9. Un read possède le FLAG `99` quel sera vraissemblement le FLAG de son *mate* ? Expliquez
10. Expliquer les CIGAR suivants et préciser la longueur du read dans chaque cas:
    - 120M
    - 50M3D47M
    - 140M10S
11. Ecrire une commande qui compte le nombre de reads avec du hard clipping
12. Si la référence est `CCTCGGTATCTTTCTAGAAGTCCTCGT` et que le read est `CCCCGGTATCTTGTCTAGAAGTCTCGT`, quel sera le CIGAR ?
13. Pourquoi faut-il trier les fichiers BAM ?
14. Dans un fichier BAM qui contient plusieurs individus, quel attribut permet de differencier les reads d'un individu donné ?

## VCF

15. Un fichier VCF posède la ligne suivante, quelles sont les allèles (lettres) de chaque individu ? Combien y a-t'il d'homozygotes ? Qui a une pronfondeur de couverture (depth) de plus de 20 ?

    | `#CHROM` | POS | ID | REF | ALT | [...] | FORMAT | alice | bob | charlie | david | eve | frank |
    |---|---|---|---|---|---|---|---|---|---|---|---|---|
    | chr12 | 45671 | . | C | T,A | [...] | GT:DP:GQ | 0/0:32:99 | 0/1:21:99 | 1/1:13:60 | 0/2:60:99 | 1/1:8:10 | ./.:.:. |
    
16. Pourquoi serait-il étrange de voir la ligne suivante dans un fichier VCF ?

    | `#CHROM` | POS | ID | REF | ALT | [...] | FORMAT | alice | bob | charlie     | david | eve | frank |
    |---|---|---|---|---|---|---|---|---|---|---|---|---|
    |chr12 | 57954 | . | G | C | [...] | GT:DP:GQ | 0/0:32:99 | 0/0:21:99 |     ./.:.:. | 0/0:60:99 | 0/0:8:10 | ./.:.:.|

17. Ecrire une commande qui permet de compter le nombre de sites variants dans un fichier VCF
18. <!--Ecrire une commande qui permet de compter le nombre de variants multiallèliques (sites variants avec plusieurs allèles alternatifs)-->
19. Ecrire une commande qui permet de compter le nombre d'individus dans un fichier VCF.
    *Je vous suggère d’utiliser la commande* `tr` *quelque part, mais ce n’est pas obligatoire.*
20. La recalibration d'un fichier VCF par VQSR permet de classer les variants dans des *tranches*. Pourquoi intégrer (ou non) un nombre important de tranches dans une analyse peut-il être intéressant ?

## Base de Données et Annotations

21. GnomAD répertorie la fréquence des variants dans plusieurs population. Pourquoi est-il souhaitable de filtrer les variants en fonction de leur fréquence ?
22. Quelle est la frequence dans FrEx du variant `rs192173389`. 
23. Ce variant vous semble t-il avoir été bien couvert lors du séquençage ?
24. Comment interpretez vous ses scores SIFT/Polyphen ?
25. A quelle classe d'*Impact* appartient l'annotation *missense_variant* ? D'après-vous pourquoi est-elle dans cette classe ?
26. L'acide aminé impacté par le variant `rs545014063` vous semble t'il très conservé chez les vertébrés ?
27. Pour ce même variant, en vous basant sur les annotations CADD, quel allèle est le plus délétère et quel semble être l'allèle ancestral ?

**Dans USCS (Genome Human GRCh37/hg19) chercher le variant : rs113993960**

28. Quel gène est impacté
29. Quels sont les nucléotides et l'acides aminés impactés par ce variant
30. En survolant l'acide aminé avec votre souris, dans quel exon du gène se trouve cet acide aminé ? 
31. En zoomant en arrière, quels transcrits sont impactés/non-impactés par ce variant. (afficher les données de `Ensembl Genes`)
32. En affichant en "Full" les données de `OMIM Alleles` quel est la conséquence de ce variant sur la protéine. Quelle pathologie peut en découler ?

<!--
## Que font les commandes suivantes :

28. cat monfichier.fastq | paste - - - - | cut -f2 | grep "ACGTGCGCGCGTG"
29. cat monfichier.sam | grep -v "^@" | cut -f6 | grep "H" | wc -l
30. cat monfichier.vcf | grep -v "#" | cut -f11 | cut -c1-3 | sort | uniq -c
-->

