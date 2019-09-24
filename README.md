# UNIX-Assignment_SaraHazinia

## Data Inspection
**Attributes of Fang_et_al_genotypes.txt**

```head -n 5 fang_et_al_genotypes.txt```
```head -n 5 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'``` 

```tail -n 5 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'```
 

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
