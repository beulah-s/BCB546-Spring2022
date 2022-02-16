#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`
```
here is my snippet of code used for data inspection
```
cat fang_et_al_genotypes.txt generated a long list of sequences
head fang_et_al_genotypes didn't help much as well
head -n 1 fang_et_al_genotypes printed a smaller output
tail -n 1 fang_et_al_genotypes printed the last line of the file
(head -n 1; tail -n 1) < fang_et_al_genotypes.txt gave the first and the last line of the file
less fang_et_al_genotypes.txt allowed me to scroll through the file contents and inspect it. I used 'q' to exit
wc fang_et_al_genotypes.txt printed the number of lines = 2783, words = 2744038, and bytes or characters =  11051939
du -h fang_et_al_genotypes.txt printed the file size as 6.5 M
awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt printed the number of columns as 986
tail -n +6 fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' and  grep -v "^#" fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}' gave the same result. No lines starting with #
hexdump
By inspecting this file I learned that:

1. This file contains the sample information arranged in 986 columns with A/T/G/C characters
2. It has 2783 lines, 2744038 words and 11051939 characters
3. File size is 6.5M
4. Has very long lines of ASCII characters

###Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
```
head -n 3 snp_position.txt printed the header and first 2 rows 
tail -n 3 snp_position.txt printed the last 3 rows
(head -n 3; tail -n 3) < snp_position.txt printed the header, first 2 rows and last 3 rows

By inspecting this file I learned that:

1. point 1
2. point 2
3. point 3

or

* point 1
* point 2
* point 3

##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does


###Teosinte Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does
