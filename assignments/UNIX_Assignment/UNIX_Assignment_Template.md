#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`
```
here is my snippet of code used for data inspection
```
cat fang_et_al_genotypes.txt  and head fang_et_al_genotypes.txt-> to observe file, head -n 1 fang_et_al_genotypes.txt -> to generate the first line and tail -n 1 fang_et_al_genotypes.txt printed the last line of the file, in combination (head -n 1; tail -n 1) < fang_et_al_genotypes.txt gave the first and the last line of the file
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
head -n 3 snp_position.txt printed the header and first 2 rows 
tail -n 3 snp_position.txt printed the last 3 rows
(head -n 3; tail -n 3) < snp_position.txt printed the column heads, first 2 rows and last 3 rows
wc snp_position.txt printed the number of lines, words, and bytes or characters.
du -h snp_position.txt printed the file size.
awk -F "\t" '{print NF; exit}' snp_position.txt printed number of columns.
file snp_position.txt -> to check for ASCII characters.
sort -k1,1 -k2,2n -c snp_position.txt and echo $? -> to check if it was sorted

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
grep -E "(ZMMIL|ZMMLR|ZMMMR|Group)" fang_et_al_genotypes.txt > 3maize_files.txt
cut -f 3 3maize_files.txt | sort | uniq -c | head -n 10 
awk -f transpose.awk 3maize_files.txt > transposed_maize.txt
tail -n+4 transposed_maize.txt > maize_headless.txt
cut -f 1,2,3,4 maize_headless.txt | head -n 4
cut -f 1,3,4 snp_position.txt > cut_snp.txt


Here is my brief description of what this code does


###Teosinte Data

```
here is my snippet of code used for data processing
```
grep -E "(ZMPBA|ZMPIL|ZMPJA|Group)" fang_et_al_genotypes.txt > 3teosinte_files.txt
cut -f 3 3teosinte_files.txt | sort | uniq -c | head -n 10
awk -f transpose.awk 3teosinte_files.txt > transposed_teosinte.txt
cut -f 1,2,3,4 transposed_teosinte.txt | head -n 4
tail -n+4 transposed_teosinte.txt > teosinte_headless.txt
cut -f 1,2,3,4 teosinte_headless.txt | head -n 4
cut -f 1,3,4 snp_position.txt > cut_snp.txt
Here is my brief description of what this code does
