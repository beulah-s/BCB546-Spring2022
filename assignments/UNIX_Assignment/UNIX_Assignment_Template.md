#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`
```
here is my snippet of code used for data inspection
```

to observe file

```
head fang_et_al_genotypes.txt 
```
to print the first line

```
head -n 1 fang_et_al_genotypes.txt
```

to print the last line of the file

```
tail -n 1 fang_et_al_genotypes.txt
```
```
(head -n 1; tail -n 1) < fang_et_al_genotypes.txt 
```

scroll through the file contents and inspect the file

```
less fang_et_al_genotypes.txt
```

to know the number of lines, words, and bytes or characters

```
wc fang_et_al_genotypes.txt 
```

to know the file size

```
du -h fang_et_al_genotypes.txt
```

to know the number of columns

```
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt 
```

```
tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'
```
```
grep -v "^#" fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' 
```

to check if the file had ASCII characters
```
file fang_et_al_genotypes.txt
```

By inspecting this file I learned that:

1. The information is arranged in 986 columns with A/T/G/C bases
2. It has 2783 lines, 2744038 words and 11051939 characters
3. File size is 6.5M
4. Has ASCII text with very long lines

###Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
```

to view the header and first 2 rows 

```
head -n 3 snp_position.txt 
```

to view the last 3 rows

```
tail -n 3 snp_position.txt
```

to view header, first 2 rows and last 3 rows

```
(head -n 3; tail -n 3) < snp_position.txt  
```

to know the number of lines, words, and characters

```
wc snp_position.txt 
```

to know the file size

```
du -h snp_position.txt  
```

to know the number of columns

```
awk -F "\t" '{print NF; exit}' snp_position.txt 
```

to check for ASCII characters

```
file snp_position.txt 
```

to sort the file

```
sort -k1,1 -k2,2n -c snp_position.txt
```

to check if it was sorted

```
echo $? 
```

By inspecting this file I learned that:

1. The snp file has 15 columns
2. It has 984 lines, 13198 words, and 82763 bytes or characters
3. File size is 41 K
4. It has ASCII text
5. File is not sorted


##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
```

Separate the 3 maize groups to one file

```
grep -E "(ZMMIL|ZMMLR|ZMMMR|Group)" fang_et_al_genotypes.txt > 3maize_files.txt
cut -f 3 3maize_files.txt | sort | uniq -c | head -n 10 
```

transpose 3 maize_files.txt

```
awk -f transpose.awk 3maize_files.txt > transposed_maize.txt
```

remove the first 3 rows

```
tail -n+4 transposed_maize.txt > maize_headless.txt
cut -f 1,2,3,4 maize_headless.txt | head -n 4
```

to sort maize_headless.txt before joining

```
sort -k1,1 maize_headless.txt > sort_maize_headless.txt
head -n 2 sort_maize_headless.txt
```

separate columns for snp ID, Chromosome and positions to one file 

```
cut -f 1,3,4 snp_position.txt > cut_snp.txt
head -n 2 cut_snp.txt
```

to remove header in cut_snp.txt

```
tail -n+2 cut_snp.txt > cut_snp_headless.txt
head -n 2 cut_snp_headless.txt
```

to sort cut_snp_headless.txt

```
sort -k1,1 cut_snp_headless.txt > sort_cut_snp_headless.txt
```

to join sort_cut_snp_headless.txt and sort_maize_headless.txt

```
join -1 1 -2 1 -t $'\t' sort_cut_snp_headless.txt sort_maize_headless.txt > join_maize.txt
```


```
wc sort_cut_snp_headless.txt
```

The sorted file has 983 lines,  2943 words and 22282 characters 


```
wc sort_maize_headless.txt
```

The sorted maize file has 983 lines, 1547242 words and 6195860 characters 

``` 
wc sort_teosinte_headless.txt
```

The sortd teosinte file has 983 lines, 959408 words and 3844524 characters

sort based on increasing order of positions and missing data encoded by "?". scroll through the file and check first and last entries, "?" already present in file in place of missing data
 
```
grep -v "multiple" join_maize.txt | grep -v "unknown" | sort -k3,3n > increase_join_maize.txt
less increase_join_maize.txt
```
 
sort based on decreasing order of positions
 
```
grep -v "multiple" join_maize.txt | grep -v "unknown" | sort -k3,3nr > decrease_join_maize.txt
less decrease_join_maize.txt
```
 
replace missing data (i.e.,?) with "-" in decrease_join_maize.txt

```
sed 's/?/-/g' decrease_join_maize.txt > decrease_join_maize-.txt
less decrease_join_maize-.txt
```

 
multiple snp positions in one file
 
```
awk '$3 ~ /^multiple$/' join_maize.txt > maize_multiple.txt
```
  
unknown snp positions in one file
  
```
awk '$3 ~ /^unknown$/' join_maize.txt > maize_unknown.txt
```

Distribute data by chromosomes in increasing order of snp positions

```
awk '$2 ~ /^1$/' increase_join_maize.txt > chr1_inc_maize.txt
less chr1_inc_maize.txt
awk '$2 ~ /^2$/' increase_join_maize.txt > chr2_inc_maize.txt
```
did the same for all 10 chromosomes

Distribute data by chromosomes in decreasing order of snp positions

```
awk '$2 ~ /^1$/' decrease_join_maize-.txt > chr1_dec_maize.txt
less chr1_dec_maize.txt
awk '$2 ~ /^2$/' decrease_join_maize-.txt > chr2_dec_maize.txt
```
did the same for all 10 chromosomes


###Teosinte Data

```
here is my snippet of code used for data processing
```
To separate the 3 Teosinte groups to one file


```
grep -E "(ZMPBA|ZMPIL|ZMPJA|Group)" fang_et_al_genotypes.txt > 3teosinte_files.txt
cut -f 3 3teosinte_files.txt | sort | uniq -c | head -n 10
```

to transpose 3teosinte_file.txt


```
awk -f transpose.awk 3teosinte_files.txt > transposed_teosinte.txt
cut -f 1,2,3,4 transposed_teosinte.txt | head -n 4
```

to remove the first 3 rows

```
tail -n+4 transposed_teosinte.txt > teosinte_headless.txt
cut -f 1,2,3,4 teosinte_headless.txt | head -n 4
```

to sort teosinte_headless.txt

```
sort -k1,1 teosinte_headless.txt > sort_teosinte_headless.txt
head -n 1 sort_teosinte_headless.txt
```

joining snp and teosinte

```
join -1 1 -2 1 -t $'\t' sort_cut_snp_headless.txt sort_teosinte_headless.txt > join_teosinte.txt
cut -f 1,2,3,4,5 join_teosinte.txt |head -n 3
```

Sort based on increasing order of snp positions and missing data encoded by "?". scroll through the file and check first and last entries, "?" already present in file in place of missing data

```
grep -v "multiple" join_teosinte.txt | grep -v "unknown" | sort -k3,3n > increase_join_teosinte.txt
less increase_join_teosinte.txt
```

sort based on decreasing order of snp positions

```
grep -v "multiple" join_teosinte.txt | grep -v "unknown" | sort -k3,3nr > decrease_join_teosinte.txt
less decrease_join_teosinte.txt
```

replace missing data (i.e.,?) with "-" in decrease_join_maize.txt

```
sed 's/?/-/g' decrease_join_teosinte.txt > decrease_join_teosinte-.txt
less decrease_join_teosinte-.txt
```

multiple snp positions in one file

```
awk '$3 ~ /^multiple$/' join_teosinte.txt > teosinte_multiple.txt
```

unknown snp positions in one file

```
awk '$3 ~ /^unknown$/' join_teosinte.txt > teosinte_unknown.txt
```

Distribute data by chromosomes in increasing order of snp positions

```
awk '$2 ~ /^1$/' increase_join_teosinte.txt > chr1_inc_teosinte.txt
less chr1_inc_teosinte.txt
awk '$2 ~ /^2$/' increase_join_teosinte.txt > chr2_inc_teosinte.txt
```

did the same for all 10 chromosomes

Distribute data by chromosomes in decreasing order of snp positions

```
awk '$2 ~ /^1$/' decrease_join_teosinte-.txt > chr1_dec_teosinte.txt
less chr1_dec_teosinte.txt
awk '$2 ~ /^2$/' decrease_join_teosinte-.txt > chr2_dec_teosinte.txt
```

did the same for all 10 chromosomes
