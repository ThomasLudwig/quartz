---
title: "BioInfo TP1 - Unix & Linux"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

> [!success] :book: *Linux is User-Friendly too, it’s just more peaky who its friends are...*

## 1. Why Linux ?
- Write programs that do one thing and do it well.
- Write programs that work together.
- Write programs that handle text streams, because that is a universal interface.
 
## 2. Why learn Linux basic command lines ?

- Simple : do one thing
- Flexible : built for reuse
- Versatile : can do anything/everything
- Fast : no graphics overload
- Ubiquitous : available on all machines— Permanent : First released in 1971

## 3. shell (command-line interpreter)

A Unix **terminal** is a command-line interpreter or **shell** that provides a traditional Unix-like command line user interface. Users direct the operation of the computer by entering commands as text for a command line interpreter to execute, or by creating text scripts of one or more such commands.

## 4.  bash (one type of shell, like sh, ksh, zsh,...)

bash is a command processor that typically runs in a text window, where the user types commands that causeactions.

## 5.  exit (leave the interpreter)

```bash
bash
exit
```

## 6. Linux Shortcuts

- Previous command : $\uparrow$ Key
- Next command : $\downarrow$ Key
- Autocompletion : $\longrightarrow$ (*TAB*) Key

> [!info] ***Copy+Paste***: 
> - ***Unix/Unix***: 
>     1. Select text 
>     2. middle-click (wheel button) where you want to paste
> - ***Windows/Unix***: 
>     1. Select text and Ctrl-C in Windows 
>     2. middle-click (wheel button) where you want to paste in Unix
> - ***Unix/Windows***: 
>     1. Select text in Unix 
>     2. Ctrl-V to paste in Windows

## 7. echo (print a message)

Command 7.1
```bash
echo
```
Command 7.2
```bash
echo "ATGC"
```
Command 7.3
```bash
echo -n "ATGC"
```
Command 7.4
```bash
echo -e  ">seq\nATGC"
```
Command 7.5
```bash
echo -e  ">seq\tATGC"
```
Command 7.6
```bash
echo -e -n  ">seq\nATGC"
```
## 8. man (user manual)

```bash
man echo
```
```bash
man man
```

leave by pressing `q`

## 9. filesystem

- 'home' (See `~`)
- 'root'

```bash
/
+-- /home/
|   +-- /home/tludwig/
|       +-- /home/tludwig/seqlink/
|       |   +-- /home/tludwig/seqlink/SEQLinkage-master/
|       |   +-- /home/tludwig/seqlink/SEQLinkage-master.zip
|       +-- /home/tludwig/linux/
|       |   +-- /home/tludwig/linux/installed.pkg
|       +-- /home/tludwig/file1.txt
|       +-- /home/tludwig/file2.txt
+-- /etc
+-- /usr
```

## 10. path

- A Unix/Linux pathname is a text string made up of one or more names separated by slashes
- Destination object may be file, directory, or other
- `.` current directory
- `..` parent directory
- `/` root directory
- `/home/user/directory/file.txt` : absolute path
- `directory/file.txt`: relative path to the current directory. Same as `./directory/file.txt`

## 11. pwd (Print working directory)
*Where am I ?*
```bash
pwd
```
## 12. ls (Lists files and folders)
Commande 12.1
```bash
ls
```
Commande 12.2
```bash
ls .
```
Commande 12.3
```bash
ls /
```
Commande 12.4
```bash
ls ../
```
Commande 12.5
```bash
ls ../..
```
Commande 12.6
```bash
ls -1 /
```
Commande 12.7
```bash
ls -l /
```
Commande 12.8
```bash
ls does_not_exists
```
### file globbing `*` means *anything*

Commande 12.9
```bash
ls /home/* #all files in /home/
```
Commande 12.10
```bash
ls /etc/*.conf #all files in /etc/ that end with .conf
```
Commande 12.11
```bash
ls -d /home/*i* #all /home subdirectory that contain i
```

## 13. cd (Change Directory)
Commande 13.1
```bash
cd ..
```
Commande 13.2
```bash
cd .
```
Commande 13.3
```bash
cd 
```
Commande 13.4
```bash
cd ~
```
Commande 13.5
```bash
cd /
```
Commande 13.6
```bash
cd -
```
## 14. mkdir (make directory)
Commandes 14.1
```bash
mkdir dir1 dir2 dir3
ls
```
Commande 14.2
```bash
mkdir a/b/c/d
```
Commande 14.3
```bash
mkdir -p a/b/c/d
ls
ls a
ls a/b/c
```

## 15. cp (copy files or folders)
Commandes 15.1
```bash
cp /home/tludwig/master2ggb/file1.txt file1.txt
ls
```
Commande 15.2
```bash
cp file1.txt dir1
ls dir1
```
Commande 15.3
```bash
cp file1.txt dir1/file2.txt
ls dir1
```
Commande 15.4
```bash
cp dir1 dir2
```
Commande 15.5
```bash
cp -r dir1 dir2
ls dir2
```
## 16. rmdir (remove directory)
```bash
rmdir dir1 dir2 dir3 a
ls
```
## 17. rm (remove files or folders)
Commande 17.1
```bash
rm file1.txt
```
Commande 17.2
```bash
rm  does_not_exits.txt
```
Commande 17.3
```bash
rm -f does_not_exits.txt
```
Commande 17.4
```bash
rmdir a
```
Commande 17.5
```bash
rm -rf a
ls
```
Commande 17.6
```bash
rm dir1
```
Commande 17.7
```bash
rm -rf dir1 dir2
```

## 18. seq (Create a sequence)
Commande 18.1
```bash
seq 10
```
Commande 18.2
```bash
seq 2 9
```
Commande 18.3
```bash
seq 2 3 9
```
## 19. cat (Concatenate files)
Préparation
```bash
cp /home/tludwig/master2ggb/file1.txt .
cp /home/tludwig/master2ggb/file2.txt .
```
Commande 19.1
```bash
cat file1.txt
```
Commande 19.2
```bash
cat file2.txt
```
Commande 19.3
```bash
cat file1.txt file2.txt
```
Commande 19.4
```bash
cat -n file1.txt file2.txt
```

## 20. standard input
Where the data are read from (default : keyboard)
- `<` uses a file as standard input.
## 21. standard output (stdout)
Where the results are written (default : screen)
- `>` redirects standard output to a file (and overwrites the file if it already exists).
- `>>` Appends standard output to a file.

Sans redirection
```bash
ls /root /bin
```
Commande 21.1
```bash
ls /root /bin > stdout.txt
cat stdout.txt
```
Commande 21.2
```bash
echo "new data" > stdout.txt
cat stdout.txt
```
Commande 21.3
```bash
ls /root ~ >> stdout.txt
cat stdout.txt
```
## 22. standard error (stderr)
Where the error messages are written (default : screen)
- `2>` redirects standard error to a file (and overwrites the file if it already exists)

Commande 22.1
```bash
ls /root /bin 2> stderr.txt
cat stderr.txt
```
Commande 22.2
```bash
ls /root /bin  > stdout.txt 2> stderr.txt
cat stdout.txt
cat stderr.txt
```
## 23. redirection (stdout | stdin )
- `|` (*pipe* : `AltGR+6` for normal people `Alt+Maj+L` for Mac users) redirects standard output of a program as standard input of another.

Sans redirection
```bash
ls /
```
Commande 23.1
```bash
ls / | cat -n
```
Commande 23.2
```bash
echo "1601627*5" | bc
```

## 24. wc (Word count)
Commande 24.1
```bash
cat /etc/libaudit.conf
wc /etc/libaudit.conf
```
Commande 24.2
```bash
seq 1000000 | wc -c
```
Commande 24.3
```bash
seq 1000000 | wc -l
```
Commande 24.4
```bash
echo | wc -c
```
Commande 24.5
```bash
echo -n | wc -c
```

## 25. stop input stdin : Ctrl-D
```bash
bc
5*5
2+9
```
:keyboard: <ctrl+D>
## 26. stop a processus Ctrl-C
```bash
seq 10000000
```
:keyboard: <ctrl+C>
## 27. head (output the first part of files)
Commande 27.1
```bash
seq 10000 | head
```
Commande 27.2
```bash
seq 10000 | head -3
```
## 28. tail (output the last part of files)
Commande 28.1
```bash
seq 10000 | tail
```
Commande 28.2
```bash
seq 10000 | tail -3
```
Commande 28.3
```bash
cat /home/tludwig/master2ggb/table.tsv
```
Commande 28.4
```bash
cat /home/tludwig/master2ggb/table.tsv | tail -n+2
```

> [!warning] Cette commande doit être utilisé en même temps pour tout le groupe, attendre le feu vert :traffic_light: 
> 
> Commande 27.2
> ```bash
> tail -f /home/tludwig/master2ggb/log.txt
> ```
> :keyboard: <ctrl+C>

## 29. paste (merge lines of files)
Préparation
```bash
seq 1 10 > a 
cat a 
seq 2 12 > b 
cat b 
```
Commande 29.1
```bash
paste a b
```
Commande 29.2
```bash
seq 20 | paste - -
```
Commande 29.3
```bash
seq 20 | paste - - -
```

## 30. cut (remove sections from each line of files)
Sans cut
```bash
seq 1 100 | paste - - - - -
```
Commande 30.1
```bash
seq 1 100 | paste - - - - - | cut -f 3
```
Commande 30.2
```bash
seq 1 100 | paste - - - - - | cut -f 3-
```
Commande 30.3
```bash
seq 1 100 | paste - - - - - | cut -f 2-4
```
Commande 30.4
```bash
seq 1 100 | paste - - - - - | cut -f -2
```
Commande 30.4
```bash
seq 1 100 | paste - - - - - | cut -f 1,5
```
Commande 30.5
```bash
echo "ABCDE" | cut -c 2
```
Commande 30.6
```bash
echo "ABCDE" | cut -c 2,5
```
Commande 30.7
```bash
echo "ABCDE" | cut -c 2-5
```
Commande 30.8
```bash
echo "AA:BB:CC:DD:EE:FF" | cut -d':' -f2
```

## 31. sort (sort lines of text files)
Commande 31.1
```bash
seq 1 25 | sort
```
Commande 31.2
```bash
seq 1 25 | sort -n
```
Commande 31.3
```bash
seq 1 25 | sort -n -r
```
Commande 31.4
```bash
cat /home/tludwig/master2ggb/chrom.list
```
Commande 31.5
```bash
cat /home/tludwig/master2ggb/chrom.list | sort
```
Commande 31.6
```bash
cat /home/tludwig/master2ggb/chrom.list | sort -n
```
Commande 31.7
```bash
cat /home/tludwig/master2ggb/chrom.list | sort -V
```
Commande 31.8
```bash
cat /home/tludwig/master2ggb/table.tsv
```
Commande 31.9
```bash
cat /home/tludwig/master2ggb/table.tsv | tail -n+2 | sort -t $'\t' -k2n
```
Commande 31.10
```bash
cat /home/tludwig/master2ggb/table.tsv | tail -n+2 | sort -t $'\t'  -k3r -k2n
```

## 32. uniq (report or omit repeated lines)

Utiliser un fichier de variant


Sans uniq
```bash
echo -e "A\nA\nA\nB\nB\nA"
```
Commande 32.1
```bash
echo -e "A\nA\nA\nB\nB\nA" | uniq
```
Commande 32.2
```bash
echo -e "A\nA\nA\nB\nB\nA" | sort | uniq
```
Commande 32.3
```bash
echo -e "A\nA\nA\nB\nB\nA" | sort -u
```
Commande 32.4
```bash
echo -e "A\nA\nA\nB\nB\nA" | sort | uniq -c
```
Commande 32.5
```bash
echo -e "A\nA\nA\nB\nB\nA" | sort | uniq -c | sort -n
```
## 33. grep (print lines matching a pattern

ajouter grep -v

Sans grep
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT"
```
Commande 33.1
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep AT
```
Commande 33.2
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep GAATTC
```
Commande 33.3
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep -B1 GAATTC
```
Commande 33.4
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep -A1 gene2
```
Commande 33.5
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep -E '(GAATTC|CCT)' 
```
Commande 33.6
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep -o A | wc -l
```
Commande 33.7
```bash
echo -e ">gene1\nATGGAATTCAAAA\n>gene2\nCCCTGCTGATCGATCGATCGT" | grep -o A
```
## 34. more / less (file perusal)
```bash
seq 1000000 | more
seq 1000000 | less
```
- `<enter>` to skip 1 line
- `<space>` to skip 1 page
- `q` to quit
- arrows in `less` but not in `more`

## 35. touch (Change file timestamp, or create a file)
```bash
touch new_file
ls -l
```

## 36. find (search for files in a directory hierarchy)
Commande 36.1
```bash
find ~ -type f
```
Commande 36.2
```bash
find /etc -type f -name "*.conf"
```

## 37. tr (Translate, squeeze, and/or delete characters)
Commande 37.1
```bash
echo "ATAAAAACGACTTTTGA" | tr "T" "U"
```
Commande 37.2
```bash
echo "ATAAAAACGACTTTTGA" | tr -s "A"
```
Commande 37.3
```bash
echo "ATAAAAACGACTTTTGA" | tr -d "A"
```
Commande 37.4
```bash
echo "ATAAAAACGACTTTTGA" | tr "[ATGC]" "[TACG]"
```

## 38. rev (reverse lines characterwise)
```bash
echo "ATAAAAACGACTTTTGA" | tr "[ATGC]" "[UACG]" | rev
```

## 39. gzip (compress files)
Sans gzip
```bash
seq 100000 | wc -c
```
Commande 39.1
```bash
seq 100000 | gzip | wc -c
```
Sans gzip
```bash
cat file1.txt
```
Commande 39.2
```bash
gzip file1.txt
cat file1.txt.gz
```
## 40. gunzip / zcat (expand files)
Commande 40.1
```bash
zcat file1.txt.gz
```
Commande 40.2
```bash
gunzip file1.txt.gz
```
Commande 40.3
```bash
seq 10 | gzip | gunzip 
```
## 41. diff / sdiff (compare files line by line)
Préparation
```bash
seq 20 > a 
seq 10 30 > b 
```
Commande 41.1
```bash
diff a b
```
Commande 41.2
```bash
sdiff a b
```
## 42. split (split a file into pieces)
```bash
seq 1 100 | split -l 20 - tmp_
ls
cat tmp_ab
```
## 43. fold (wrap each input line to fit in specified width)
Sans fold
```bash
echo -e ">Gene1\nATCGATCGATCGATGACTAGCTAGTCATCGATCGACTGATCTACGTC"
```
Commande 43.1
```bash
echo -e ">Gene1\nATCGATCGATCGATGACTAGCTAGTCATCGATCGACTGATCTACGTC" | fold -w 10
```