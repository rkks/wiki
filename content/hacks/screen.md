% GNU Screen Nits
% Ravikiran K.S.
% January 1, 2006

Q: The GNU screen is not reponsive, seems like blocked
A: Try Ctrl-A q, which is the sequence to unblock scrolling. Ctrl-A s is the
sequence that blocks scrolling, which makes screen seem like it freezes. Also
replace Ctrl with whatever your escape key is for screen commands.

With a great deal of experimentation, I was able to recover the screen session
as follows:

    Lookup the PID of the server screen process: ps | grep screen
    Send the server a HUP signal: kill -1 <PID>
    Run a screen client: screen -r -d

# Reread the configuration file without exiting screen
ctrl-a : source ~/.screenrc

#Allow mousewheel to handle the scrollbar in screen
# Scroll up
``
bindkey -d "^[[5S" eval copy "stuff 5\025"
bindkey -m "^[[5S" stuff 5\025
``
# Scroll down
``
bindkey -d "^[[5T" eval copy "stuff 5\004"
bindkey -m "^[[5T" stuff 5\004
``
# Scroll up more
``
bindkey -d "^[[25S" eval copy "stuff \025"
bindkey -m "^[[25S" stuff \025
``
# Scroll down more
``
bindkey -d "^[[25T" eval copy "stuff \004"
bindkey -m "^[[25T" stuff \004
``
#This override creates problem in vim shortcuts don't work.
``
bindkey ^. prev # previous window
bindkey ^/ next # next window
``

### Other hardstatus string options
``
" [ %H ] %{wb} %c:%s | %d.%m.%Y %{wr} Load: %l %{wb} %w "
'%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %M/%d %{W}%c %{g}]'
"%{.bW}%-w%{.rW}%n %t%{-}% w %=%{..G} %H %{..Y} %m/%d %C%a "
'%{= kg}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{=b kR}(%{W}%n*%f %t%?(%u)%?%{=b kR})%{= kw}%?% Lw%?%?%= %{g}]%{=b C}[ %d %M %c ]%{W}'
'%{= mK}%-Lw%{= KW}%50>%n%f* %t%{= mK} %+Lw%< %{= kG}%-=%D %d %M %Y %c:%s%{-}'
" [ %H ] %{wb} %c:%s | %d.%m.%Y %{wr} Load: %l %{wb} %w "
"%{rW}rkks ==>%{yk}   %-w%{gk}[%n %t]%{yk}%+w   %{bW}    -|%|-    Host:%H"
``

http://aperiodic.net/screen/interface
