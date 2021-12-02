git
===

# fork/clone a project
NOTE: never use the [https://github.com/...] address for forks
 use [git@github.com:...] instead!

 if that somehow happens, the paths can be replaced in ".../[mainGitDir/.git/config]"


- fork project in github to your user
- copy ssh clone URL e.g. git@github.com:mpsonntag/gndata-editor.git
- terminal: move to folder you want to create your repository
- clone repository

      git clone [git ssh clone URL] [projectFolderName]
      # e.g.
      ~/work$ git clone git@github.com:mpsonntag/gndata-editor.git GNdata-editor

- set upstream repository!

      git remote add upstream [git ssh spoon-knife clone url]

# managing git
- check git settings in main git folder

      cat .git/config


# regarding commits
- always pull before a commit!
- commit always only affects the local repository
- when commit, use comment format: [filename/topic]: [short descr]
- when using vim:

      esc + i ... insert
      :x ... save and exit


# git commands

    git status                      // ... shows project status
    git add []                      // ... add files to git repository
    git add .                       // ... adds all files recursively to repository which are not managed by git yet
    git commit                      // ... commits all added or removed files to/from local git repository
    git remote                      // ... show all remote repositories
    git pull origin master          // ... pull from master branch
    git pull origin [whatever]      // ... pull from [whatever] branch
    git push origin master          // ... push changes from current branch to respective master branch on github
    git push origin master --set-upstream    // ... if a project is forked, the local master is the fork, then upstream pushes to the original master from where the project was forked
    git branch                      // ... show local branches of project
    git branch -a                   // ... show all local and distant branches of project
    git checkout [branchName]       // ... switch from current project branch to [branchName]
    git merge [branchName]          // ... merge locally current project branch with [branchName]
    git branch -D [branchName]      // ... delete branch [branchName]
    git remote -v                   // ... shows, which repositories are added


# forking and cloning a project
if we fork a project in github from a folder to our own user, we create a copy of the original project within a new "namespace". when this is cloned. this new project will be the master to which commits can be pushed. upstream repositorys can be defined, so that the original project can also be updated - but its better for other people to pull from a fork and merge that with their own project than update the main repository.

## how to specify the original repository to our fork (the "spoon-knife" repository)
- get clone url of spoon-knife repository from github
- move to the local git repository dir
- check which repositories are already used

      git remote -v

- add upstream repository by using

      git remote add upstream [git ssh spoon-knife clone url]

- now we have specified the upstream repository for our fork repository

## how to sync fork repository with upstream repository
- to get modifications done in the upstream repository

      git fetch upstream

- this creates a local branch of the upstream repository

      git remote -v

- next make sure you are in the master fork repository

      git checkout master

- then merge the upstream repository with the fork repository

      git merge upstream/master


# pull request
if everyone is working on their own forks, then a pull request lets other people know, that there are major changes in your fork and they should sync their forks with the contents of your fork.

## creating a pull request
In github under your user and your fork, look on the left hand side above the file list. there is a green button and a branch button. select the branch you want to compare and then use the green button, to see, if there are any changes between the spoon and the fork repositories of the selected branch. Through the following menu you can create a pull request for all other users.

## handling a pull requires
if there is an open pull request, it can be seen on the right hand side


# branches
branches are useful for working on stuff, while always keeping a master branch that contains the last working version. once the branch version is functional, the cahnges can be merged with the last functional version of the master branch.

- first create a new branch and immediately switch to this branch

      git checkout -b [name of new branch]

- next we push this branch to github

      git push origin [name of new branch]

- we can check all existing branches

      git branch

- show differences between branches

      git diff [branch1]..[branch2]


## merging branches
- checkout branch you want to import changes into

      git checkout [branch1]    // ... e.g.:  git checkout master

- then merge

      git merge [branch2]    // ... e.g.: git merge workinprogress


# push
- normal push moves always all associated branches to remote

      git push

- to push only a specific branch to the remote repository

      git push origin [branch name]


# Creating a github repository from local repository
- create a github repository thats named exactly as the folder you want to add on github
- move to folder containing files
- initialize git

      git init

- create file .gitignore and modify

      *_exc_*
      *~

- locally add all files to repository (will not add empty folders)

      git add .

- commit

      git commit -m 'hurra'

- add remote repository

      git remote add origin [repo ssh origin url]
      git remote -v

- push stuff to github

      git push origin master


# git rebase
http://git-scm.com/book/de/v1/Git-Branching-Rebasing



## G-Node git pipeline
- go to github
- go to your project
- open Milestone of interest
- go to issues (right hand side)
- create a new one, assign milestone and person and submit

      git fetch --all
      git rebase upstream/master

- In IDEA go to tools
- go to tasks & context, select configure server
- enter host github.com
-- repository G-Node / [repository name] e.g. gndata-editor
- either enter API token that already exists for a project or create new one
- on right hand side upper corner select default task/add task ... issues from github should be imported.
- if this is correct, IDEA creates a new branch for this issue
- make changes in project, add changed files in "changes"
- use "commit", write one line description, leave one empty line, write "Fixes #issuenumber" if this commit actually fixes the number

      git log                             // ... check last commits
      git push origin [branchname]        // ... push issue branch to repository e.g.:
                                                    git push origin gndata-editor-46

- someone has to check the changes on github and merge the branch with the master

      git status

- change to master

      git fetch --all
      git rebase upstream/master              // ... to get the changes included into the own master repository
      git log
      git push origin master
      git branch -D gndata-editor-46          // ... remove issue branch locally
      git push origin :gndata-editor-46       // ... remove issue branch in repository


## creating pull request
- github: create pullrequest (process self explanatory)
- if pull request cannot be merged

      git checkout master
      git fetch --all
      git rebase upstream/master
      git checkout [problem branch]
      git rebase master
      git status                              // ... check where the conflicts are, resolve them in IDE, save
      git add [modified files]                // ... will be added to last commit
      git status
      git rebase --continue
      git log
      git push origin [problem branch]        // ... if this does not work, use force:
      git push origin [problem branch] -f
  
      git commit --amend                      // add changes to last commit


## creating an intermediate branch and subsequent pull request with these changes only
- start

      git checkout -b [newbranch]    // ... create new branch

- do stuff

      git add .                       // ... add changes
      git commit                      // ... commit changes
      git push origin [newbranch]     // ... push changes to github

- create pull request in github 
-- pull request
-- edit
-- select branch to pull
-- add description
-- create pull request

      git checkout [workBranch]       // get back to actual workBranch
      git rebase [newBranch]          // get changes from [newBranch] to [workBranch]


# using submodules (git within git)
http://git-scm.com/book/en/v2/Git-Tools-Submodules

move to folder where the submodule is supposed to be installed

    git submodule add git@github.com:mpsonntag/testBoot.git stylesheets


# create github repository from local
https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/#platform-linux

- initialize local git repository

      git init

- create .gitignore file, open and add (or copy from another project):

      # IDEA
      .idea/
      *.iml
  
      # log files
      *.log
  
      # maven
      target/
      pom.xml.tag
      pom.xml.releaseBackup
      pom.xml.versionsBackup
      pom.xml.next
      release.properties

- add all files to git and commit

      git add .
      git commit -m "first commit"

- create new repository in github using the same name as the folder the project resides in; do not initialize the github repo
- copy the quick setup ssh adresse of the github repository e.g. git@github.com:mpsonntag/G-Node-Bootstrap.git
- add the github repository as the remote repository for the local git, e.g.:

      git remote add origin git@github.com:mpsonntag/G-Node-Bootstrap.git

- push changes from local to remote

      git push origin master

- have a cup of tea


# undo last commit:
http://stackoverflow.com/questions/927358/how-to-undo-the-last-commit


# Find difference between commits:

http://jk.gs/git-diff.html

- show the difference between the last two commits:

      git diff HEAD^ HEAD


# git delete
- delete local branch

      git branch -d the_local_branch

- delete remote branch

      git push origin :the_remote_branch


# git amend commit (add stuff, change message)
- add stuff to the last commit

      git commit --amend

- add stuff to the last commit w/o changing the commit message

      git commit --amend --no-edit

- change commit message of last commit

      git commit --amend -m "New commit message"


# transfer ownership from user to organization:

https://help.github.com/articles/transferring-a-repository/

- update github repository
- close IDE
- rename local project folder as backup
- transfer ownership
- fork from organisation to user
- move to main directory, clone user repository
- set upstream
- copy .iml file and .idea folder from backup folder to new folder
- open IDE, check if everything works
- delete backup folder

- if there were CI links, these have to be updated as well. (G-Node appvoyer repo runs via adrians user)

# create branch from previous commit:

e.g. to create a save branch when an specific release has been done.

    git branch branchname <sha1-commit-hash>

from: http://stackoverflow.com/questions/2816715/branch-from-a-previous-commit-using-git


# Remove stale upstream repositories

If working on with distributed repositories, it can happen, that stale remote repositories can be shown when using `git branch -a` or `git checkout.`

Remove stale remote repositories by:

    git remote prune [origin/upstream]

You can display which repositories will be removed by using first:

    git remote prune --dry-run [origin/upstream]
