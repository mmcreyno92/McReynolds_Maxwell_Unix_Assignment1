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

