# Generating SNPs files from VCF file and use it for genetic analysis

## General Description
Description: this file is used to explain how to convert sorghum genotype data.


step1: convert VCF file to Hapmap file (let’s take filteredSNP,imp.all.hmp.yan.SNP_Matrix.noduplicated.genotypes.vcf for example)
	（1）use TASSEL software to convert vcf file to Hapmap file. (307_hmp.txt is Hapmap file formed by QiFu) 
	 (2) delete pound keys (#) in the header of Hapmap file and then read it into R software.(see Script1.R)
	 (3) then generate Sorghum_genotype_307_hmp.txt	 
    
Step2: use Aaron’s script to filter genotype data based on max missing rate is 0.6 and minor allele frequency is 0.05
	(1) https://github.com/amkusmec/snptools （this is a website developed by Aaron）
	(2)for more details, please check Linux_command2.txt file

## Transform hmp file to .ped and .map

This step will generate genotypic files for GWAS (.xmat and .map file) and LD calculation via plink (.ped and .map)

Description: this file is used to display how to use Aaron’s script to convert Hapmap file to numerical file
Author: QiFu & Yan Zhou (last modified by Yan Zhou at Oct 15th, 2018)
	please visit below website for more details
	https://github.com/amkusmec/snptools （this is a website developed by Aaron）

Step1: Download snptools-master folder from above website and make a subfolder named AB
	cd snptools-master
	mkdir AB

Step2: Put Hapmap file in AB subfolder (Let’s take Sorghum_genotype_307_hmp.txt	for example)

Step3: use Hapmap file to generate corresponding .stat file 
	cd src/
	python3 snpstat.py -p ../AB/ -i Sorghum_genotype_307_hmp.txt -o ../AB/307.stat -mi 2

Step4: Filter Hapmap file
	python3 filter.py -p ../AB/ -i Sorghum_genotype_307_hmp.txt -s 307.stat -o ../AB/Sorghum_genotype_307_filter -mi 2 -n 0.6 -f 0.05
then, Sorghum_genotype_307_filter.hmp.txt and Sorghum_genotype_307_filter_filtered.hmp.txt were generated.

Step5: use Sorghum_genotype_307_filter.hmp.txt file to get corresponding .stat file
	python3 snpstat.py -p ../AB/ -i Sorghum_genotype_307_filter.hmp.txt -o ../AB/307new.stat -mi 2

Step6: generate .xmat and .map file
	python3 numericalize.py - p ../AB/ -i Sorghum_genotype_307_filter.hmp.txt -s 307new.stat -o ../AB/307 -mi 2
then, 307.xmat file and 307.map file were generated

Step7: rename 307.xmat file
	mv 307.xamt myGD_filter_307.txt

Step8: generate .map file and .ped file 
	python3 convert.py -p ../AB/ -i Sorghum_genotype_307_filter.hmp.txt -o new_307numerical.ped -mi 2 -mo 3
then, new_307numerical.ped and new_307numerical.map were generated

## Calculate LD using plink

Description: this file is used to display how to perform LD decay analysis. This step requires .ped and .map file
Author: QiFu

Step1: use Plink software (version 1.07) to generate LD files.(Let’s take 307.ped, 307.map for example)
	plink --noweb --file 307 --missing-genotype N --r2 --ld-window 99999 --ld-window-r2 0 --out  307LD
then, Then three files were generated, which are  307LD.ld , 307LD.log, 307LD.nosex

Step2: use 307LD.ld to draw LD decay plot.

Note: for subset population (i.e. 300 or 238 or 272) we could directly modified this on .ped file using unix awk command

## Using plink generate .ped .bim .fam file for faststructure

## Use faststructure to estimate population structure.
