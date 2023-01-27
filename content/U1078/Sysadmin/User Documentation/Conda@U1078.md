---
title: "Conda@U1078"
tags:
- documentation
- sysadmin
---

###### tags #documentation #sysadmin

> [!DANGER] TL;RD From 2022/07/08 what is changing for you
> 1. Unless you edit you `.condarc` file, the ***base*** environment will not be automatically activated 
> 2. Your environment were moved to `/PROGS/conda/username/envs` so don't load them with full path: use `conda activate myenv` instead of `conda activate /the/old/path/to/myenv`

## Problem with Rscript

> [!DANGER] If Rscript doesn't work anymore, with this error: 
> `Rscript execution error: No such file or directory`
> you can solve the problem by typing: `conda update r-base`

## Introduction

So as to be mutualized on all servers, `conda` is preconfigured by default as follows :

The configuration file `/home/username/.condarc` on all server is a link to the file `/PROGS/conda/username/condarc`

Also the environments are stored in `/PROGS/conda/username/envs`. This guarantees that an environment created from one server is also available on all other servers. Conda Packages will be stored in  `/PROGS/conda/username/pkgs` because this disk will be larger and it will free space in the `/home` disk from each server.

> [!INFO]
> Conda is loaded be default for all users (some lines are added to you `.bashrc`). So the `conda` command line should be available to everyone upon log-in. By default, the *base* environment **will NOT** be loaded.

## The default configuration file 

The default configuration file is as follows, and can be edited by the user:

```yaml
channels:
  - r
  - conda-forge
  - bioconda
  - defaults
channel_priority: disabled
env_prompt: '{name}>'
envs_dirs:
  - /PROGS/conda/XXXXXXXX/envs
  - /PROGS/conda/common/envs
pkgs_dirs:
  - /PROGS/conda/XXXXXXXX/pkgs
auto_activate_base: false
```

`channel_priority`: Can be `strict`, `flexible` or `disabled`. ***Strict*** is faster, but has more risks of failing on packages installation. 
`env_prompt`: The ways the environment's name is displayed in your prompt
`auto_activate_base`: Set to ***TRUE*** if you want the base environment to be activated upon log-in

## Common environments

By default, when you create as environment, it will be stored in `/PROGS/conda/username/envs` and it will be private to you.
Everybody is also able to access `/PROGS/conda/common/envs` which holds environments that might be usefull for several users (related to a project or a pipeline).


## Creating an environment

Example : 

An environment name *myproject* on python 3.9 with `snakemake` `R` 4.1.3 and R package `stringi` 

```bash
conda create -n myproject python=3.9
conda activate myproject
conda install -y snakemake
conda install -y -c conda-forge r-base=4.1.3
conda install -y -c conda-forge r-stringi
````

## Listing environments

`conda info --envs`

## Joining/Leaving environment

- Join: `conda activate myproject`
- Leave: `conda deactivate`

> [!DANGER]
> Don't use full path like `/WORKING_DIRECTORY/xxxxxx/.conda/envs/myproject` everything is stored in `/PROGS/conda` now.
> The syntax is the same for personnel/common environments :
> ```bash
> conda activate MYenv
> conda activate COMMONenv
> ```

## Publishing an environment as common

Type the command `conda_publish privatename publicname`

where `privatename` is the current name of one of YOUR environments and `publicname` will be the name of the shared env. 
> [!WARNING]
> - The names **MUST** be different
> - This will only work is you belong to the `bioinfo` unix group.
> - Only members of `bioinfo` can modify a common env


