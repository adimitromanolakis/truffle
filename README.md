# TRUFFLE - Fast shared segment and ancestry estimation. 


# Quickstart


## Introduction 
Estimating relatedness and co-ancestry among pairs of individuals is a commonly encountered task in most association or 
population genetics studies. TRUFFLE enables the simple and accurate identification of IBD1 and 2 segments, 
calculation of total IBD1/2 probabilities and provide graphical reporting of distribution of shared segments 
across pairs of individuals. 


## Important Notes for this release

Truffle is currently under development. There might be serious bugs and issues that we are currently working on. 
If you use this version of truffle, we ask you to notify us of any problems or errors encountered. If truffle works perfectly, we
would also like your feedback.

## Quickstart

```sh
./truffle --vcf input.vcf.gz --cpu 4
```

Truffle can read most common vcf files as input to the segment detection algorithm. The file should, generally, contain
all chromosomes from all individuals. Currently, there is no support for joining vcf files from different chromosomes.

For most common use cases, the vcf file should contain autosomal chromosomes only.

## Output a list of segments

To generate a list of segments in the file truffle.segments use the option `--segments`:

```sh
./truffle --vcf input.vcf.gz --cpu 4 ---segments
```








## Segment detection sensitivity

Truffle has some build-in measures to adjust the sensitivity of the segments detection.

The default sensitivity threshold should produce reasonable but maybe not optimal, results for whole-genome autosomal marker panels. In many cases you might need to adjust it, for example when:

* running exome sequencing data
* analyzing data from mixed ethnicities
* analyzing single chromosomes

In most cases, you get more than expected relatedness and you might want to consider adjusting the parameter L to 1.5-3. For example:

```sh
./truffle --vcf input.vcf.gz --L 2
```



### OPTIONAL: LD-pruning and filtering steps for genotyping array data

The following commands show a suggested pipeline for performing LD-pruning of variants and minor allele frequency pruning before analysis:

```
plink --bfile input --maf 0.05 --mind 0.2 --make-bed --out /tmp/t
plink --bfile /tmp/t --maf 0.05 --geno 0.05 --make-bed --out /tmp/t

plink --bfile /tmp/t --indep-pairwise 2000 100 0.5 --out /tmp/t
cat /tmp/t.bim|awk '$1<=22' >/tmp/autosom.txt
plink --bfile /tmp/t --extract /tmp/t.prune.in --make-bed --out /tmp/t2

plink --bfile /tmp/t2 --extract /tmp/autosom.txt --recode-vcf --out /tmp/filtered
```






## Commonly used command line options

### Input/Output control


```
--vcf `vcffile
```

Reads the input `vcffile` for processing.

```
--segments
```
Output a list of identified segments in the file truffle.segments

```
--out X
```

Change the name of the output files. The results will be stored in the files X.ibd and X.segments

### CPU usage control


* `--cpu N `

Use N cpus when running truffle.


### Segment length threshold

* `--L X`

Adjust the sensitivity threshold for detecting segments to X. 



### Variant filtering

* `--maf *X* `



Remove variants that have a minor allele frequency less than X. For example:
`--maf 0.05`

* `--missing X `

Excludes variants with missing rate more than X. For example `--missing 0.02`

* `--mindist X`

Removes variants that are less than X base-pairs apart.For example `--mindist 2000`


### Other options

* `--pair ID1 ID2`
* `--ibs1 x`
* `--ibs2 x`
* 


 
