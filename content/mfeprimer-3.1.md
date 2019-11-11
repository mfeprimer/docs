---
weight: 1
bookFlatSection: true
title: "MFEprimer-3.1"
type: docs
---

# MFEprimer-3.1

Big change from v3.0

## Major changes from v3.0

- merge several tools into one **single execute file** `mfeprimer`
- support major platforms: Linux, Mac and Windows
- bug fixed and **faster** than v3.0
- fix **thermodynamics** calculation bugs for dimers and hairpins
- **local web server** version is available

## Summary of different versions

Before getting started, I'd like to make a summary for different versions. Please choose the right version for you. 

| Title          | web server by the author  | command-line                                                 | local web server                       |
| -------------- | ------------------------- | ------------------------------------------------------------ | -------------------------------------- |
| primer number  | <= 50                     | no limit                                                     | usually <= 50                          |
| database       | pre-indexed by the author | no limit                                                     | no limit                               |
| user interface | graphic and easy to use   | terminal and requires essential Linux skills                 | graphic and easy to use                |
| users from     | worldwide                 | the owner                                                    | usually all members of the institution |
| workflow       | no                        | yes and can be integrated into user defined primer design workflows | no                                     |

## 1. Web servers maintained by the author‌

1. https://mfeprimer3.igenetech.com/ [version 3.0] web server funded by [iGeneTech](http://www.igenetech.com/), a company aims to provide kit and service for target capture and sequencing, also is the company the author works for.
2. http://124.207.243.56:8081/ [version 3.1, for test] web server funded by Prof. Dongsheng Zhao (AMMS, 军事医学科学院). Currently, there is only human genome (hg19) available and will add other genomes later.

## 2. Command-line version

### Install

1. Please download the right version for your machine: https://github.com/quwubin/MFEprimer-3.0/releases (take Linux version as an example).
2. Uncompress the file.
3. Make sure the file has the execute permission.
4. Run with option "-h" to get help message. You can rename the file to `mfeprimer` as you wish.

``` bash
# download the executable mfeprimer file
wget -c https://github.com/quwubin/MFEprimer-3.0/releases/download/v3.1.0/mfeprimer-3.1.0-linux-amd64.gz


# uncompress the file
gzip -d mfeprimer-3.1.0-linux-amd64.gz


# make it executable
chmod +x mfeprimer-3.1.0-linux-amd64


# test it
./mfeprimer-3.1.0-linux-amd64 -h


MFEprimer-3.1.0, a functional primer quality control program for checking
non-specific amplicons, dimers, hairpins and others.


Web site: https://www.mfeprimer.com/


Cite: Wang, K., Li, H., Xu, Y., Shao, Q., Yi, J., Wang, R., … Qu, W. (2019).
MFEprimer-3.0: quality control for PCR primers. Nucleic Acids Research.
https://doi.org/10.1093/nar/gkz351.


Usage:
  mfeprimer [flags]
  mfeprimer [command]


Available Commands:
  dimer       analysis primer dimers
  hairpin     analysis primer hairpins
  help        Help about any command
  index       index fasta file for mfeprimer
  indexcmp    Compare the old and the new index file, return false when they are not same
  spec        analysis specificity of primers
  version     print version number


Flags:
  -b, --bind           print specific and nonspecific binding sites and patterns for each primer
  -d, --db strings     [*] database indexed for specificity check: -d hg19.fa -d mrna.fa
      --diva float     concentration of divalent cations [mM] (default 1.5)
      --dntp float     concentration of dNTPs [mM] (default 0.25)
  -h, --help           help for mfeprimer
  -i, --in string      [*] a file contains primer sequences in fasta format
  -j, --json           output in json format
  -k, --kvalue int     k value for format, defaut is 9 (default 9)
  -S, --maxSize int    max product size for the predicted amplicons [bp] (default 2000)
  -s, --minSize int    min product size for the predicted amplicons [bp]
      --misEnd int     mis-match ends to the position of 3' end, 9 (kvalue) for the 9th base from the 3'end (default 9)
      --misMatch int   max allowed mismatches between kmer and its binding sites
      --misStart int   mis-match starts from the position of 3' end, 1 is for the very end of primer. (default 1)
      --mono float     concentration of monovalent cations [mM] (default 50)
      --oligo float    concentration of annealing oligos [nM] (default 50)
  -o, --out string     output file name, e.g., primer.mfe.txt
      --snp string     SNP file name in bed format, e.g., dbsnp151.bed
  -t, --tm float       tm cutoff value to filter the amplicons (default 30)


Use "mfeprimer [command] --help" for more information about a command.
```

> In v3.1.0, I make `index`,`dimer`,`hairpin` as sub-commands to `mfeprimer`, each sub-command has their own arguments and options to set. 
> 
> I manually tested the Mac and Linux version, and I will find time to test the Windows version.

### Usage

‌

I'd like to do some examples with sample files, let's prepare the test files:


``` bash
# make a test directory
mkdir test
cd test


# chrM.fa as database
wget -c https://github.com/quwubin/MFEprimer-3.0/raw/master/chrM.fa


# p.fa test primer sequences in fasta format
wget -c https://github.com/quwubin/MFEprimer-3.0/raw/master/p.fa


# snp in bed format to check whether a primers bind to a snp site
wget -c https://github.com/quwubin/MFEprimer-3.0/raw/master/snp.bed


# make a softlink
ln -s ../mfeprimer-3.1.0-linux-amd64 mfeprimer 
```

The file tree is:

``` bash
tree .
.
├── chrM.fa
├── mfeprimer -> ../mfeprimer-3.1.0-linux-amd64
├── p.fa
└── snp.bed
```

### Index databases

``` bash
./mfeprimer index -i chrM.fa
```

After index, the file tree is:

``` bash
tree .
.
├── chrM.fa
├── chrM.fa.fai
├── chrM.fa.json
├── chrM.fa.log
├── chrM.fa.primerqc
├── chrM.fa.primerqc.fai
├── mfeprimer -> ../mfeprimer-3.1.0-linux-amd64
├── p.fa
└── snp.bed
```

> `mfeprimer index` will add 5 files with suffixes: ".fai", ".json", ".primerqc", ".primerqc.fai" and ".log".

### Run `mfeprimer` for full quality control

``` bash
# run mfeprimer
./mfeprimer -i p.fa -d chrM.fa
```

Without "-o" options, the result will print to the screen:

``` bash
MFEprimer-3.0 Primer Quality Reports (2019-10-17 11:29:18)


Primer ID                      Sequence (5'-->3')                    Length     GC      Tm      Dg        Binding Number
                                                                      (bp)      (%)    (°C)  (kcal/mol)   Plus     Minus


p1                             CTACAACCCCACCACGTACC                      20   60.00   60.55    -22.23         0         0
p2                             CGTTACACACTTTGCGGCAA                      20   50.00   60.47    -22.43         0         0
p3                             CTTAAATAGGGACCTGTATGAATGGCTC              28   42.86   61.98    -27.11         2         1
p4                             CCGAAATTTTTAATGCAGGTTTGGTAGT              28   35.71   61.97    -27.14         0         1
p5                             GGACACTCTATGGGAAAGAGTGTCC                 25   52.00   62.91    -26.07         0         1


Hairpin List (1)

Hairpin 1: p5

    Score: 9, Tm = 26.56 °C, Delta G = -4.48 kcal/mol

    /////////-------\\\\\\\\\
    GGACACTCTATGGGAAAGAGTGTCC


Dimer List (1)

Dimer 1: p2 x p2


    Score: 7, Tm = 14.55 °C, Delta G = -6.09 kcal/mol


CGTTACACACTTTGCGGCAA
           ::::.::::
           AACGGCGTTTCACACATTGC



Descriptions of [ 1 ] potential amplicons


AmpID HitID                                            Size       FpTm       RpTm       FpDg       RpDg
                                                       (bp)       (°C)       (°C)   kcal/mol   kcal/mol


1     chrM                                              200      61.93      62.67     -27.40     -28.08



Amplicon details


Amp 1: p3 + p4 ==> chrM

    Size = 200 bp, GC content = 43.50%
    F: Tm = 61.93 °C, Delta G = -27.40 kcal/mol
    R: Tm = 62.67 °C, Delta G = -28.08 kcal/mol
    Binding sites: 2612(28/28) ... 2811(28/28)

>>>p3
   1                          28
5' CTTAAATAGGGACCTGTATGAATGGCTC 3'
   ::::::::::::::::::::::::::::                                                            2811
5' CTTAAATAGGGACCTGTATGAATGGCTCcacgagggttcagct...acccacaggtcctaaACTACCAAACCTGCATTAAAAATTTCGG 3'
   2612                                                         ::::::::::::::::::::::::::::
                                                             3' ACTACCAAACCTGCATTAAAAATTTCGG 5'
                                                               28                          1
                                                                                          p4<<<

>Amp_1 p3 + p4 ==> chrM
CTTAAATAGGGACCTGTATGAATGGCTCcacgagggttcagctgtctcttacttttaaccagtgaaattgacctgcccgt
gaagaggcgggcataacacagcaagacgagaagaccctatggagctttaatttattaatgcaaacagtacctaacaaacc
cacaggtcctaaACTACCAAACCTGCATTAAAAATTTCGG




Parameters


                   Primer file: p.fa
                      Database: chrM.fa
                        Kvalue: 9
                      MisMatch: 0
                Tm cutoff (°C): 30.00
        Amplicon min size (bp): 0
        Amplicon max size (bp): 2000
                           SNP:
       Monovalent cations [mM]: 50.00
         Divalent cations [mM]: 1.50
                    dNTPs [mM]: 0.25
         Annealing oligos [nM]: 50.00



Cite

Kun Wang, Haiwei Li, Yue Xu, Qianzhi Shao, Jianming Yi, Ruichao Wang, Wanshi Cai, Xingyi Hang,
Chenggang Zhang, Haoyang Cai, Wubin Qu, MFEprimer-3.0: quality control for PCR primers,
Nucleic Acids Research, Volume 47, Issue W1, 02 July 2019, Pages W610–W613,
https://doi.org/10.1093/nar/gkz351


Web & Contact

https://www.mfeprimer.com
Wubin Qu <quwubin@gmail.com>


Total time used: 81.641035ms.
```

The relation between `mfeprimer` and its sub-commands: `mfeprimer` do a full quality control for primers for specificity, binding sites, dimers and hairpins. While, `spec` only for specificity, `dimer` is only for dimers and `hairpin` is only for hairpins, as their names indicated. The sub-commands have more options to control the behavious. `mfeprimer` only have default parameters for dimers and hairpins.

‌

### Run `mfeprimer spec` for specificity

``` bash
./mfeprimer spec -i p.fa -d chrM.fa
```

‌

### Run `mfeprimer dimer` for dimers

``` bash
./mfeprimer dimer -i p.fa
```

### Run `mfeprimer hairpin` for hairpins

``` bash
./mfeprimer hairpin -i p.fa
```

## 3. Local web server

### Download and install

1. Please download the right version for your machine: https://github.com/quwubin/MFEprimer-3.0/releases (take the Mac version as an example).
2. Uncompress the file.
3. Make sure the file has the execute permission.
4. Run with option "-h" to get help message. You can rename the file to `mfeprimer` as you wish.

``` bash
# download the executable mfeprimer-web file
wget -c https://github.com/quwubin/MFEprimer-3.0/releases/download/v3.1.1/mfeprimer-web-3.1.1-darwin-amd64.tar.gz

# uncompress the file
tar zxvf mfeprimer-web-3.1.1-darwin-amd64.tar.gz

# The file tree is:
tree -L 2

.
├── conf
│   └── app.conf
├── mfedb
│   ├── chrM.fa
│   ├── chrM.fa.fai
│   ├── chrM.fa.json
│   ├── chrM.fa.log
│   ├── chrM.fa.primerqc
│   └── chrM.fa.primerqc.fai
├── mfeprimer-web
├── static
│   ├── css
│   ├── fonts
│   ├── img
│   └── js
└── views
    ├── dimer
    ├── hairpin
    ├── layout.html
    ├── seq
    └── spec


# test it
./mfeprimer-web

# open your browser and enter the following address, and you will see the default page.

http://localhost:8080/
```

![mfepriemr-web front page](https://tva1.sinaimg.cn/large/006y8mN6ly1g8txd89sf6j31au0u04is.jpg)

### Configure the server

Open the "conf/app.conf", change the corresponding item following the references in the file.

### Add database

There is an example database named "chrM.fa" in "mfedb" directory. You can place any database here 
and index the database with command `mfeprimer index -i dbname.fasta`.
