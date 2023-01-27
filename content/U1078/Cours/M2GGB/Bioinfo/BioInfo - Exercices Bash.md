---
title: "BioInfo - Exercices Bash"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 

Preparation
```bash
cd
cp /home/tludwig/s.omalleysbar.txt .
```

### A. Get the first word from each line (easy)
scp 
use `cut`

```bash
cat s.omalleysbar.txt | ... | head -30
```

:::spoiler Solution
- use `cut -d' '` and keep first field
:::spoiler Code
```bash
cat s.omalleysbar.txt | cut -d' ' -f1 | head -30
```
:::

### B. Sort first word from each line by number of occurences (medium)

use `cut`, `sort`, `uniq`

```bash
cat s.omalleysbar.txt | ... | head -20
```

:::spoiler Solution
- get first word
- sort
- count uniq occurences
- sort numerically
:::spoiler Code
```bash
cat s.omalleysbar.txt | cut -d' ' -f1 | sort | uniq -c | sort -n | tail -30
```
:::

### C. Get the last word from each line (hard)

use `cut`, `rev`

```bash
cat s.omalleysbar.txt | ... | head -20
```

:::spoiler Solution
- reverse the whole line
- get the first word of the reversed line
- reverse this word
:::spoiler Code
```bash
cat s.omalleysbar.txt | rev | cut -d' ' -f1 | rev | head -30
```
:::

### D. Get all the line containing "as" (easy)

use `grep -w`

```bash
cat s.omalleysbar.txt | ... 
```

:::spoiler Solution
- use `grep -w "as"`
:::spoiler Code
```bash
cat s.omalleysbar.txt | grep -w "as" | head -30
```
:::

### E. Get the number of occurences of "as" (medium)

use `grep`, `wc`

```bash
cat s.omalleysbar.txt | ... 
```

:::spoiler Solution
- use `grep -o`
:::spoiler Code
```bash
cat s.omalleysbar.txt | grep -o -w "as" | wc -l
```
:::

### F. Sort all words alphabetically (medium)

use `tr`, `sort`
```
cat s.omalleysbar.txt | tr '[:upper:]' '[:lower:]' | ... | head -30
```

:::spoiler Solution
- put one word per line
- sort them
:::spoiler Code
```bash
cat s.omalleysbar.txt | tr '[:upper:]' '[:lower:]' | tr " " "\n"| sort -u | head -30
```
:::

### G. Sort all words by number of occurences (medium-hard)

use `tr`, `sort`, `uniq`

```bash
cat s.omalleysbar.txt | tr '[:upper:]' '[:lower:]' | ... | head -30
```

:::spoiler Solution
- put one word per line
- sort them
- count unique occurences
- sort numerically
:::spoiler Code
```bash
cat s.omalleysbar.txt | tr '[:upper:]' '[:lower:]' | tr " " "\n"| sort | uniq -c | sort -n | tail -30
```
:::

### H. Get all lines with more than 45 characters (hard)

use `cut`, `diff`, `grep`

```bash
cat s.omalleysbar.txt | ... 
...
```

:::spoiler Solution
- cut lines after the 45^th^ character
- store the result in a file
- get the difference between this file and the original
:::spoiler Code
```bash
cat s.omalleysbar.txt | cut -c-45 > max45
diff s.omalleysbar.txt max45 | grep "^<" | cut -c3-
```
:::

