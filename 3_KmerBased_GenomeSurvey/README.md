Kmer - based genome survey
================

### What is a *k-mer*?

It’s a metematical concept representing substrings (*mers*) of a certain
length (*k*) contained within a biological sequence. *K-mers* are not
related to real, physical molecules.

![](https://raw.githubusercontent.com/jacopoM28/CompOmics_2022/main/Figures/kmers.png)

### Kmer based genome survey

1.  **Genome size estimation**
2.  **Repeat Content estimation**
3.  **Heterozigosity estimation**

For a given sequence of length L, and a k-mer size of k, the total k-mer’s possible (*n*) will be given by :

*( L – k ) + 1*

Since we sequenced a genome multiple time the total length (N) will be :

*n = [( L – k ) + 1] * C*

*N = n/C*  

In real case scenarios we have a **non-uniform** coverage of
the genome ( **Repetitive content**; **Sequencing errors**;
**heterozigosity**; ecc…)

-----

## Preliminary step: QC of short reads data

Necessary softwares :

1.  **[fastqc](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/)**
2.  **[trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic)**

##### Fastqc:

    fastqc seqfile1 seqfile2 .. seqfileN

##### Trim reads in a sliding window approach: **TRIMMOMATIC**:

    trimmomatic PE -threads 20 -phred33 SRR11672503_1.fastq.gz SRR11672503_2.fastq.gz SRR11672503_1_paired.fastq SRR11672503_1_unpaired.fastq SRR11672503_2_paired.fastq SRR11672503_2_unpaired.fa
    stq ILLUMINACLIP:/usr/local/anaconda3/share/trimmomatic-0.39-2/adapters/TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36 2> stats_trimmomatic

where :

  - ILLUMINACLIP:TruSeq3-PE.fa:2:30:10
      - path to adapters fasta;
      - seedMismatches: specifies the maximum mismatch count which will
        still allow a full match to be performed;
      - palindromeClipThreshold: specifies how accurate the match
        between the two ‘adapter ligated’ reads must be for PE
        palindrome read alignment;
      - simple clip threshold: the minimum score of the full alignment
        between adapter and read for the clipping to take place
  - LEADING:3 \#Remove leading low quality or N bases (below quality 3)
  - TRAILING:3 \#Remove trailing low quality or N bases (below quality
    3)
  - SLIDINGWINDOW:4:15
      - windowSize: specifies the number of bases to average across;
      - requiredQuality: specifies the average quality required.
  - MINLEN:36

See the
[manual](http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/TrimmomaticManual_V0.32.pdf)
for deeper explanation of parameters.

Other possibilities :

  - **bbduck** from [BBMap](https://sourceforge.net/projects/bbmap/)
    suite
  - **[TrimGalore](https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/)**

-----

## Compute *k-mer* frequency

[KAT](https://kat.readthedocs.io/en/latest/index.html);[Jellyfish](https://github.com/gmarcais/Jellyfish);[BBmap](https://sourceforge.net/projects/bbmap/)

    kat hist -t <THREADS> -m <MER LENGTH> -H <HASH size> -o <OUTPUT PREFIX>

## Genome size, heterozigosity and repetitive content estimation

[Genomescope2](http://qb.cshl.edu/genomescope/genomescope2.0/)

**Watch out for lower and upper count boundaries**

-----

#### Some other example

[*Ruditapes philippinarum*](<https://raw.githubusercontent.com/jacopoM28/CompOmics_2022/main/3_KmerBased_GenomeSurvey/Data/Rphil_kmer27.png>)
[*Reticulitermes lucifugus*](https://raw.githubusercontent.com/jacopoM28/CompOmics_2022/main/3_KmerBased_GenomeSurvey/Data/Rluc.kmc_30_Genomescope.png)
