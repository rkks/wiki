% SVN Commands
% Ravikiran K.S.
% January 1, 2006

## Tips

  - For SVN to be able to open any file in your favorite editor, update
    EDITOR environment variable with your editor path: `$ export
    EDITOR=vim`

## Commands

### Checkout workspace

`$ svn co https://svn-repo.com/ .`

### Check whether workspace is up-to-date

`$ svn status -u`

### Any changes(add/modify/delete) in workspace

`$ svn status`

### Update workspace

`$ svn update [-r <rev>]`

### View differences

`$ svn diff <filename>` `$ svn compare -r R1:R2 <filename>` Ex: `$ svn
diff -r 636971:636544 –summarize` `$ svn diff -r 636971:636544`

### Commit changes

`$ svn commit`

### Check changes to given file

`$ svn log -v [-r<rev>]`  
Ex: `$ svn log
https:<svn-server>/svn/<repo>/branches/<branch-name>/<path-to-file> >
file-name.log Then: `$ svn diff -c \<rev-num\>
https:\<svn-server\>/svn/\<repo\>/branches/\<branch-name\>/\<path-to-file\>
\> file-name.diff

### Add new file to repo

`$ svn add <filename>`

### Get help on any svn command

`$ svn help <command>`

### Add svn ignore to a repo

`$ svn propset svn:ignore -F ~/.svnignore .`

REF:
[http://sethholloway.com/blog/2011/03/10/svnignore-example-for-java/](http://sethholloway.com/blog/2011/03/10/svnignore-example-for-java/ "http://sethholloway.com/blog/2011/03/10/svnignore-example-for-java/")

### For errors like '\> local edit, incoming delete upon update' on svn update/merge

`$ svn remove –force <filename/dirname>` OR `$ svn resolve
–accept=working <filename/dirname>`

REF:
[http://little418.com/2009/05/svn-local-obstruction-incoming-add-upon-merge.html](http://little418.com/2009/05/svn-local-obstruction-incoming-add-upon-merge.html "http://little418.com/2009/05/svn-local-obstruction-incoming-add-upon-merge.html")

### To get list of files modified between two dates:

`svn diff -r{2013-03-09}:{2013-03-22} –summarize
https://svn-server.com/svn/repo/branches/BRANCH_TAG > output.txt`

NOTE: To get the source code changes between dates, remove '–summarize'
option.

### List last 10 commits

`$ svn log –limit 10`

