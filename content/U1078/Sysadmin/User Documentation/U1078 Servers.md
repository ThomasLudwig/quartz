---
title: "U1078 Servers"
tags:
- documentation
- sysadmin
---

###### tags #documentation #sysadmin

## Servers

| Server | Address | Cores | RAM | OS |
|---|---|---|---|---|
| alanine | 193.54.252.59 | 64 @2.2Ghz | 384Go DDR3 | Ubuntu 22.04.1 LTS *Jammy Jellyfish* |
| lysine  | 193.54.252.69 | 48 @2.3Ghz | 384Go DDR4 | Ubuntu 22.04.1 LTS *Jammy Jellyfish* |
| serine  | 193.54.252.17 | 56 @2.4Ghz | 512Go DDR4 | Ubuntu 22.04.1 LTS *Jammy Jellyfish* |
| valine  | 193.54.252.16 | 56 @2.4Ghz | 512Go DDR4 | Ubuntu 22.04.1 LTS *Jammy Jellyfish* |

### FileSystem

Each user has 4 personnal directories
- `/home/username` this directory is private to each server
- `/WORKING_DIRECTORY/username` (linked to `/home/username/working_directory` ) this directory is shared accross all servers
- `/PROGS/conda/username` which stores conda libraries and environments
- `/PROGS/scripts/username` in which the scripts you are developping should be stored
    - This also to separate scripts from PROJECTS data, thus granted acces to your scripts to other users, thatare not allows to access the same projects as you
    - The subdirectory `/PROGS/scripts/username/bin` is available in your path. By symlinking a scripts to this directory, it makes it available from any directory. Ex `ln -s /PROGS/scripts/username/ProjectsA/Sorting/mybestsortingalgorithm.sh /PROGS/scripts/username/bin`

There are also some directories shared accross the servers
- `/PROGS` that contains available programs
    - `/PROGS/EXTERN` that contains public softwares
    - `/PROGS/INTERN` that contains tools developed in the research unit
- `/PUBLIC_DATA` that contains data from public databases
- `/PROJECTS` that contains one directory by project. These directories are protected. Content is available to all authorized users.

## Authentication

Your account name is the first letter of your firstname, following by your lastname.
*Example: **Élias de Kelliwic'h*** :arrow_right:  `edekelliwich`

Connection to the server via `ssh` is protected by double authentication:
- password
- TOTP by Google Authenticator

### Password

#### Rules

- At least 12 characters
- At least one of each : Uppercase, Lowercase, Number, Other
- Longest repeat (aaa, 111,+++): 3
- Longest sequence (abc,123,zyx,987): 3
- Different from the 5 previous passwords
- At most 9 characters in common with previous password
- Username is not present (straight or reversed) in the password
- Password will expire once a year

#### Command

To change you password, type on each server : `passwd`

### Google Authenticator

Google authenticator is a tool to generate a 6-digits code.
- A new code will be generated every 30s
- Each code is valid during 4 minutes
- Each code is only valid once (on each server)

> [!WARNING] For this system to work, the time on your computer must be correct !

When your action is create, you will received from `U1078GGB.servers@univ-brest.fr` a mail looking like this:

> [!INFO]
> Your new secret key is: **MRKJP3LAHJKWYFE6AFUHSUGWL3**
> Your verification code is 049804
> Your emergency scratch codes are:
>   *95863214*
>   *50189304*
>   *90746054*

You will need to install either
- Google Authenticator on your smartphone
- Auhenticator plugin in Chrome
- both

#### Google Authenticator on Chrome

- Go to [this link](https://chrome.google.com/webstore/detail/authenticator/bhghoamapcdpbohphigoooaddinpkbai)
- Install and Enable the plugin
- Click on the ![](https://lh3.googleusercontent.com/LEgohRXYMasRoU-SXiJrkH_LsMMMgpKERWbOCpofID-cbbtKm4DjovRnDo2eiyvWBGcOUSjvQmBPOGKJW7g8y1aJCw=w128-h128-e365-rj-sc0x00ffffff) Icon in the Browser
- Click on the :pencil2: Icon
- Click on :heavy_plus_sign: 
- Click on "Sasie Manuelle"
    - *Emetteur* : U1078Servers
    - *Secret* : Your secret key from the mail
    - **OK**

#### Google Authenticator on Android

- From THe Plugin in chrome, click on the square icon next to the code
- Scan from your smartphone

## Connection

Connection on the servers is done via the `ssh` protocole.

> [!DANGER] Nothing is displayed when typing your password (no ********)

> [!WARNING] On the first connection, your default password is expired, you'il need to change it **on each server**
> 
> The sequence is as follow:
> 1. Enter your **current** password:  `O1d_P4ssword`
> 2. Enter the verification code from google authenticator
> 3. Your password is expired, you will be required to change it: enter (again) your **current** password
> 4. Enter your **new** password (it must respect the rules)
> 5. Enter a second time your **new** password (for confirmation)
> 6. Your session will close automatically, login once again

### Windows

- Download MobaXterm from the [link](https://mobaxterm.mobatek.net/download-home-edition.html).
- Install it
- Launch it
- Click on *session > ssh*
- Remote host: Enter the name of the server (alanine, lysine, serine or valine)
- Check *specify username*
- Enter you username
- Click **OK**

### Mac/Unix

If your login is **edekelliwich** and you want to connect to serine
- Type `ssh -X edekelliwich@serine` (`-X` can be ommited if you don't plan on using the graphical user interface)
- To copy a local file to the server 
```scp /path/on/you/computer/filename.ext edekelliwich@serine:/path/on/the/server```
- To copy a file from the server 
```scp edekelliwich@serine:/path/on/the/server/filename.ext /path/on/you/computer/```

### VPN

If you are not directly connected to the UBO network, you'll need to install and confirgure **EduVPN** (extra credentials are needed)

## Permissions

To ask for permissions to a group, fill the following form 
http://methionine.univ-brest.fr/groupadduser/

## Tutorial

[[Tutorials]]