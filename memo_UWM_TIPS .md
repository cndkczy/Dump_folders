
## SNP filtering

This was processed via plink. SNPs from hapmap3 were downloaded from Panzea (https://www.panzea.org/) and filtered for the 285 genotypes from Wisconsin Diversity Panel.

The hapmap3 file was then filtered with max missing genotyping less than or equal to 0.6 and minimum MAF=0.02.

This resulted in 28,203,722 SNPs.Then we filtered the SNPs based on the LD using:

    ./plink --file uwm285_all.numeric_transformed --missing-genotype N --indep 50 10 2
  
And this result in 6,974,427 SNPs remain.
