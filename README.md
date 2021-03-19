# Clustering-Algorithms-Tutorial

The following PLINK command can convert your VCF to `raw`, which has rows for samples and columns for SNPs, just like the data you showed.

If the factors are 0/1 only, you have the dominance effect. If they are 0/1/2, it is additive (most common). It's not clear to me what you mean by saying a 0 represents no SNP at a position, because the VCFs I'm used to working with only have a row for a position if there is a SNP there... Do you mean 0 represents both alleles are the reference allele and there is no alternative allele at the position? This would be the dominance effect.

If it is additive:

plink --vcf my_vcf_path.vcf --recode A --out my_output_path

If dominant:

plink --vcf my_vcf_path.vcf --recode D --out my_output_path

This will produce the file my_output_path.raw

I suggest using `fread` from the `data.table` package to quickly read this file into R, but if it is small enough you can probably get away with base R functions like `read.csv`. If it is very massive (millions of SNPs), you'll need to read `read.big.matrix` from the `bigmemory` package.

Hopefully this will work :

my_data_in_R <- fread("my_output_path.raw")

The raw file has 6 extra columns at the front that you can get rid of with:

my_data_in_R <- my_data_in_R[, 7:ncol(my_data_in_R)]

I think it would be easiest to do the conversion on a whole-genome VCF, load the whole thing into R and then divide as needed after loading into R.

Reference for PLINK raw file format: https://www.cog-genomics.org/plink/1.9/formats#raw

Please let me know if anything is unclear or too slow... The PLINK conversion is not very fast but I believe PLINK is still the fastest tool for this purpose.

Michael Nagle
