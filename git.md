

Reference 

https://docs.github.com/fr/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax



# Create new branch 

```
git checkout -b my_new_branch8
git push --set-upstream origin my_new_branch
```


# To delete all local branch 
The below command will delete all the local branches except master branch.

```
git branch | grep -v "master"| xargs git branch -D
```
The above command:  
	1. list all the branches  
	2. From the list ignore the master branch and take the rest of the branches   
    3. delete the branch

# To delete all branch under specific word

Example command to delete all branch start with "fix/*"  
```
git branch --list 'fix/*' | xargs git branch -D
```
Breakdown of the command:
 - git branch --list 'fix/*': Finds all local branches starting with "fix/".
 - | (Pipe): Takes the output of the first command and hands it to the next.
 - xargs git branch -D: Takes each branch name found and executes the delete command against it.



# Misc 
```
How to add existing folder to git
Create an empty Git repository or reinitialize an existing one
git init
Add file contents to the index
git add .
Commit changes to the repository
git commit -m "initial commit"
Add a remote
git remote add origin <remote-repo-url>
Show URLs of remote repositories
git remote -v
Push the changes to remote repo
git push -f origin master
```
