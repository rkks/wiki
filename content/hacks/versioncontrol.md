% Version Controls
% Ravikiran K.S.
% January 1, 2006

# CVS

### Checkout entire CVS repository

`$ cvs co .`

### Clean CVS Update

`$ cvs update -C`

NOTE: overwrite local modified files with clean repository copies


# SVN

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

## ClearCase

### Findmerge

Edit the config spec to the base branch or label or tree on which you want to
merge some other branch or label or tree. For example, if your current branch
on which you want to merge some label (ex, fm40-ppc-v2.0.0b01) is opengrid-00,
then you make your config spec to point that subbranch and then do:

$cleartool findmerge . -fver fm40-ppc-v2.0.0b01 -print
[rkks@fuchsia][/vobs/fm40/cp2]$ cleartool findmerge . -fver fm40-ppc-v2.0.0b01 -print

This will show the entire list of files to be merged. Then

$cleartool findmerge . -fver fm40-ppc-v2.0.0b01 -merge
[rkks@fuchsia][/vobs/fm40/cp2]$ cleartool findmerge . -fver fm40-ppc-v2.0.0b01 -merge

will merge the label onto this branch.

``
$ ct findmerge . -serial -nr -fversion /main/fpsctp_rel_3.0_dev/LATEST -merge
``

### Find All elements:

``
cleartool find -all -element '{lbtype_sub(v2_6_6r11) && lbtype_sub(v2_6_7r00)}' -ver '{lbtype(v2_6_6r11) && !lbtype(v2_6_7r00)}' -print
ct lsco -cview -avobs
ct ci -nc *
ct find . -version 'version(.../cccn-fm40-l2ha/LATEST)' -print | awk -F @@ '{print $1}'
grep -w "printf" `ct find . -version 'version(.../cccn-fm40-l2ha/LATEST)' -print | awk -F @@ '{print $1}'`
ct find . -version 'version(.../cccn-fm40-l2ha/LATEST)' -print | awk -F @@ '{print $1}' > list
grep -w "printf" `cat list` | awk -F : '{print $1}' | sort | uniq
``
