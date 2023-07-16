Sets up git inside the folder
====================
    git init 

What changes have been made since previous version
==============
    git status

To pick which changes we want in our next version
=============
    git add {filename|foldername|.=>currentFolder}

To create a new version
===========================
    git commit -m "message"

To config username and email
================================
    git config --global user.email "deneme@deneme.com"
    git config --global user.name "deneme@deneme.com"

To get our version history
============================
    git log

Amends will add changes to the previous commit
============================
    git commit -m "message" --amend

--no-edit means I don't want to change the last commit's message
    
    git commit --amend --no-edit

--amend will change the last commit's number so you should not amend public commits.

**Working area:** not added changes  
**Staging area:** where git puts changes

A file can be in both at the same time.

To take a file-folder out of the staging area
================================
    git reset {filename|foldername|.=>currentFolder}

To select the branch to work on
================

    git checkout {branchname}
You can switch to a non-existing branch (and create it) with:

    git checkout -b {new_branch_name}

You can restore working tree files with git checkout
===================
    git checkout -- .
    git checkout "commit hash" => o commite geri çeviriliyor
    git log => mevcut seçili commite kadar listeliyor
    git log --all => tüm commitleri listeliyor

HEAD => Which version we are currently viewing
HEAD normally points to the most recent commit of a branch.
You can make head point to a specific commit.

Eski bir commite dönüp (checkoutlayıp) bunun üzerinde değişiklik yaparsak git yeni bir branch oluşturuyor.

Print commit history as a tree
==============================
    git log --all --graph

 
Discard all unstaged changes in the current directory (see git reset for more undo-like commands):
====    
    git checkout .

How to set "git aliases"
===========================
    git config --global alias.s "status"

To remove git from our folder
===================
    rm -rf .git

For adding git local files to remote repository
=======================
 - Add a remote:
   
        git remote add {{remote_name}} {{remote_url}}
        git remote add origin https://github.com/serhankars/git-details.git

For listing remote repository
=======================
    git remote -v
    git remote remove origin
    git config --global credential.username "serhankars"

We push 1 branch of commits at a time
=====================
    git push {target} {which_branch_to_push}
    git push origin master

Remote repository ekledikten sonra
git log --all --graph yapınca remote branch de gözüküyor ==> remote tracking branch

To save remote repository name and branch 
=========================================
    git push origin master --set-upstream

So we can only write git push next time
Git push only pushes commits

If we **--amend** and change the last commit, git shows remote repository in a different branch.
And if we try to push the branch with the "overriden commit" using "git push origin master" git will throw an exception.
"hint: Updates were rejected because the tip of your current branch is behind" 
So we need to use -f argument. git push origin master -f

git clone
================
    git clone {url} {foldername}

Remote tracking branches don't update automatically

To update all remote tracking branches
================
    git fetch

To get updates from origin/master to our master branch
=================
    git pull origin master
    git pull origin master --set-upstream

From commited back to staging area
=================
    git reset --soft HEAD~1

---
# BRANCHING

    git branch feature1

We can not use space characters in branch names

    git checkout {branchname}

To delete last one commit
================
    git reset --hard HEAD^

^ operator shows the parent commit.
So **HEAD^^** is equal to the parent of a parent of HEAD.

To delete last **two** commits
================
    git reset --hard HEAD~2

When you merge two branches together the result will go on to the branch that you are currently working on.

### When on master branch
    git merge feature1

Feature Branch Workflow
=====================
    2096 git branch new_feature
    2097 git checkout new_feature 
    2098  touch new_feature.txt
    2099  echo "New Feature" >> new_feature.txt 
    2100  git add .
    2101  git commit -m "New feature added"
    2102  git push origin new_feature
    2096 git branch new_feature
    2097 git checkout new_feature 
    2098  touch new_feature.txt
    2099  echo "New Feature" >> new_feature.txt 
    2100  git add .
    2101  git commit -m "New feature added"
    2102  git push origin new_feature    

1. Create a feature branch
2. Upload feature branch to Github
3. Create a "Pull request"
4. Merge feature branch into master/main branch

To delete a branch name
===================
    git branch -D {branch_name}

To change commits (combine, delete, change message, reorder) with rebase -i
=========================
    git rebase -i HEAD~X :

After interactive rebase all the commits after the changed commit will be replaced: new hash values etc...

Cherrypick: Copying a commit from another branch
===========================
Why use cherry pick? For example you made a commit to a wrong branch, then cherrypick that commit to correct branch and delete old commit

    git checkout {Which branch to copy into}
    git cherry-pick {HASH}

Then delete from the old branch:
    git checkout {Which branch to copy from}
    git reset --hard HEAD~1

Reflog
====================
Reflog is a dairy of what happens to HEAD.
You can see the resetted commits hash in reflog and restore that commits.
    git branch {branchname}  

We can not delete the head branch in git.

To delete the local branch use one of the following:
======================
git branch -d <branch_name>
git branch -D <branch_name>
The -d option is an alias for --delete, which only deletes the branch if it has already been fully merged in its upstream branch.
The -D option is an alias for --delete --force, which deletes the branch "irrespective of its merged status." [Source: man git-branch]

git submodule add {url}

After cloning a repository with submodules
=====================
To also download the submodules:

    git submodule update --init --recursive

OR you can use

    git clone --recurse-submodules {url}

Submodule repositories are always checked out on a specific commit, not a branch.

Because submodules are mostly library code, and we as an owner of main project want to depend on a specific version mostly.

Search & Find
===================
You can combine criterias:

    git log {branch_name}
    git log --after="2017-7-1" --before="2017-7-5"
    git log --grep="ref"
    git log --author="serhankars"
    git log -- README.md => double dash is used so git doesn't confuse file name with branch name
    git log Y..X => Show all commits that are in X but not in Y

git stash
================
To take modifief or staged items to somewhere else.
Stashed items are stored in a stack.

    git stash
    git stash -m 'You can give name'
    git stash list => list entries
    git stash show => what is in it
    git stash pop => back to the top of HEAD commit
    git stash pop --index 1 => to pop a specific stash
    git stash branch Nav {index} => pop the stash with {index} to a new branch
    git stash drop {index}
    git stash clear

git rebase
================
Rebase the current branch on top of another specified branch:

    git rebase {{new_base_branch}}

In workflow using rebase, we first rebase the feature branch on the master branch.

    git checkout {feature_branch}
    git rebase {master}

With this command our feautre brach is now like branched after the latest commit of master.
Then we do:

    git checkout master
    git rebase {feature_branch}  
    git push

So we will have a straight commits line.