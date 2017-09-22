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
