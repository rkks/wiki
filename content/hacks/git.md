% Git Tips Tricks
% Ravikiran K.S.
% January 1, 2006

### Try this for use of external diff to generate patch
git difftool --extcmd="diff -Nacp" --no-prompt HEAD
git difftool --extcmd="diff -Nwup" --no-prompt HEAD | less

### Change configuration
$ git config --global user.name "Ravikiran K.S."
$ git config --global user.email "ravikirandotks@gmail.com"
$ git config --global core.autocrlf input
$ git config --global core.safecrlf true
$ cat ~/.gitconfig

### Create a repo
$ mkdir hello && cd hello
$ git init
Initialized empty Git repository in /.amd/bng-eng001-cf2/vol/build24/bng-nfsbuild24/raviks/hello/.git/

### Add files to repo
$ cp ~/test/template/* .
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       Makefile
#       README
#       template.c
#       template.h
nothing added to commit but untracked files present (use "git add" to track)
$ git add *
$ git commit -m "Initial commit"
[master (root-commit) e125b69] Initial commit
 4 files changed, 97 insertions(+)
 create mode 100755 Makefile
 create mode 100644 README
 create mode 100644 template.c
 create mode 100644 template.h
$ git status
# On branch master
nothing to commit (working directory clean)

### Review changes to repo
$ git log
commit e125b6951011747e7d6d7f01c554ebd7e3428488
Author: Ravikiran K.S <ravikirandotks@gmail.com>
Date:   Wed Nov 14 10:42:04 2012 +0530

    Initial commit

### Change repo file
$ vi template.c
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   template.c
#
no changes added to commit (use "git add" and/or "git commit -a")

### Stage the modified file for commit later
$ git add template.c
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   template.c
#

### Commit the staged changes. This can be done any time.
$ git commit -m "Changed template.c"
[master a0900b4] Changed template.c
 1 file changed, 2 insertions(+), 1 deletion(-)

### Display summary of changes to repo
$ git log --pretty=oneline
a0900b4a551931aa221a993010351c6d55efe21e Changed template.c
e125b6951011747e7d6d7f01c554ebd7e3428488 Initial commit

Normal 'git log' is equivalent to 'git log HEAD master'
