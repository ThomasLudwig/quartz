---
title: "Mettre à disposition des fichiers via https"
tags:
- documentation
- sysadmin
---

###### tags #documentation #sysadmin

Créer des fichiers et répertoires:
```bash
mkdir -p ~/public_html/share
echo "AuthType Basic" > ~/public_html/share/.htaccess
echo "AuthName \"Password Protected Area\"" >> ~/public_html/share/.htaccess
echo "AuthUserFile \"/home/`whoami`/public_html/share/.htpasswd\"" >> ~/public_html/share/.htaccess
echo "Require valid-user" >> ~/public_html/share/.htaccess
```

Générer un mot de passe pour l'utilisateur `download`
```bash
htpasswd -c ~/public_html/share/.htpasswd download
```

Ajuster les droits
```bash
cp /path/to/my/files/*.fastq* ~/public_html/share
chmod 775 ~/public_html
chmod 775 ~/public_html/share
chmod 664 ~/public_html/share/.htpasswd
chmod 664 ~/public_html/share/.htaccess
```

Copier les fichiers et ajuster les droits 

> [!DANGER] adapter le chemin des fichier d'origines

```bash
cp /path/to/my/files/* ~/public_html/share
chmod 664 ~/public_html/share/*
```

Les fichiers seront accéssibles dans sur https://lysine.univ-brest.fr/~USER/share/ 
- identifiant "download"
- password (celui choisi plus haut)  
> [!DANGER] REMPLACER `USER` par l'identifiant du compte unix utilisé
