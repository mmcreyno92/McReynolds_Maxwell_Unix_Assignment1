# McReynolds_Maxwell_Unix_Assignment1
## Creating a Git Repository and Copying Files
In order to start the assignment I needed to create a Unix Assignment 1 Git Hub repository
I did this by logging on to my account on GitHub, chose the create repository option and then used the command-line based approach to initialize the depository and add this README.md file.
Once I had the repository named I followed the following GitHub instructions to create the repository:
```
echo "# McReynoldsUnixAssignment" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:mmcreyno92/McReynoldsUnixAssignment.git
git push -u origin master
```
With my repository set up I then went to the BCB546X-Fall2017 directory and did the following:
```
git pull origin master
```
to update my directory
Next I copied the files from the class repository into my own using the following: 
```
cd UNIX_Assignment/
ls
cp * ../../McReynolds_Maxwell_Unix_Assignment1/.
```
and did a git add and git commit to push the changes to my repository

## Analyzing the Data 

### File
I used the `file` command to look at both `fang_et_al_genotypes.txt` and `snp_position.txt`
```
$ file fang_et_al_genotypes.txt 
fang_et_al_genotypes.txt: ASCII text, with very long lines
```
```
$ file snp_position.txt 
snp_position.txt: ASCII text
```

### Head
I used the `head` command to inspect the data sets `fang_et_al_genotypes.txt` and `snp_position.txt` 
`fang_et_al_genotypes.txt` showed a large wall of text even though it was just a head command showing the first 10 lines
`snp_positions.txt` was much more manageable but still a good amount of text
I counted 15 columns for `snp_positions.txt` and an uncountable (confusing) amount for `fang_et_al_genotypes.txt` I will try a different command for it.  I also tried limiting the amount of lines by using 
```
head -n 1 fang_et_al_genotypes
```
The amount of information on the shell window was still too much to count easily

### Tail
I used the `tail` command out of curiosity on the data sets.  It offered no real help in surveying the data sets.  Still didn't hurt to try it...

### Head and Tail 
Used both commands simultaneosly to check the head and tail of the data sets using 
```
(head -n 2; tail -n 2) < fang_et_al_genotypes.txt
```
Not much information gained.

### Less
Since the files were so large I inspected them using the `less` command and navigated using `space bar, b, j, k, g, and q keys`

### WC
I used the `wc` command and found the number of lines, words, and bytes for each file.  
`fang_et_al_genotypes.txt` had 2,783 lines, 2,744,028 words, and 11,051,939 characters
`snp_position.txt` had 984 lines, 13,198 words, and 82,763 characters

### File Size
Inspecting the file size of a file is crucial before downloading or analyzing anything
I used the `ls -lh` and `du -h` (human readable) commands to get the file sizes
`fang_et_al_genotypes.txt` had a file size of 11M
`snp_positions.txt` had a file size of 84K
I agree with these sizes after using the `less` command to inspect the sizes of them earlier

### `Awk` for Column Number
I used my laptop to count the columns using the `awk` one liner we learned in class `awk -F "\t" '{print NF; exit}'`
`fang_et_al_genotypes` had 986 columns
`snp_position.txt` had 15 columns
To make sure that none of the columns didn't have a header I also employed the `grep -v "^#" <FILE NAME> | awk -F "\t" '{print NF; exit}'` from class on both files and received the same results


## Data Manipulation
The goal of this project is to create files for maize and teosinte genotyping by combining the `fang_et_al_genotypes.txt` file and the `snp_positions.txt` file and outputting new files for each chromosome (1-10) by increasing or decreasing SNP position values.  Any missing information should include either a `?` or `-` as determined by the assignment details.  Also a file with all SNPs with unknown positions in the genome as well as a file with SNPs with multiple positions in the genome should be created.  This will total to approximately 44 files.

### Transpose Data
First I transposed the genotype data so the columns became rows.  I used the code provided in the assignment to do this:
```
awk -f transpose.awk fang_et_al_genotypes.txt > transposed_genotypes.txt
```

### Extracting Maize and Teosinte Data
Next I extracted the data for maize and teosinte from the `fang_et_al_genotypes.txt` file as well as the headers so I knew which column contained which data using an extended grep command as shown below
```
grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt
grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt
```
This created files with the extracted data

### Transposing the Maize and Teosinte Data
The data extracted for maize and teosinte also needed transposed much like the `fang_et_al_genotypes.txt` file so I followed the command listed in the assignment tips section
```
awk -f transpose.awk teosinte_genotypes.txt > transposed_teosinte_genotypes.txt

awk -f transpose.awk maize_genotypes.txt > transposed_maize_genotypes.txt
```
I can now match the data from these files with the `snp_positions.txt` file

### Removing Headers from Maize, Teosinte, and SNP Positions Files
In order to sort the data I removed the headers from the above-mentioned files.  With headers present it wouldn't allow me to later join the data sets together
```
tail -n +4 transposed_maize_genotypes.txt > headerless_transposed_maize_genotypes.txt
tail -n +4 transposed_teosinte_genotypes.txt > headerless_transposed_teosinte_genotypes.txt
```

### Sorting and Joining Files
With the headerless files now created I sorted them on column one for each of the data sets.  This was needed to join the files which shared data in column one.  I performed the following codes:
```
sort -k1,1 headerless_transposed_teosinte_genotypes.txt > sorted_headerless_transposed_teosinte_genotypes.txt
sort -k1,1 headerless_transposed_maize_genotypes.txt > sorted_headerless_transposed_maize_genotypes.txt
```
for maize, teosinte, and the snp_position files and then joined the maize file with the snp file and the teosinte file with the snp file using the sorted column 1
```
join -1 1 -2 1 sorted_headerless_snp_position.txt sorted_headerless_transposed_maize_genotypes.txt > joined_maize.txt
join -1 1 -2 1 sorted_headerless_snp_position.txt sorted_headerless_transposed_teosinte_genotypes.txt > joined_teosinte.txt
```
This created two joined files with teosinte and snp_position being one and maize and snp_position being the other

### Cutting Excess Columns
For each of the maize and teosinte joined files I wanted to remove all of the other unecessary columns just to leave the SNP id, chromosome number, snp position and the genotyping information.  To do this I did a `head` command to see the column titles so I knew which columns I wanted to keep and which to get rid of for my joined maize and teosinte files.  I then used
```
$ cut -d" " -f1,3,4,14-1586 joined_maize.txt > subset_cut_joined_maize.txt
$ cut -d" " -f1,3,4,14-988 joined_teosinte.txt > subset_cut_joined_teosinte.txt
``` 
to remove those uneccessary columns but keep the SNP ID, Chromosome number, and SNP position columns as well as the remaining genotype information.  

### Sorting, Removal of Unknown/Multiple, and Exporting Chromosome Specific Files
With the files now having the correct columns with all the chromosomes present I wanted to sort and create files for the individual chromosomes.  At the same time I wished to remove any of the unknown and multiple data for each file. To do this I used a for loop as shown below and paired each with a specific sorting method to sort the SNP position either increasing or decreasing and switched out the ? for a - symbol
```
$ for Chrom in {1..10}; do awk -v i=$Chrom '$2=i' subset_cut_joined_maize.txt | awk '($3 !~ /[{a-z]/)' | sort -k3,3 -r | sed 's/?/-/g' > decreasing_maize_Chrom$Chrom.txt; done
$ for Chrom in {1..10}; do awk -v i=$Chrom '$2=i' subset_cut_joined_teosinte.txt | awk '($3 !~ /[{a-z]/)' | sort -k3,3 -r | sed 's/?/-/g' > decreasing_teosinte_Chrom$Chrom.txt; done
$ for Chrom in {1..10}; do awk -v i=$Chrom '$2=i' subset_cut_joined_maize.txt | awk '($3 !~ /[{a-z]/)' | sort -k3,3  > increasing_maize_Chrom$Chrom.txt; done
$ for Chrom in {1..10}; do awk -v i=$Chrom '$2=i' subset_cut_joined_teosinte.txt | awk '($3 !~ /[{a-z]/)' | sort -k3,3  > increasing_teosinte_Chrom$Chrom.txt; done
```
The difference between the commands is the first two sort by decreasing (with - for missing genotype information) for maize and teosinte, respectively... While the last two sort by increasing (with ? for missing genotype information) for maize and teosinte, respectively. 

### Creation of Teosinte and Maize Unknown/Multiple Position Files
Having completed the first 40 files for maize and teosinte known/single positions I then created 4 more files for maize and teosinte having unknown and multiple positions in the genome.  To create these I used the awk command to look for the words unknown and multiple in column 3 within my pre-"separated by chromosome" files and move them to the newly specified files listed in the code
```
$ awk '$3 ~ /unknown/' subset_cut_joined_maize.txt > unknown_maize_genotypes.txt
$ awk '$3 ~ /unknown/' subset_cut_joined_teosinte.txt > unknown_teosinte_genotypes.txt
$ awk '$3 ~ /multiple/' subset_cut_joined_teosinte.txt > multiple_teosinte_genotypes.txt
$ awk '$3 ~ /multiple/' subset_cut_joined_maize.txt > multiple_maize_genotypes.txt
```

#Final Remarks 
This assignment was very beneficial to me to heighten my critical thinking skills when it comes to data inspection and manipulation. 
The finished files are as follows:
```
maize:
increasing_maize_Chrom1.txt  
increasing_maize_Chrom2.txt   
increasing_maize_Chrom3.txt   
increasing_maize_Chrom4.txt   
increasing_maize_Chrom5.txt   
increasing_maize_Chrom6.txt   
increasing_maize_Chrom7.txt   
increasing_maize_Chrom8.txt   
increasing_maize_Chrom9.txt   
increasing_maize_Chrom10.txt   
decreasing_maize_Chrom1.txt 
decreasing_maize_Chrom2.txt 
decreasing_maize_Chrom3.txt 
decreasing_maize_Chrom4.txt 
decreasing_maize_Chrom5.txt 
decreasing_maize_Chrom6.txt 
decreasing_maize_Chrom7.txt 
decreasing_maize_Chrom8.txt 
decreasing_maize_Chrom9.txt 
decreasing_maize_Chrom10.txt 
unknown_maize_genotypes.txt
multiple_maize_genotypes.txt 
```
```
teosinte
increasing_teosinte_Chrom1.txt
increasing_teosinte_Chrom2.txt
increasing_teosinte_Chrom3.txt
increasing_teosinte_Chrom4.txt
increasing_teosinte_Chrom5.txt
increasing_teosinte_Chrom6.txt
increasing_teosinte_Chrom7.txt
increasing_teosinte_Chrom8.txt
increasing_teosinte_Chrom9.txt
increasing_teosinte_Chrom10.txt
decreasing_teosinte_Chrom1.txt
decreasing_teosinte_Chrom2.txt
decreasing_teosinte_Chrom3.txt
decreasing_teosinte_Chrom4.txt
decreasing_teosinte_Chrom5.txt
decreasing_teosinte_Chrom6.txt
decreasing_teosinte_Chrom7.txt
decreasing_teosinte_Chrom8.txt
decreasing_teosinte_Chrom9.txt
decreasing_teosinte_Chrom10.txt
unknown_teosinte_genotypes.txt
multiple_teosinte_genotypes.txt 
```
