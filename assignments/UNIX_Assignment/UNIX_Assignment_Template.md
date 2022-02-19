#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`
```
here is my snippet of code used for data inspection
```
cat fang_et_al_genotypes.txt  and head fang_et_al_genotypes.txt-> to observe file


head -n 1 fang_et_al_genotypes.txt -> to generate the first line and tail -n 1 fang_et_al_genotypes.txt printed the last line of the file, in combination (head -n 1; tail -n 1) < fang_et_al_genotypes.txt gave the first and the last line of the file



less fang_et_al_genotypes.txt -> allowed me to scroll through the file contents and inspect it.


wc fang_et_al_genotypes.txt -> printed the number of lines, words, and bytes or characters. 


du -h fang_et_al_genotypes.txt -> printed the file size. 


awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt -> printed the number of columns.


tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' and  grep -v "^#" fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' gave the same result for number of columns.


file fang_et_al_genotypes.txt -> to check if the file had ASCII characters

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
```

check if it worked

```
cut -f 3 3maize_files.txt | sort | uniq -c | head -n 10 
```

transpose 3 maize_files.txt

```
awk -f transpose.awk 3maize_files.txt > transposed_maize.txt
```

check if it worked

```

```

remove the first 3 rows

```
tail -n+4 transposed_maize.txt > maize_headless.txt
```

check if the first 3 rows are removed

```
cut -f 1,2,3,4 maize_headless.txt | head -n 4
```

to sort maize_headless.txt before joining

```
sort -k1,1 maize_headless.txt > sort_maize_headless.txt
```

check if it was sorted

```
head -n 2 sort_maize_headless.txt
```

remove the first 3 rows in the snp_positions.txt file

```
cut -f 1,3,4 snp_position.txt > cut_snp.txt
```

view first 2 rows

```
head -n 2 cut_snp.txt
```

to remove header in cut_snp.txt

```
tail -n+2 cut_snp.txt > cut_snp_headless.txt
```
check file

```
head -n 2 cut_snp_headless.txt
```

to sort cut_snp_headless.txt

```
sort -k1,1 cut_snp_headless.txt > sort_cut_snp_headless.txt
```

to join sort_maize_headless.txt and sort_cut_snp_headless.txt

```
join -1 1 -2 1 -t $'\t' sort_cut_snp_headless.txt sort_maize_headless.txt > join_maize.txt
```

Here is my brief description of what this code does

 wc sort_cut_snp_headless.txt
 983  2943 22282 sort_cut_snp_headless.txt

 wc sort_maize_headless.txt
 983 1547242 6195860 sort_maize_headless.txt
 
 wc sort_teosinte_headless.txt
 983  959408 3844524 sort_teosinte_headless.txt
 
 
 sort based on increasing order of positions
 
 ```
 grep -v "multiple" join_maize.txt | grep -v "unkown" | sort -k3,3n > increase_join_maize.txt
 ```
 check first and last entries 
 
 ```
 less increase_join_maize.txt
 ```
 
###Teosinte Data

```
here is my snippet of code used for data processing
```
Separate the 3 Teosinte groups to one file

```
grep -E "(ZMPBA|ZMPIL|ZMPJA|Group)" fang_et_al_genotypes.txt > 3teosinte_files.txt
```
check if it worked

```
cut -f 3 3teosinte_files.txt | sort | uniq -c | head -n 10
```

transpose 3teosinte_file.txt

```
awk -f transpose.awk 3teosinte_files.txt > transposed_teosinte.txt
```

check if it worked

```
cut -f 1,2,3,4 transposed_teosinte.txt | head -n 4
```

remove the first 3 rows

```
tail -n+4 transposed_teosinte.txt > teosinte_headless.txt
```

check if it worked

```
cut -f 1,2,3,4 teosinte_headless.txt | head -n 4
```

to sort teosinte_headless.txt

```
sort -k1,1 teosinte_headless.txt > sort_teosinte_headless.txt
```

check if it was sorted

```
head -n 1 sort_teosinte_headless.txt
```

joining snp and teosinte

```
join -1 1 -2 1 -t $'\t' sort_cut_snp_headless.txt sort_teosinte_headless.txt > join_teosinte.txt
```

```
cut -f 1,2,3,4,5 join_teosinte.txt |head -n 3
```

Here is my brief description of what this code does
