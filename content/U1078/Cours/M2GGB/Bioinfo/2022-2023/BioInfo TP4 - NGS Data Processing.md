---
title: "BioInfo TP4 - NGS Data Processing"
tags:
- m2ggb
- bioinfo
---

###### tags #m2ggb #bioinfo
*contact :* ludwig@univ-brest.fr 
## Part 1: NGS Workflow

> [!success] ***Objective*** :bookmark_tabs:
> Identification of candidate variants involved in a pathology in the simple case of a mendelian disease.

> [!warning] ***Disclaimer*** :warning:
> - Only Human Data will be addressed
> - Focus on constitutional (germline) variations
> - No time for in-depth analysis of every aspect of the workflow
> - *"Damn it, Jim ! I'm a Computer-Scientist, not a Biologist !"*

### 1.1. What do we want ?

- We have DNA from several individuals. Some are cases and some are controls (in regard of a given pathology).
- We want to identify **THE** genetic variant that is associated to the observed phenotype.
- We look at every variants in all the individuals and try to find some candidates that match the supposed transmission pattern (dominant, recessive, *de novo*...)

### 1.2. How to process data from the sequencer ?

- Trying to put every pieces of the text in its proper location: **alignment** against a "*reference genome*".
- Identifying the differences between the reconstructed text and the reference (biological variants, sequencing errors, alignment errors): **variant calling**.

### 1.3. What is a variant ?

> ***Small sized variations***
> - **SNV**: **S**ingle **N**ucleotide **V**ariant (abusively simplified as **SNP**)
> example : AGT<span style="color:green;font-weight:bold">G</span>TGC→AGT<span style="color:red;font-weight:bold">A</span>TGC
> - Short **indel**: **in**sertion or **del**eltion
> insertion : AGTGTGC→AGTG<span style="color:red;font-weight:bold">ACGC</span>TGC
> deletion : AGT<span style="color:green;font-weight:bold">GTG</span>C→AGTC

> ![](http://lysine.univ-brest.fr/~tludwig/oldstructuralvariantstlu.png)
> *length $\ge$ 1kb*

### 1.4. Classic NGS Workflow :wrench:

![](http://lysine.univ-brest.fr/~tludwig/NGSWorkFlow.png)

### 1.5. The Fastq file format

#### Example
```
@HWI-1KL149:109:C8D6YACXX:1:1101:1076:64252/1
CAGACTGCTTGTGTCCATGTCGTCCTCAGAGCCTAGTCCATGATGGCACAGTACTAGATCCACCAAGTCTTCTTTC
+
B@CFFFFFHHHHHHJJGIJJJJIJJJJJJEHIJJJJJJJJJIHJJIIFHGIHIIJIIIJJJJJIIGIJJEHJJJHH
@HWI-1KL149:109:C8D6YACXX:1:1101:1093:50109/1
GAGTCAAATAATCTTTAAGTGATGACAGAAAGTGAATCTTTTTCACTAAAACAAAACCTATCTGTGATACAAATGC
+
@CCDFFDFHHGHHJJJJJJJJIJJJJJJJIJJEHGIJJIJJJJJJJJJJIIIJBFGIJJIJIJJIJJJJJIAGIJG
```

#### Quality Score

- ASCII code from 33 `!` to 126 `~`: 93 scores available
- Score = Phred$_{+33}$
- $Q = -log_{10}(P) \Leftrightarrow P = 10^{\frac{-Q}{10}}$
- Probability of incorrect identification $Q_{10} \rightarrow P=\frac{1}{10}$, $Q_{20} \rightarrow P=\frac{1}{100}$, $Q_{30} \rightarrow P=\frac{1}{1000}$,...
- Example: `J` $\rightarrow$ code ASCII `74` $\rightarrow Q=74-33=41 \rightarrow P=10^{-4.1} = 7,9433e^{-5} = \frac{1}{12589}$ 

### 1.6. QC on fastq files, with *FastQC*

Quality Control of the `fastq` files produced by the sequencer
- Quality preservation along the Reads
- Overall Quality of each Read
- GC% offset and base repartition might indicate contamination

> [!info] ***Initialize Project***
> ```bash
> cd ; 
> cp -R /home/tludwig/master2ggb/tp4 .;
> cd tp4;
> ```

> [!info] ***Check fastq file with fastqc***
> ```bash
> fastqc
> ```
> File, Open :  data / father.R1.L001.fastq.gz

#### Good Per Base Quality

![](http://lysine.univ-brest.fr/~tludwig/good_per_base_quality.png)

#### Bad Per Base Quality

![](http://lysine.univ-brest.fr/~tludwig/bad_per_base_quality.png)

#### Good Per Sequence Quality

![](http://lysine.univ-brest.fr/~tludwig/good_per_sequence_quality.png)

#### Bad Per Sequence Quality

![](http://lysine.univ-brest.fr/~tludwig/bad_per_sequence_quality.png)

### 1.7. Alignment

Map each read to a reference genome

#### Several Algorithms available

| Aglorithm | Sensitivity <100bp | Sensitivity >100bp | Specificity <100bp | Specificity >100bp | Speed <100bp | Speed >100bp  | Tandem Low | Tandem High |
|---|---|---|---|---|---|---|---|---|
|BWA | :star: | :star::star::star: | :star::star: | :star::star::star: | :star::star::star: | :star::star::star: | :star::star: | :star: |
|Bowtie2 | :star: | :star::star::star: | :star: | :star: | :star::star: | :star::star: | :star::star: | :star: |
|NovoAlign | :star::star::star: | :star::star::star: | :star::star: | :star::star::star: | :star: | :star: | :star::star: | :star: |
|Smalt | :star: | :star::star::star: | :star: | :star: | :star::star: | :star::star: | :star::star: | :star: |
|Stampy | :star::star: | :star::star::star: | :star::star: | :star::star::star: | :star: | :star: | :star::star: | :star: |

An alignment *aligns* reads from a `fastq` file to a reference genome (`fasta` file) and produces a `SAM`/`BAM`/`CRAM` file

> [!info] ***align fastq files in sam files***
> `./script1.sh` 
> ```bash
> #!/bin/bash
> for lane in L001 L002 L003; do
>     echo "Aligning from father lane : $lane";
>     bwa mem -M ~/tp4/resources/reference.b37.fasta \
>     ~/tp4/data/father.R1.$lane.fastq.gz \
>     ~/tp4/data/father.R2.$lane.fastq.gz \
>    > ~/tp4/results/father.$lane.sam ;
done
> ```

#### A view from *IGV*

![](http://lysine.univ-brest.fr/~tludwig/igv.png)

#### **SAM** (**S**equence **A**lignment **M**ap) file format

| #   | Name  | Type   | Description                       |
|-----|-------|--------|-----------------------------------|
| 1   | NAME  | String | NAME                              |
| 2   | FLAG  | Int    | bitwise FLAG                      |
| 3   | RNAME | String | Reference sequence NAME  (chr)    |
| 4   | POS   | Int    | 1-based leftmost mapping POSition |
| 5   | MAPQ  | Int    | MAPping Quality                   |
| 6   | CIGAR | String | CIGAR string                      |
| 7   | RNEXT | String | Ref. name of the mate read (chr)  |
| 8   | PNEXT | Int    | Position of the mate read         |
| 9   | TLEN  | Int    | observed Template LENgth          |
| 10  | SEQ   | String | segment SEQuence                  |
| 11  | QUAL  | String | ASCII base QUALity                |
| 12+ | PGM   | String | Program Annotations               |

#### Compressed formats for SAM files

`SAM` files are very **BIG**, so there are 2 binary formats that intend to reduce this size:
- **BAM** $\frac{size}{4}$: gzipped version of the **SAM**, most used format
- **CRAM** $\frac{size}{7}$: increasingly used format. The reference genome `fasta` file is needed to read back the `CRAM` file

### 1.8. Processing BAM files

#### Merging

For example, when there is one file per lane and you want one file per sample

> [!info] ***merge multiple sam files into an unique bam file***
> `./script2.sh`
> ```bash
> #!/bin/bash
> picard-tools MergeSamFiles \
>     I=~/tp4/results/father.L001.sam \
>     I=~/tp4/results/father.L002.sam \
>     I=~/tp4/results/father.L003.sam \
>     O=~/tp4/results/father.bam \
>     SO=unsorted \
>     USE_THREADING=true ;
> ```

#### Sorting 

The **reads** are ordered by coordinates. This allows for quick search and extraction.

> [!info] ***sort bam file (with picard or sambamba)***
> `./script3.sh`
> ```bash
> #!/bin/bash
> sambamba sort ~/tp4/results/father.bam -o ~/tp4/results/father.sorted.bam;
> ```

#### Marking Duplicates

Points out PCR artefacts.

Otherwise, one given read could falsely be present several hundreds times, and completly alter the subsequent variant calling.

> [!warning] Avoid this step in the case of amplicons.

> [!info] ***mark duplicates in bam file (with picard or sambamba)***
> `./script4.sh`
> ```bash
> #!/bin/bash
> sambamba markdup ~/tp4/results/father.sorted.bam  ~/tp4/results/father.dedup.bam ;
> ```

#### Set Read Groups

A read group refers to a set of reads that are generated from a single run of a sequencer. 

In the simple case where a single library preparation derived from a single biological sample was run on a single lane of a flow cell, all the reads from that lane run belong to the same read group. When multiplexing is involved, then each subset of reads originating from a separate library run on that lane will constitute a separate read group.

Read groups have various properties:
- **ID**: Read group **Id**entifier
- **PU**: **P**latform **U**nit
- **SM**: **S**a**m**ple
- **PL**: **Pl**atform/technology used to produce the read
- **LB**: DNA preparation **l**i**b**rary identifier

> [!info] ***create read groups***
> `./script5.sh`
> ```bash
> #!/bin/bash
> picard-tools AddOrReplaceReadGroups \
>     I=~/tp4/results/father.dedup.bam \
>     O=~/tp4/results/father.withRG.bam \
>     RGLB=lib1234 \
>     RGPL=ILLUMINA \
>     RGPU=Unit17 \
>     RGSM=FAM001FATHER \
>     RGCN=SeqCenter ;
> ```

#### Indel Local Realignment

Improves Indel quality in subsequent variant calling (useful for legacy callers such as *UnifiedGenotyper* and *MuTect*, Redundant with new callers like *HaplotypeCaller* or *MuTect2*).

> [!info] ***index bam file (with picard or sambamba)***
> `./script6.sh`
> ```bash
> #!/bin/bash
> sambamba index ~/tp4/results/father.withRG.bam ;
> ```

> [!info] ***realign indels***
> `./script7.sh`
> ```bash
> #!/bin/bash
> GATK -T IndelRealigner \
>     -I ~/tp4/results/father.withRG.bam \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -targetIntervals 12 \
>     -known ~/tp4/resources/Mills_and_1000G_gold_standard.indels.b37.vcf.gz \
>     -known ~/tp4/resources/1000G_phase1.indels.b37.vcf.gz \
>     -o ~/tp4/results/father.real.bam ;
> ```

#### Base quality score recalibration

Adjusts systematic techinical error regarding base call quality, using machine learning to model errors empirically.

> [!info] ***tabulate data for recalibration***
> `./script8.sh`
> ```bash
> #!/bin/bash
> GATK -T BaseRecalibrator \
>     -I ~/tp4/results/father.real.bam \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -knownSites ~/tp4/resources/dbSNP.150.b37.vcf.gz \
>     -knownSites ~/tp4/resources/Mills_and_1000G_gold_standard.indels.b37.vcf.gz \
>     -knownSites ~/tp4/resources/1000G_phase1.indels.b37.vcf.gz \
>     -o ~/tp4/results/father.table ;
> ```

> [!info] ***recalibrate bam file***
> `./script9.sh`
> ```bash
> #!/bin/bash
> GATK -T PrintReads \
>     -I ~/tp4/results/father.real.bam \
>     -BQSR ~/tp4/results/father.table \
>     -o ~/tp4/results/father.bqsr.bam \
>     -R ~/tp4/resources/reference.b37.fasta ;
> ```

> [!info] ***Look at the alignment***
> ```bash
> igv results/father.bqsr.bam 12:497992
> ```

### 1.9. Calling Variants

Use the information in the alignment to infer the genotype of the individual at each genomic position

#### Several Algorithms

| Algorithm | **TP** | **FP** |
|---|---|---|
| **HaplotypeCaller** | 21,707 | 367    |
| FreeBayes           | 23,857 | 18,256 |
| SAMtools Mpileup    | 25,081 | 2,129  |
| SNPSVM              | 17,920 | 65     |

#### Algorithms for Somatic Variants

- MuTect2
- Strelka
- EBCall
- Seurat
- VarScan2

#### The VCF (and gVCF) file format

![](http://lysine.univ-brest.fr/~tludwig/vcf_format.png)

![](http://lysine.univ-brest.fr/~tludwig/VCFexplained.png)

#### VCF vs. gVCF file formats

- `VCF` files provide information only for sites where at least one sample has a variant allele
- `gVCF` files (as produced by *HaplotypeCaller*) provide a genotype for each position and not just variant positions. This is useful to differentiate missing calls from position homozygous to the reference genome.

> [!info] ***call variant of the bam alignment into a gVCF file***
> `./script10.sh`
> ```bash
> #!/bin/bash
> GATK -T HaplotypeCaller \
>     -I ~/tp4/results/father.bqsr.bam \
>     -o ~/tp4/results/father.gvcf.gz \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -L ~/tp4/resources/capture.kit.bed \
>     --interval_padding 100 \
>     --emitRefConfidence BP_RESOLUTION \
>     --variant_index_type LINEAR \
>     --variant_index_parameter 128000 \
>     -gt_mode DISCOVERY \
>     --dbsnp ~/tp4/resources/dbSNP.150.b37.vcf.gz ;
> # other options
> #    -nct 8 # for multithreading
> #    --dontUseSoftClippedBases # mostly for clinical data, if no further QC are done
> ```

#### Combining gVCF files 

- `gVCF` files produced by *HaplotypeCaller* contain a single sample
- The actual genotyping (next step) will need to look simultaneously at the genotype of each sample
- If a large (200+) amount of samples is present, this can be demanding (as the machine needs to keep a great number of very large files open at the same time)
- Combining several mono-sample `gVCF` files into fewer multi-sample files can greatly speed-up the genotyping and be less resource demanding.

![](http://lysine.univ-brest.fr/~tludwig/gvcfscombine.png)

> [!info] ***combine multiple mono-sample gvcf files into a unique multi-sample gvcf file***
> `./script11.sh`
> ```bash
> #!/bin/bash
> for fam in fam1 fam2 fam3;
> do
>     GATK -T CombineGVCFs \
>         -o ~/tp4/results/${fam}.gvcf.gz \
>         -R ~/tp4/resources/reference.b37.fasta \
>         -V ~/tp4/data/${fam}-E.gvcf.gz \
>         -V ~/tp4/data/${fam}-M.gvcf.gz \
>         -V ~/tp4/data/${fam}-P.gvcf.gz ;
> done
>```

#### Genotyping gVCF files

- This step looks at the genotype of each sample at each position
- It produces biallelic or multiallelic variants and affects genotypes to each sample
- Only variant sites are kept
- Several information (statistics, local frequencies, quality, etc.) are also provided.

> [!info] ***genotype multiple gVCF file into a VCF file***
> `./script12.sh`
> ```bash
> #!/bin/bash
> GATK -T GenotypeGVCFs \
>     -o ~/tp4/results/genotyped.vcf.gz \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -V ~/tp4/results/fam1.gvcf.gz \
>     -V ~/tp4/results/fam2.gvcf.gz \
>     -V ~/tp4/results/fam3.gvcf.gz ;
> ```

### 1.10. Recalibrating Variant Quality Scores

- Calculates a new quality score called **VQSLOD** (**V**ariant **Q**uality**S**core **L**og-**Od**ds)
- Allowing to finely balance sensitivity (less FNs) and specificity(less FPs)
- Uses Machine Learning to affect a score based on various contextual information
- Trains on highly validated variant data (omni, 1000g, hapmap)
- :warning: Large amount of data is needed to perform this step! At least exome wide and with as many samples as possible
- The user defines a set of quality tranches, containing a certain amount of True/False Positives
- Later, these tranches will be used to filter out data, depending on the needs.

![](http://lysine.univ-brest.fr/~tludwig/vqsr.png)

 > [!warning] given as an example, won't work on such a small dataset       :warning: 

> [!info] ***recalibrate variant quality scores***
> `./script13.sh`
> ```bash
> #!/bin/bash
> # Step1 Compute Recalibration for SNPs
> GATK -T VariantRecalibrator \
>     -mode SNP \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -input ~/tp4/results/genotyped.vcf.gz \
>     -resource:hapmap,known=false,training=true,truth=true,prior=15.0 \
>     ~/tp4/resources/hapmap_3.3.b37.vcf.gz \
>     -resource:omni,known=false,training=true,truth=true,prior=12.0 \
>     ~/tp4/resources/1000G_omni2.5.b37.vcf.gz \
>     -resource:1000G,known=false,training=true,truth=false,prior=10.0 \
>     ~/tp4/resources/1000G_phase1.snps.high_confidence.b37.vcf.gz \
>     -resource:dbsnp,known=true,training=false,truth=false,prior=2.0 \
>     ~/tp4/resources/dbSNP.150.b37.vcf.gz \
>     -an DP -an QD -an FS -an SOR -an MQ -an MQRankSum -an ReadPosRankSum \
>     -tranche 100.0 \
>     -tranche 99.9 \
>     -tranche 99.0 \
>     -tranche 90.0 \
>     -recalFile ~/tp4/results/snp.recalFile \
>     -tranchesFile ~/tp4/results/snp.tranchesFile \
 >    -rscriptFile ~/tp4/results/out.SNP.R ;
>  
> # Step2 Apply Recalibration for SNPs
> GATK -T ApplyRecalibration \
>     -mode SNP \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -input ~/tp4/results/genotyped.vcf.gz \
>     -o ~/tp4/results/recal.SNP.vcf.gz \
>     -recalFile ~/tp4/results/snp.recalFile \
>     -tranchesFile ~/tp4/results/snp.tranchesFile \
>     --ts_filter_level 90 ;
>     
> # Step3 Compute Recalibration for INDELs
> GATK -T VariantRecalibrator \
>     -mode INDEL \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -input ~/tp4/results/genotyped.vcf.gz \
>     -resource:mills,known=true,training=true,truth=true,prior=12.0 \
>     ~/tp4/resources/Mills_and_1000G_gold_standard.indels.b37.vcf.gz \
>     -an DP -an QD -an FS -an SOR -an MQRankSum -an ReadPosRankSum \
>     -tranche 100.0 \
>     -tranche 99.9 \
>     -tranche 99.0 \
>     -tranche 90.0 \
>     --maxGaussians 4 \
>     -recalFile ~/tp4/results/indel.recalFile \
>     -tranchesFile ~/tp4/results/indel.tranchesFile \
>     -rscriptFile ~/tp4/results/out.INDEL.R ;
>     
> # Step4 Apply Recalibration for INDELs
> GATK -T ApplyRecalibration \
>     -mode INDEL \
>     -R ~/tp4/resources/reference.b37.fasta \
>     -input ~/tp4/results/genotyped.vcf.gz \
>     -o ~/tp4/results/recal.INDEL.vcf.gz \
>     -recalFile ~/tp4/results/indel.recalFile \
>     -tranchesFile ~/tp4/results/indel.tranchesFile \
 >    --ts_filter_level ;
> ```

> [!warning] won't work as the `script13` was not executed :warning: 

> [!info] ***merge snp and indel recalibrated files***
> `./script14.sh`
> ```bash
> #!/bin/bash
> dir="~/tp4/results";
> snp="$dir/recal.SNP.vcf.gz";
> indel="$dir/recal.INDEL.vcf.gz";
> outsnp="$dir/VQSR.SNP.vcf.gz";
> outindel="$dir/VQSR.INDEL.vcf.gz";
> output="$dir/VQSR.vcf";
> 
> # extract SNP
 >echo "Extract SNP to $outsnp";
> zcat $snp | awk ’{if(($1 ~ "^#") || ($7 != ".")) print $0;}’ | bgzip > > $outsnp;
> tabix $outsnp;
> 
> # extract INDEL
> echo "Extract INDEL to $outindel";
> zcat $indel | awk ’{if(($1 ~ "^#") || ($7 != ".")) print $0;}’ | bgzip > > $outindel;
> tabix $outindel;
> 
> # merge SNP+INDEL
> echo "Merge to $output";
> head -2000 $outsnp  | grep "^#" | cat -n > head.snp
> head -2000 $outindel | grep "^#" | cat -n > head.indel
> cat head.snp head.indel | sort -n | uniq | cut -f2- > $output
> rm head.snp head.indel
> cat $outsnp $outindel | grep -v "^#" | sort -V >> $output;
> echo "bgzip $output";
> bgzip $output;
> echo "Tabix";
> tabix $output.gz;
> ```

### 1.11. Variant Annotation

Adding information from external sources:
- Genes/Transcripts Impacted
- Consequences
- Phenotypical Information
- Validation
- Frequencies from different populations

Several Annotation Softwares:
- [ANNOVAR](https://annovar.openbioinformatics.org/en/latest/)
- [snpEff](http://pcingola.github.io/SnpEff/)
- [vep (Variant Effect Predictor)](https://www.ensembl.org/info/docs/tools/vep/index.html)
- [VarAFT](https://varaft.eu/) (INSERM U1251)

### 1.13. Regions

Sometimes we want to work on a subset of variants, defined by regions. Those regions can for example match a gene/set of genes or the positions taken into account by the capture kit.

The prevaling format to define regions is the ***BED*** (**B**rowser **E**xtensible **D**ata) file format. It a tabulated file, with the following columns:

|#| Name | Description|
|---|---|---|
| 1 | chrom      | Chromosome (e.g. chr3, chrY, chr2_random) or scaffold (e.g. scaffold10671) name |
| 2 | chromStart | Start coordinate on the chromosome or scaffold for the sequence considered. In base$_0$ |
| 3 | chromEnd   | End coordinate on the chromosome or scaffold for the sequence considered. In base$_1$ |

> [!info] - base$_0$ means that the first nulceotide of the the chromosome is 0
> - base$_1$ means that the first nulceotide of the the chromosome is 1 (can also be view as base$_0$ excluded : `[start;end[`)
> 
> VCF File are numbered in base$_1$, so if you want the chromosome 2 on position 11,12,13,14 et 15 you don't write `chr2 11 15` but `chr2 10 15` and for just the position 17, you write `chr 16 17`

### 1.12. Searching for Candidate Variants

The variant filtering, selection and priorization is based on several criteria:
- Variant Type (SNV, INDEL, SV)
- Consequence on the transcript/protein
- Frequency
- Genotypes and transmission pattern
- Affected Gene/Pathway 

---

## Part 2: Identifying Candidate Variants

> [!success] ***Medical Information***
> - Suspected recessive disease
> - Thickened, misshaped fingernails and toenails
> - Palmar keratoderma
> - Oral leukokeratosis
> - Cysts, including steatocystoma multiplex
> - Follicular hyperkeratosis
> - Blisters
> - Excessive sweating of the palms and soles

### 2.1. Annotation with variant effect predictor

Type this command
<!--`vep ~/tp4/data/family.vcf.gz ~/tp4/results/family.annotated.vcf`-->
`cp ~/tp4/data/family.annotated.vcf  ~/tp4/results/family.annotated.vcf`

Details: 
```bash
#! /bin/sh
invcf=$1;
outvcf=$2;
vep=/PROGS/EXTERN/EnsEMBL/latest/vep
VEP_CACHE_PATH=/PUBLIC_DATA/Annotation/VEP/VEP_CACHE.latest
cache="--cache --merged --offline --dir $VEP_CACHE_PATH"
speed="--fork 24 --buffer_size 25000"
species="homo_sapiens"
assembly="GRCh37"
fasta="$VEP_CACHE_PATH/$species/91_$assembly/fasta.fa.gz"
annotations=""
annotations="$annotations --use_given_ref"
annotations="$annotations  --check_existing"
annotations="$annotations --allele_number"
annotations="$annotations --canonical"
annotations="$annotations --biotype"
annotations="$annotations --symbol"
annotations="$annotations --numbers"
annotations="$annotations --domains"
annotations="$annotations --sift b"
annotations="$annotations --polyphen b"
annotations="$annotations --regulatory"
annotations="$annotations --gene_phenotype"
annotations="$annotations --af"
annotations="$annotations --max_af"
annotations="$annotations --af_1kg"
annotations="$annotations --af_esp"
annotations="$annotations --af_gnomad"
annotations="$annotations --hgvs --hgvsg --fasta $fasta"
annotations="$annotations --plugin dbNSFP,/PUBLIC_DATA/Annotation/dbNSFP/2.9/dbNSFP.gz,LRT_score,GERP++_RS"  #GRCh37
annotations="$annotations --plugin CADD,/PUBLIC_DATA/Annotation/CADD/CADD_v1.3/whole_genome_SNVs.tsv.gz"
annotations="$annotations --plugin LoFtool,/PUBLIC_DATA/Annotation/VEP/VEP_CACHE.latest/Plugins/LoFtool_scores.txt"
options="$cache $speed --species $species --assembly $assembly $annotations --vcf"
command="$vep $options -i $invcf -o $outvcf"
echo "Running: $command"
$command
```

### 2.2. Filtering with VCFProcessor

Documentation : [http://lysine.univ-brest.fr/vcfprocessor/](http://lysine.univ-brest.fr/vcfprocessor/)

Get VCFProcessor
```bash
git clone https://github.com/ThomasLudwig/VCFProcessor/
cd VCFProcessor/
chmod +x gradlew
./gradlew Deploy
cd ..
ln -s VCFProcessor/build/libs/VCFProcessor.jar
```

Keep only variants compatible with a recessive transmission pattern
```bash
java -jar VCFProcessor.jar Recessive \
    --vcf ~/tp4/results/family.annotated.vcf \
    --ped ~/tp4/data/family.ped \
    --missing false \
    --nohomo true \
    --mode strict \
    --out ~/tp4/results/recessive.vcf
```

Keep only rare variants 
```bash
java -jar VCFProcessor.jar FilterGnomADFrequency \
    --vcf ~/tp4/results/recessive.vcf \
    --threshold 0.05 \
    --out ~/tp4/results/0.05.recessive.vcf
```

Keep only variants that strongly affect the protein
```bash
java -jar VCFProcessor.jar FilterConsequenceLevel \
    --vcf ~/tp4/results/0.05.recessive.vcf \
    --csq splice_region_variant \
    --out ~/tp4/results/effect.0.05.recessive.vcf
```

### 2.3. Results

The last command shows a list of genes.
```bash
java -jar VCFProcessor.jar GeneList \
    --vcf ~/tp4/results/effect.0.05.recessive.vcf \
    --out ~/tp4/results/effect.0.05.recessive.gene.list
```

Which gene (and thus variant) seems to be the best candidate to explain the patient's phenotype ?