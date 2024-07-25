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
