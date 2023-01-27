---
title: "TP M2GGB"
tags:
- m2ggb
---

###### tags #m2ggb
*contact :* ludwig@univ-brest.fr 

## 1. Préparation 
<!--
### 1.0. Visio

:::danger 
:warning: ***Connexion à EduRoam***

Si vous êtes en distanciel ou que vous n'arrivez pas à vous connecter à `EduRoam` il faudra

1. Télécharger et Installer [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
2. Télécharger et ouvrir la machine virtuelle [http://lysine.univ-brest.fr/~tludwig/m2ggb/U1078GGB.zip](http://lysine.univ-brest.fr/~tludwig/m2ggb/U1078GGB.zip)  contenant les données et programme pour le TP 
3. Extraire les fichier `.vbox` et `.vdi` du zip et ouvrir le fichier `.vbox` avec VirtualBox

Lancer la Virtual Box, mais avant de cliquer sur `Démarrer`, il faut cliquer sur `Oublier`

:::
-->
### 1.1. Windows - Télécharger MobaXTerm

Télécharger sur [https://mobaxterm.mobatek.net/download-home-edition.html](https://mobaxterm.mobatek.net/download-home-edition.html)
- *[Installer edition](https://download.mobatek.net/2132021082033134/MobaXterm_Installer_v21.3.zip)* sur dans la plupart des cas
- *[Portable edition](https://download.mobatek.net/2132021082033134/MobaXterm_Portable_v21.3.zip)* sur vous n'avez pas les droits d'administrateur sur vous ordinateur

### 1.2. Mac / Linux - Attendre que les autres téléchargent MobaXTerm

Sur Mac, vous pouvez repérer comment faire certains caractères utiles avec votre clavier:

| Caractère | Touches |
|---|---|
| \| | `alt + shift + l` |
| `~` | `alt + n` |
| `[` | `alt + shift + (` |
| `]` | `alt + shift + )` |
| `{` | `alt + (` |
| `}` | `alt + )` |

## 2. Connection au serveur

Liste des logins:

| Nom | Login |
|---|---|
| Axelle Autret | aautret |
| Morgane Ayella | mayella |
| Elora Bellenoue | ebellenoue |
| Theo Benvenuti | tbenvenuti |
| Wiam Echih | wechih |
| Pauline Foliard | pfoliard |
| Valentine Hoyau | vhoyau |
| Mouhammad Merhi | mmerhi |
| Abrahan Mendoza | amendoza |
| Imane Naouadir | inaouadir |
| Taghrid Nasser | tnasser |
| Nada Ouali | nouali |
| Emile Simon | esimon |

### 2.1. Windows

- Ouvrez MobaXTerm
- Cliquez sur Session
- ***Remote host*** : `methionine.univ-brest.fr`
- Specify username :ballot_box_with_check: **`login`**
- Port: 4444
- :heavy_check_mark: ***OK***

![](https://lysine.univ-brest.fr/~tludwig/mobaxtermdopamine.png)

Taper votre mot de passe 

> [!warning] C'est normal que rien ne s'affiche (pas de *****) 

### 2.2. Mac/Linux

Ouvrez le programme ***Terminal*** (dans ***/Applications/Utilitaires***)et tapez:

> [!warning] Remplacer `login` par votre login...

```
ssh -p 4444 -X login@methionine.univ-brest.fr
```

Taper votre mot de passe 

> [!warning] C'est normal que rien ne s'affiche (pas de *****) 

## 3. Avant de commencer

> [!danger] Faites attention à ce que vous tapez  
> 
> - Les majuscules/minuscules ne peuvent pas être interverties (la plupart du temps)
> - Les espaces/absences d'espaces sont importants (en revanche le nombre d'espace n'est pas important)
> 
> Un humain peut comprendre la phrase *"hiermatin j'ai VU unélé pHant"* mais pour une machine c'est plus difficile (et en réalité pas du tout souhaitable): la syntaxe et la grammaire doivent être respectées.
> 
> Ces commandes sont donc différentes:
> ```bash
> #fonctionnera
> cat monfichier | grep -v "quelque chose" 
> 
> #ne fonctionneront pas:
> cat monfichier | grep -V "quelque chose"
> CAT monfichier | grep -v "quelque chose"
> cat monfichier | grep-v "quelque choset"
> cat monfichier | grep - v "quelque chose"
> catmonfichier | grep -v "quelque chose"
> ```

> [!success] :notebook: **Prise de Note**
> - Il faudra probablement prendre des notes pendant le TP.
> - ça sera plus facile sur notepad/notepad++/sublimtext ou encore mieux https://hackmd.io/ où il suffira de copier coller l'ensemble du TP avant d'y ajouter notes ou commentaires
> - Si vous copiez des commandes dans word/openoffice/libreoffice/googledrive... il y a de grandes chances pour que certains caractères soient remplacés et que la commande ne marche plus une fois recopiée dans le terminal unix.

## TP Bioinfo

> [!danger] :bomb: ***Evaluation*** à rendre pour le 21 Novembre 2022 :skull:
> - [[Evaluations/2022-2023 Evaluation UE Bioinformatique]]

- [[Bioinfo/2022-2023/BioInfo TP1 - Unix & Linux]]
- [[Bioinfo/2022-2023/BioInfo TP2 - Next Generation Sequencing]]
- [[Bioinfo/2022-2023/BioInfo TP3 - Interprétation d’un fichier de variants (VCF) issu d’une analyse NGS]]
- [[Bioinfo/2022-2023/BioInfo TP4 - NGS Data Processing]]

Pour aller plus loin
- [[Bioinfo/BioInfo - Exercices Bash]]
- [[Bioinfo/Bioinfo - Programmer en AWK]]
- [[Bioinfo/BioInfo - Introduction à Python]]

## TP Epidémio

- [[Epidémio/2022-2023/Epidémiologie TP1 - R]]
- [Bases de Données](https://lysine.univ-brest.fr/~tludwig/master2/2022_M2_Genet_Epidemio_TP1_TLU.pdf)