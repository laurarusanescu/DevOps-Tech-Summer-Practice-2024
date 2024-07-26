# 1. Instalare GIT pe UBUNTU

### Check if it is installed: dpkg -l | grep git

The ii in the list means (if there are packages installed, you should see this mark) that the package is correctly installed and available.

apt list git -a

apt-cache pkgnames git

or... Just try run git command

### Installing Git

#### Using apt command

apt install -y git

git --version

# 2. Configurarea Git

## Must haves

So:

* --system - table relevant for the whole machine

* --global - for the current user

* --local (default) for the current repository

Making an entry.

#### git config --global user.name "John Doe"

#### git config --global user.email johndoe@myemail.com

We created our "personality". It can be overwritten with the --local in any of repos.

And now the file .gitconfig in the root directory of our user exist: 

#### ls -al

#### git config --global core.editor vim

Let's see what we have in our config.

git config --list -a ll settings are printed.

git config --list --global - only --global table is listed

git config user.name - selected record is printed.

Configurarea unui Repo:

#### cd test && ls -al

There is a .git directory, which is a hidden one (. on the beginning). This directory is created during git init operation.


Continutul repo-ului .git:

1. hooks: directory contains all custom hooks. These are small (usually) scripts which have to be executed before commit, or after, before push, etc.
2. HEAD - pointer to the current branch and its latest commit.
3. config - configuration file for the repository
4. info - the place where you stage the files using git add.
5. refs - the current state of the whole repo.
6. objects - commits, trees and blobs are stored here. May be very big.
7. logs
8. description - descrierea repositori-ului.

#### cat .git/config

Momentan nu are mare lucru in el, dar dupa ce incepem sa lucram cu trees. branches, remote acesta va avea maimmulte linii.

Ex:
[core]

        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true

### Exemple Utile

#### git config --global color.status auto

#### git config --global color.branch auto

#### git config --global color.interactive auto

#### git config --global color.diff auto

#### git config --global alias.adog "log --all --decorate --oneline --graph" 

Toate aceste comenzi adauga culoare codului .

## adog 
Este cel mai popular mod de a retine cele mai utile seturi de parrametrii pentru **git log**.

#### git config --list

We have a file to commit in the repo. We can do it now, as we configured our user.

git add .

git commit testfile-01 -m "create testfile-01" 

And now it is time to execute our alias.

#### git adog 

## a7b619b (HEAD -> master) create testfile-01

# Commiting Files

### 1.Imitializarea Repo-ului

Create a directory

#### mkdir test-repo

and navigate there

#### cd test-repo

The directory is obviously empty

#### ls -al

Now it is time to initialize repository

#### git init

Now you should be able to execute status command

#### git status

And also the .git directory

#### ls -al 

Creating the first file:

#### touch newfile && echo > newfile
#### git status
Ca si output terminalul ar trebui sa dea new file este: **untracked**.

Adding a file to the staging area:

#### git add newfile

You see that the file is tracked and ready to be commited.

It is important to know, if you stage the file once, you don't need to repeat this action again during the work.

COMMITING A FILE:

Comit este un proces careava salva o versiune a fisierului in Version Control.

## git commit newfile -m "my first commit
E SFANT

Adaugand "-m <comment>" descriem ce se intampla in acest commit.

Folosind argmentul **-a**  dam commit la toate fisierele din repo.

Folosind **.**  comitem schimbarile de pe path ul curent in mod recursiv.

## Adaugarea si Commit-ul mai multor fisiere

The file inside the directory is not visibe, as the directory itself is not tracked yet. But Git is aware, that this directory is not empty, therefore we see it as ready to be added.

Second directory is empty, Git knows that, and it decides that adding the directory at this moment is not needed.

#### Command: git add .

Now, your file is listed as listed to be commited. Empty directory is ommited.

git add . will add all files from your current directory and subdirectories, not from repository's root. If you are in subdirectory, only files from this path will be added.

Now we are ready to commit.

git commit -m "my second commit"

Let's check

git status

You succesfully commited multiple files!

# 4. Removing a file from commit


### Removing a file from stage

A single file:

*  git -rm --cached testfile-01
* git status

Removing multiple/all files from stage:

* git rm --cached -r.
* git status

"." -Folosit sa spunem: TOT DE AICI

'-r' - Inseamna recursiv

### Revenirea la state-ul initial


* git log
* git status

Putem sa vedem ca au aparut niste modificari in fisiere dupa primul commit.

Mai intai verificam ce se afla in fisier:

* cat testfile-01
* git checkout testfile-01
* cat testfile-01
* git status

Am reusit sa resetam cu succes fisierul in starea sa din comit ul initial.

We succesfully reset the file to the state from previous commit, using git checkout. In this way we do the checkout of the last indexed state of this file on current branch.

In similar way like in some scenarios before, we can remove all changes in one short command.

* git checkout .
* git status

### Alta modalitate de a ne intoarce

Reset the current HEAD to the selected state

**git reset** mutaa pointer-ul curent si branch refs in starea specifica.  

**Reset** are 3 nidakitati principale de operare, dar noi ne ocupam de 2 dintre ele.

1. **--soft**

git log shows that we have many changes done. We have one commit for each file.

**git log**

Let't reset the HEAD to the state before commiting the last file.

But before, let's see what is inside the files

* cat testfile-04

* cat testfile-02

* git reset --soft HEAD~1

With --soft parameter we came back to the previous HEAD of the repository, but all changes which we commited are unchanged.

2. **--hard**

O instructiunwe mult mai puternica

**git reset --hard HEAD~2**

Verificam efectele: 

* git status

* git log

* cat testfile-04

* cat testfile-02 

We came back two more commits (~HEAD~2) and we said, this time we want to not only move back, but also we want to remove all changes which were done.


# 5. VISUAL COMMIT

Test editor to create a message for commit

Schimbam modalitatea de a adauga mesaje la commit-uri(-m):

* cd test 
* git commit testfile-01  This was the default editor where we can add stuff(add, commit).

### Default Editor

verificam mai intai configuratia:

git config --global --list | grep editor

What we've done? We asked git to show us the whole configuration in global space. We can use system and local too. Local obviously means the configuration of this specific repo. Also, we grep the output to find information about editor only.

Changing basic editor to VI.

git config --global core.editor vi

# 6. REVERT CHANGES

Git reset functoneaza pentru revenirea la primul commit, dar are un dezavantaj, el fuctionand doar pentru noi pt ca mutaam HEAD in repo ul nostru local.

The best way, best practice to revert, or more "revert" changes is to create new commit from previous one. To avoid situation that HEAD will moved somewhere where it shouldn't be.

## GIT REVERT

We have our repo prepared, two files created and two commits done. Let's see how it looks.

cd test

git status

git log

Let's move ourselves back by one commit.

git revert --no-edit HEAD

We succesfully moved back by creating new commit.

git log

By using --no-edit we informed git that we don't want to pass any message and we ask to use default. We will see what git did for us, using git log soon.

So, now we are back one commit from two already done and... we have three commits. Strange? Please, try to think about the logic behind. We do not want to create problems for other users which can work on the same repo with us. So, we 'go back by going forward'. We keep the history and logic process of work, however we bring back the part of the work from kind of 'backup'.

Our current status is like this: we are between two commits, for file-01 and file-02.

Let's have some fun, and revert our revert. Shall we?

git revert --no-edit HEAD

Before you run next commad, please try to imagine what happened in our git :)

git log

Of course, we can revert more changes than the last commit.

We can revert multiple commits by

git revert --edit HEAD~3

This time we reverted to the beginning of our history. And we forced git to give us possibility to add message (by the way, --edit is default behavior).

So, to make sure we all are on the ame page. HEAD is the current 'place' where we are. For now, let's think about it as combination of current branch and current place in git history.

So, with HEAD~3 we said: I want to move back from HEAD by 3 commits . But that's not all. This place becomes our new HEAD .

We can revert to specific commit hash too. more about hashes in git log scenario.

Important Very often you can experience problems when revert more than one commit and the same file is involed. We will talk about troubleshooting in future lesson.


