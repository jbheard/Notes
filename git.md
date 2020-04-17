# How to Use Git

[git](https://git-scm.com/) is a version control system used for keeping track of files (mostly code). It lets us make "save points" and go back to them later if we need to. It is mostly used in conjunction with git servers like GitHub, that allow collaboration between people working on the same project.

## Git Basics

All of these commands assume you are in a terminal window (or command prompt/powershell for Windows) and in the root directory of your git repository.

### status

[git status](https://git-scm.com/docs/git-status) tells you the current state of the files in your repo.  
This information includes:  
* What files have been changed
* What files have been added/removed from the currently staged commit
* What files are in the directory, but git is not keeping track of (until you add them)
* What branch you are on
* 


### add

[git add](https://git-scm.com/docs/git-add) adds a file (or multiple) to the current commit.

For example, say you modify some files `main.py` and `functions.py`  
You can do `git add main.py functions.py` to track both of those changes. Now they are ready to be [committed][#commit]

### rm

[git rm](https://git-scm.com/docs/git-rm) removes a file from the next commit. This is useful for changing the names of things (`git rm <old-name`, `git add <new-name>`), and removing files that are no longer needed. 

### reset

[git reset](https://git-scm.com/docs/git-reset) resets the current changes. So if you have removed some files, added some, etc. And decided against those changes, you could do `git reset`. Your files would still have all the changes, but git would reset its own set of things to include in your next commit.

### commit

[git commit](https://git-scm.com/docs/git-commit) saves the current changes. This is like saving a backup copy of the files at this point in time. 

The most common usage is `git commit -m "message about the changes made"`  
If you do `git commit` without the `-m` flag, git will open a text editor for you to enter your message.

### push

[git push](https://git-scm.com/docs/git-push) *pushes* (uploads) all of your new commits to your remote repository. This could be GitHub, BitBucket, or some server you set up yourself.

### pull

[git pull](https://git-scm.com/docs/git-pull) *pulls* (downloads) all commits from the remote repository that you don't have on your local machine.


### clone

[git clone](https://git-scm.com/docs/git-clone) copies a repository from a some external source (e.g. a remote repository) into a directory.


### init

[git init](https://git-scm.com/docs/git-init) tells git you want the current directory to be a git repository. git will create the files it needs in `.git`.


## Branches

TODO: Write this later

## Pull Requests and Forks

TODO: Write this later

## More Advanced Usage

TODO: Write this later


## Examples

### Creating a new repo

This is taken from github, when a new repository is created:

```
1. git init
2. git add README.md
3. git commit -m "first commit"
4. git remote add origin https://github.com/<your-github-username>/<repo-name>.git
5. git push -u origin master
```

1. Tell git that the current folder on your local machine should be a git repo. git will create all of the files and stuff it needs in the `.git` folder.
2. Add a file called `README.md`, it is now ready to be committed. **Note:** this assumes the file already exists
3. Commit the file. This commit now exists on your local machine.
4. this tells git that you want to add that URL as a remote source for this repo, and name it `origin`
5. Upload the commit from (3) to the `origin`, from (4), on branch "master"

### TODO: Add more examples
