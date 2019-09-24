# UNIX-Assignment_Sara_Hazinia
## Data Inspection
**Attributes of Fang_et_al_genotypes.txt**

```
head -n 5 fang_et_al_genotypes.txt
```

```
head -n 5 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'
``` 

```
tail -n 5 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'```
 

```wc -l fang_et_al_genotypes.txt```

```du -h fang_et_al_genotypes.txt```

By inspecting rhis file I learned that:
  
* 986 columns
* 2783 lines 
* File size: 6.1 M



**Attributes of snp_position.txt**

```head -n 5 snp_position.txt | awk -F "\t" '{print NF; exit}'```

```tail -n 5 snp_position.txt | awk -F "\t" '{print NF; exit}'```

```wc -l snp_position.txt```

```du -h snp_position.txt```


By inspeccting this file I learned: 

* 15 columns 
* 984 lines
* File size: 38K

## Data processing

```head -5 fang_et_al_genotypes.txt | cut -f 1-10 | column -t```

```head snp_position.txt | cut -f 1-10 | column -t```

To check the small portion of each file (genotypes and snps) to figure out what are the columns and rows in each file. The fang-et_al_genotype.txt file has snp information on columns and genotypes in rows, and the snp.txt file has snps on rows and extra information on columns. 


```awk -f transpose.awk fang_et_al_genotypes.txt > transposed_genotypes.txt```

```wc -l transposed_genotypes.txt```

Therefore, we need to transpose the fang_etal file to have all the snps on the rows so the two files will have similar type of information on rows and can be joined. Double check the transposed fang_et_al file which now has 2783 columns and 986 lines. 

```cut -f 1,3,4 snp_position.txt > snp_formatted_position.txt```

```sort -c -k1,1 snp_formatted_position.txt```

Join the files in a way that the first column is SNP_ID, second column is Chromosome, and third column is Position and the rest are genotype data from either maize or teosinte. 

```(head -n 1 snp_formatted_position.txt  && tail -n +2 snp_formatted_position.txt | sort -k1,1) > snp_formatted_sorted_position.txt```

```head snp_formatted_sorted_position.txt | column -t```

```tail -n +2 snp_formatted_sorted_position.txt | sort -c -k1,1```

Sorting without the header and checking the file. 

```head snp_formatted_sorted_position.txt | column -t```

And check the file I just created. 

```head fang_et_al_genotypes.txt | cut -f 1-5 | column -t```

To search and find the genotypes of interest for maize and teosinte.

```sort -k3 fang_et_al_genotypes.txt | cut -f 3 | uniq -c | column -t```

To look at column 3 of the file which contains the Group names (cut -f 3), first I sort it (sort -k3) then see how many genotypes per group do we have (uniq -c). 

**Maize data**

```awk -F "\t" '$3 ~ /ZMMIL|ZMMLR|ZMMMR|Group/' fang_et_al_genotypes.txt | cut -f 1,4-986> maize_genotypes.txt```

```head maize_genotypes.txt | cut -f 1-6 | column -t```

Used awk to extract the rows that have the group names I need for maize and transfer it into a new file. I use cut -f 1,4-986 because I do not need columns 2 and 3 in the file (I only need snp ID and data).

```awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt```

Transpose the file. 

```(head -n 1 transposed_maize_genotypes.txt && tail -n +2 transposed_maize_genotypes.txt | sort -k1,1) > sorted_transposed_maize_genotypes.txt```

```tail -n +2 sorted_transposed_maize_genotypes.txt | sort -c -k1,1```

Now I sorted the transposed file without sorting its header, and used && tail -n +2 command to double check there is no error.

```join --header -1 1 -2 1 -t $'\t' snp_formatted_sorted_position.txt sorted_transposed_maize_genotypes.txt > joined_maize_snp_genotypes.txt```

```head joined_maize_snp_genotypes.txt | cut -f 1-10 | column -t``` 

Now I join the maize file with the snp file and check to make sure it looks fine (head command).

```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 1' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom1_maize_snp_genotypes.txt```

```head Chrom1_maize_snp_genotypes.txt | cut -f 1-10 | column -t```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 2' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom2_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 3' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom3_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 4' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom4_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 5' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom5_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 6' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom6_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 7' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom7_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 8' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom8_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 9' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom9_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 10' joined_maize_snp_genotypes.txt | sort -k 3n) > Chrom10_maize_snp_genotypes.txt```

Creating 10 files for maize (one for each chromosome) with the second column to be containing chromosomes (awk '$2 == numberofchromosome') and SNPs ordered based on increasing value of position (sort -k 3n).

```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 1' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom1_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 2' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom2_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 3' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom3_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 4' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom4_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 5' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom5_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 6' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom6_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 7' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom7_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 8' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom8_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 9' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom9_reversed_maize_snp_genotypes.txt```
```(head -n 1 joined_maize_snp_genotypes.txt && awk '$2 == 10' joined_maize_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom10_reversed_maize_snp_genotypes.txt```

Creating 10 files for maize with the reverse order of position column (sort -k 3nr) with all the question marks replaced by -/- (sed 's/?/-/g')

```(head -n 1 joined_maize_snp_genotypes.txt && awk '$3 ~ /unknown/' joined_maize_snp_genotypes.txt) > Unknown_position_maize_snp.txt```

```head Unknown_position_maize_snp.txt | cut -f 1-10 | column -t```

To chose all SNPs with unknown position and transfer them in a new file.

```(head -n 1 joined_maize_snp_genotypes.txt && awk '$3 ~ /multiple/' joined_maize_snp_genotypes.txt) > Multiple_position_maize_snp.txt```

```head Multiple_position_maize_snp.txt | cut -f 1-10 | column -t```

To chose all SNPs with multiple position and transfer them in a new file.


**Teosinte data**

```awk -F "\t" '$3 ~ /ZMPBA|ZMPIL|ZMPJA|Group/' fang_et_al_genotypes.txt | cut -f 1,4-986> teosinte_genotypes.txt```

```head teosinte_genotypes.txt | cut -f 1-6 | column -t```

Used awk to extract the rows that have the group names I need for teosinte and transfer them into a new file. I use cut -f 1,4-986 because I do not need columns 2 and 3 in the file (I only need snp ID and data).

```awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt```

Transpose the file. 

```(head -n 1 transposed_teosinte_genotypes.txt && tail -n +2 transposed_teosinte_genotypes.txt | sort -k1,1) > sorted_transposed_teosinte_genotypes.txt```

```tail -n +2 sorted_transposed_teosinte_genotypes.txt | sort -c -k1,1```

Now I sorted the transposed file without sorting its header, and used && tail -n +2 command to double check there is no error.  

```join --header -1 1 -2 1 -t $'\t' snp_formatted_sorted_position.txt sorted_transposed_teosinte_genotypes.txt > joined_teosinte_snp_genotypes.txt```

```head joined_teosinte_snp_genotypes.txt | cut -f 1-10 | column -t```

Now I join the teosinte file with the snp file and check to make sure it looks fine (head command). 

```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 1' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom1_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 2' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom2_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 3' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom3_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 4' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom4_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 5' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom5_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 6' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom6_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 7' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom7_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 8' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom8_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 9' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom9_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 10' joined_teosinte_snp_genotypes.txt | sort -k 3n) > Chrom10_teosinte_snp_genotypes.txt```


Creating 10 files for teosinte (one for each chromosome) with the second column to be containing chromosomes (awk '$2 == numberofchromosome') and SNPs ordered based on increasing value of position (sort -k 3n).

```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 1' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom1_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 2' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom2_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 3' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom3_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 4' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom4_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 5' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom5_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 6' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom6_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 7' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom7_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 8' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom8_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 9' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom9_reversed_teosinte_snp_genotypes.txt```
```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$2 == 10' joined_teosinte_snp_genotypes.txt | sort -k 3nr | sed 's/?/-/g') > Chrom10_reversed_teosinte_snp_genotypes.txt```

Creating 10 files for teosinte with the reverse order of position column (sort -k 3nr) with all the question marks replaced by -/- (sed 's/?/-/g')

```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$3 ~ /unknown/' joined_teosinte_snp_genotypes.txt) > Unknown_position_teosinte_snp.txt```

```head Unknown_position_teosinte_snp.txt | cut -f 1-10 | column -t```

To chose all SNPs with unknown position and transfer them in a new file.

```(head -n 1 joined_teosinte_snp_genotypes.txt && awk '$3 ~ /multiple/' joined_teosinte_snp_genotypes.txt) > Multiple_position_teosinte_snp.txt```

```head Multiple_position_teosinte_snp.txt | cut -f 1-10 | column -t```

To chose all SNPs with multiple position and transfer them in a new file.
