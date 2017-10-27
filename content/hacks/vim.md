% VIM Cheat Sheet
% Ravikiran K.S.
% January 1, 2006

1. To insert output of a command at current cursor position do:
:r !<command>. For ex. :r !date

2. Goto first line of file - shift-v
3. Goto last line of file - shift-g

4. Rapidly invoke an editor to write a long, complex, or tricky command
ctrl-x ctrl-e

4. Save a file you edited in vim without the needed permissions
:w !sudo tee %

5. Use vim to browse man pages. One can use Ctrl-[ and Ctrl-t
to browse and return from referenced man pages. ZZ or q to quit.
Note initially within vim, one can goto the man page for the
word under the cursor by using ``[section_number]K``.

6. `` - place where last search was started
7. `. - place of last edit

8. To fix backspace issue on vim run: stty erase '^?'

9. Source developer vim settings (overriding defaultts)
if filereadable(expand("$HOME/.vim_dev.rc"))
 source ~/.vim_dev.rc
endif

10. Operating on multiple files. First input all file names using 'args'.
:args <path-url-pattern> For ex. :args app/views/*/*
Then do the operation by 'argdo' command. 'update' is same as 'write' except for
that it writes only if some modification in the file.
:argdo <search/replace expr> For ex. :argdo %s/, :expire.*)/)/gec | update

11. Some important commands
Open all buffers in horizontal split    - :sball
Open Quickfix Window    - :cwin
Complete this filename  - ctrl-x f
Complete from included files - Ctrl-x i

12. In search,
:g/<pattern>/d  - Delete all lines matching pattern
:g!/<pattern>/d  - Delete all lines not matching pattern
patterns can be bundled together by ORing - <pat1>|<pat2>|<pat3>

13. See current vim settings
key mappings - :map, :nmap, :cmap, :imap
abbrevations - :ab, :iab, :cab

14. Use '-u ~/.vimrc' option while starting vim to avoid sourcing files
like /etc/vimrc etc. This can avoid unexpected behaviors and trouble.

15. To enter special characters like  in either insert mode or in command mode, do:
type CTRL-V, then CTRL-M. That is, hold down the CTRL key then press V and M in succession.
Same for other characters.

16. Vim profiling. This is required when vim load times are very high.
a. Write time profiling of startup to file-name
$ vim --startuptime <file-name>
b. Write time profiling when viminfo is not loaded to file-name
$ vim -i NONE --startuptime <file-name>
c. Write time profiling when .vimrc isn't loaded to file-name
$ vim -u NONE --startuptime <file-name>

17. For filetype names used, look into ~/tools/freebsd/vim/share/vim/vim73/filetype.vim

18. Make :set list! which shows visible whitespace, not mangle files
set listchars=tab:>-,trail:.,extends:>

19. Vim Marks and Jumps
Vim records every line number with filename in jumplist. Upto 100 jumps are preserved.
    - To see all places of jump use ":jumps"
    - Use <num>Ctrl-O to jump backward to that jump number
    - Crtl-I to jump forward to next jump location

http://vim.wikia.com/wiki/Jumping_to_previously_visited_locations

Manually marks can be put in file and location for easy jumping back and forward.
    - To mark a particular line number and location 'm<x>'
    - To jump to line with mark <x> use '<x>. To jump to same column of mark <x> use `<x>.
    - To list all marks in vim use ':marks'
    - Jump to previous line with mark [', Jump to next line with mark ]'
    - Jump to previous mark [`, Jump to next mark ]`
http://vim.wikia.com/wiki/VimTip42

## Vim shortcuts

% - reach matching else or } or ) or ] or #else.
# - search backward
* - search forward
= - indent the paragraph

`. - goto last modified place
[[ - previous { brace
]] - next { brace
'' - goto previous place

]c - next change
[c - previous change
do - diff obtain
dp - diff put

zi - Toggles between open/close of folded text

ma - mark current line as 'a' - similary mb, mc etc.
'a - goto mark 'a' - similarly 'b, 'c etc.
:buffers - show all marks
:bd num - close/delete given buffer

:cf - take to first compile error
:cl - list the errors
:cn - goto next compilation error
:e# - goto previous file

gg=G - indent the whole file at one go
gUw - Change the word to upper case

:%s/foo/bar/g - replace all foo with bar in current file
:bufdo %s/foo/bar/g - do the same in all files that I open
:tabdo %s/foo/bar/g - do the same across all tabs that I open

:ls - list all open files
:cd - cd
:pwd - pwd
:make - build using make

ctrl+l - redarw
ctrl+p or ctrl+n - complete the word
ctrl+w ctrl+w - switch windows
ctrl+w gf - open file under cursor in a new tab
ctrl-w ctrl-- - Maximize current window.
ctrl-w = - Realign all windows.

gg - Goto beginning of file
G - Goto end of file
Ctrl-V + gq - reformat selected text to textwidth
