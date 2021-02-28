# UNIX Assignment

## Data Inspection


### Attributes of 'fang_et_al_genotypes.txt'

#### Here is a sample of code I used for inspection

head -n 15 fang_et_al_genotypes.txt
wc -l fang_et_al_genotypes.txt
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
file fang-et_al_genotypes.txt
du -h fang_et_al_genotypes.txt

#### By inspecting this file I learned:

1) 1 line of header
2) 1109 lines
3) 0 columns
4) 96K


#### Attributes of 'snp_position.txt'

##### Here is a sample of code I used for inspection

head -n 15 snp_position.txt
wc -l snp_position.txt
awk -F "\t" '{print NF; exit}' snp_position.txt
file snp_position.txt
du -h snp_position.txt

##### By inspecting this file I learned:

1) 3 lines header
2) 5073 lines
3) 0 columns
4) 33K



## Data Processing

### Maize Data

grep -E "(Sample_ID|ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
#### get SNP ID and position


awk -f transpose.awk maize_genotypes.txt > transposed_maize-genotypes.txt
#### using your transpose.awk file I transposed genotype columns into rows


tail -n +2 snp_position.txt | sort -k1,1 > snp_position_sorted.txt
#### sort without first line


tail -n +4 transposed_maize_genotypes.txt | sort -k1,1 > maize_genotypes_sorted.txt
#### sort without first 3 lines


join -1 1 -2 1 -t $'\t' snp_position_sorted.txt maize_genotypes_sorted.txt > maize_genotypes_combined.txt
#### combine the files


mkdir -p Maize_SNPs/Increasing_SNPs
#### make directory for increasing maize SNPs


mkdir -p Maize_SNPs/Decreasing_SNPs
#### same but for decreasing SNPs


awk -F "\t" $3 ~ /1/ maize_genotypes_combined.txt | sort -t $'\t' -k4,4n > Maize_SNPs/Increasing_SNPs/maize_chr1.txt
#### sort maize SNPs in increasing order and store it in its own directory
#### this was done for each 10 chromosomes


awk -F "\t" '$3 ~ /1/' maize_genotypes_combined.txt | sort -t $ '\t' -k4,4rn | sed 's/?/-/g' > Maize_SNPs/Decreasing_SNPs.maize_chr1.txt
#### sort maize SNPs in decreasing order and store in a diretory
#### done for each chromosome


awk -F "\t" '$3 ~ /unknown/' maize_genotypes_combined > Maize_SNPs/maize_unknown.txt
#### gather unknown SNPs


awk -F "\t" '$3 ~ /multiple/' maize_genotypes_combined.txt > Maize_SNPs/maize_multiple.txt
#### gather positions with mulitple SNPs



### Teosinte Data

#### Almost the same but with ZMPBA, ZMPIL, and ZMPJA and different file names
