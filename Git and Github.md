# Git and Github

` `  
` `  
## Committing files   
` `  
` `  
```
git add
```

Adds a change in the working directory to the staging area. It basically tells Git to include updates to a particular file in the next commit: it marks the updated files you would like to commit. Changes are not actually recorded until you run git commit.   
` `  
` `  
` `   
```
git add -i
```
Select specific files by number through the command index
   

- select range: ```1-22```
- select all: ```1-```    

if you wish to add all files, use ```add 'foldername'``` instead  
` `  
` `  
` `  

```
git commit
```

A commit is the Git equivalent of a "save".  
After git commit your local repository has a new version with all staged files
Git commit will open up the locally configured text editor and prompt for a commit message to be entered.  
` `  
` `  
```
git commit -m "commit message"
```
Passes a commit message immediately with the commit.  
` `  
` `  
` ` 

```
git push
```
sends a commit to Github  
` `  
` `  
` `  
```
git commit --amend
```
Allows you to modify the last commit
- change it's name
- add updated files to the commit
- delete files from the commit

You can't push a changed commit, it will give an error message because of the changes. Therefor, use ```git push -f```   
` `  
` `  
` `  
## .gitignore

A gitignore file specifies intentionally untracked files that Git should ignore.

1. open repository folder in VSCode
2. create new file
3. save as .gitignore
4. add files you wish to ignore
5. commit .gitignore file 

[More on .gitignore](https://www.atlassian.com/git/tutorials/saving-changes/gitignore)   
` `  
` `  
` `  
## Creating a new repo on the command line
` `  
` `  
``` 
echo "# my-new-repo" >> READ.md
```
Optional: creates new empy file from command line to commit  
` `    
` `   
```
git init
```
in folder where assignments or project files are located  
` `  
` `  
```
git add README.md
```
Optional: you can also create a repo and add/commit files to it later on.  
` `  
` `  
```
git commit -m "first commit"
```
` `  
` `  
```
git remote add origin https://github.com/your_username/your_new_repository_name.git
```
Links Github repository to your local repo.  
**remote**: Local computer repo  
**origin**: Github  
` `  
` `  
```
git push -u origin master
```
Sets upstream branch, only needs to be done once







