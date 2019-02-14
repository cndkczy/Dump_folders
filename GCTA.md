#Use GCTA to calculate genetic correlation and heritability
For tutorial (https://cnsgenomics.com/software/gcta/#Overview)
Two improtant reference need read prior to this process:

  Yang J, Lee SH, Goddard ME and Visscher PM. GCTA: a tool for Genome-wide Complex Trait Analysis. Am J Hum Genet. 2011 Jan 88(1): 76-82. [PubMed ID: 21167468]
  Yang, J., Zeng, J., Goddard, M.E., Wray, N.R. and Visscher, P.M., 2017. Concepts, estimation and interpretation of SNP-based heritability. Nature genetics, 49(9), p.1304.

Using GCTA require the bed and fam file from plink, we could use plink to generate this files. Remember to use plink ld function to generate this file since it allow customrily setting missing genotype.
Generating the grm file is simple:
  gcta64  --bfile SAM_339 --make-grm  --out SAM339_f
  
  ##Heritability

From my own experience, at least for using WGS data, it is preferred to use GREML-LDMS method which It corrects for the LD bias in the estimated SNP-based heritability. There are detailed discussion at second reference that I listed at the beginning of this memo.

Codes are listed as below:
  ./gcta64 --bfile SAM_339_f --ld-score-region 200  --out SAM_339_f
  ===following codes are in R===
  lds_seg = read.table("SAM_339_f.score.ld",header=T,colClasses=c("character",rep("numeric",8)))
quartiles=summary(lds_seg$ldscore_SNP)

  lb1 = which(lds_seg$ldscore_SNP <= quartiles[2])
  lb2 = which(lds_seg$ldscore_SNP > quartiles[2] & lds_seg$ldscore_SNP <= quartiles[3])
  lb3 = which(lds_seg$ldscore_SNP > quartiles[3] & lds_seg$ldscore_SNP <= quartiles[5])
  lb4 = which(lds_seg$ldscore_SNP > quartiles[5])

  lb1_snp = lds_seg$SNP[lb1]
  lb2_snp = lds_seg$SNP[lb2]
  lb3_snp = lds_seg$SNP[lb3]
  lb4_snp = lds_seg$SNP[lb4]

  write.table(lb1_snp, "snp_group1.txt", row.names=F, quote=F, col.names=F)
  write.table(lb2_snp, "snp_group2.txt", row.names=F, quote=F, col.names=F)
  write.table(lb3_snp, "snp_group3.txt", row.names=F, quote=F, col.names=F)
  write.table(lb4_snp, "snp_group4.txt", row.names=F, quote=F, col.names=F)
  ====R code is finished===
  ./gcta64 --bfile SAM_339_f --extract snp_group1.txt --make-grm --out test_group1
  ./gcta64 --bfile SAM_339_f --extract snp_group2.txt --make-grm --out test_group2
  ./gcta64 --bfile SAM_339_f --extract snp_group3.txt --make-grm --out test_group3
  ./gcta64 --bfile SAM_339_f --extract snp_group4.txt --make-grm --out test_group4
  nano multi_GRMs.txt
  .／gcta64 --reml --mgrm multi_GRMs.txt --pheno branchingZone.txt --BZ_0
  ...

##Genetic correlation


