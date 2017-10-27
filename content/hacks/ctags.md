% Ctags Tips Tricks
% Ravikiran K.S.
% January 1, 2006

1. ctags support
set tags=<PATH>/tags assigns given tags under vim for browsing
C-] - go to definition
C-T - Jump back from the definition.
C-W C-] - Open the definition in a horizontal split
C-\ - Open the definition in a new tab
A-] - Open the definition in a vertical split
``
Ctrl-Left_MouseClick - Go to definition
Ctrl-Right_MouseClick - Jump back from definition
``
use 'g ctrl-]' instead of just 'ctrl-]' to goto definition

2. To list different kind of files that ctags understands:
[rkks@in-fuchsia][~/test]$ ctags --list-kinds

3. Pattern matching.
Navigate through list of function names that have 'get'
:ta /^get
:ts - shows the list.
:tn - goes to the next tag in that list.
:tp - goes to the previous tag in that list.
:tf - goes to the function which is in the first of the list.
:tl - goes to the function which is in the last of the list.

4. Ctags options
-R -V
-x --c-kinds=f file - Generate Function prototypes
-x --c-kinds=v --file-scope=no file global  variables
--<LANG>-kinds=[+|-]kinds

As an example for the C language, in order to add prototypes and external variable
declarations to the default set of tag kinds, but exclude macros, use option:
--c-kinds=+px-d; to include only tags for functions, use --c-kinds=f.

--fields=+aKlmnSz
--totals=yes
``[rkks@in-fuchsia][~/test]$ ctags --fields=afmikKlnsStz test_sw_num.c``

