---
title: "Tutorials"
tags:
- documentation
- sysadmin
---

###### tags #documentation #sysadmin

## How-To

- [[Sysadmin/User Documentation/Conda@U1078]]
- [[Sysadmin/User Documentation/Mettre à disposition des fichiers via https]]

> [!INFO] **Paralleliser avec `xargs`**
> 
> The xargs command can be used to run the same script with differente arguments, in parallel
> - the -P*n* arguments allows xargs to use at most *n* processors at all time
> - the -l arguments tells to read each line as a list of arguments.
> 
> *Example*
> Considere a script script.sh . We want to launch that script 1000 times, using at most 8 processors.
> 
> We put the arguments of the script in a file arguments.lst, and execute :
> 
> `cat arguments.lst | xargs -l -P8 ./script.sh`

> [!WARNING] **Récupérer des fichiers effacés/corrompus**
> 
> 1. Trouver le fiesystem qui contient le fichier : `df .`
> 2. Aller dans le répertoire du filesystem (ex `cd /mnt/projects1/`)
> 3. Aller dans le répertoire caché zfs `cd .zfs/snashot`
> 4. Repérer une date de sauvegarde qui pourrait contenir une version acceptable du fichier (ex `cd zfs-auto-snap_monthly-2022-10-01-0452`)
> 5. Copier le fichier depuis ce répertoire vers le bon répertoire de travail

## Cours M2GGB

Des TP de bioinfo, des tutoriaux informatiques et des exercies sont disponibles en bas de cette page : [[Cours/M2GGB/TP M2GGB]]


Test :

```dot
digraph RAID { 
    label="Alanine Partitions" 
    bgcolor="#888888:#DDDDDD" 
    gradientangle="90"    
    fontcolor="white"
    splines=false      
    fontname="Helvetica "
    graph [
        rankdir="LR",
        nodesep=0.1,
        ranksep=0.5,
    ]
    node [
        fontname="Helvetica ",
        shape="box",
        style="filled",
        height=.3,
        width=2,
        fixedsize = true,
    ]
    edge [
        color="black",
        arrowhead=blank,
    ]
    subgraph clusterphysical{
        fontcolor="black"
        bgcolor="#E8F8F5:#FDEDEC"  
        gradientangle="270"
        label="Physical Servers"
        g1 [style="invis"]
        g2 [style="invis"]
        alanine [fillcolor="#FADBD8"]
        g3 [style="invis"]
    }
    subgraph clusterraid{
        fontcolor="black"
        bgcolor="#E8F8F5:#FDEDEC"  
        gradientangle="270"
        label="RAID Groups"
        newos [fillcolor="#F5B7B1"]
        m1 [style="invis"]
        alanine1 [fillcolor="#F1948A"]
        alanine2 [fillcolor="#CD6155"]
    }
    
    alanine:e->newos:w
    alanine:e->alanine1:w
    alanine:e->alanine2:w
    g3->alanine2 [style="invis"]
    
    subgraph clusterpartiton{
        fontcolor="black"
        bgcolor="#E8F8F5:#FDEDEC"  
        gradientangle="270"
        label="Partitions"
        node  [fillcolor="#F5B7B1"]
        aefi [label="/boot/efi" fontcolor="white"]
        aroot [label="/" fontcolor="white"]
        node  [fillcolor="#F1948A"]
        ahome [label="/home" fontcolor="white"]
        d1 [style="invis"]
    }        

    newos:e->aefi:w
    newos:e->aroot:w
    alanine1:e->ahome:w
}
```


