% VIM color schemes
% Ravikiran K.S.
% January 1, 2006

## How to create your own vim color schemes

Boilerplate code that must be present in every color scheme:
``
set background=light
highlight clear
if exists("syntax_on")
	syntax reset
endif
let g:colors_name = "Test Light"
``

gui = bold, italic, underline
guibg = #hex value
guifg = #hex value

highlight default <group-name> <key-val-pairs>

.vimrc setting displays the group-name of word under cursor.
set statusline+={%{synIDattr(synID(line('.'),col('.'),1),'name')}}  " show highlight group for current word

Vim provides default colors for both light and dark backgrounds. One only needs
to change the colors that they choose to.

While defining the colorscheme, it is better to specify all key-value pairs
including: guifg, guibg, gui, cterm, ctermfg, ctermbg, term.
If you don't want to specify any value for a key-value pair, put value as NONE.
This will ensure that default colors don't trail, thereby making the
colorscheme display much more reliable. So, simple highlight line would be:

highlight <group-name> cterm=NONE ctermbg=NONE ctermfg=NONE gui=NONE guibg=NONE guifg=NONE

If one highlight option is create, same can be applied to multiple groups by
linking them:

highlight link <group2> <group1>

To know all highlight groups available with vim, 'help highlight-groups'. Some
important groups are:

* Comment	- All sorts of comments.
* Constant	- All types of constants viz.
	* Character - 'c', '\n'
	* String - "Hello World"
	* Number - 123, 0x234
	* Boolean - TRUE, false
	* Float - 2.3e10
* Identifier - Any variable name, including function names.
	* Function - Function name, Method name in Classes
* Statement - Any statement
	* Conditional - if, then, else, endif, switch, etc.
	* Repeat - for, while, do, etc.
	* Label - case, default
	* Operator - sizeof, +, -, *, /, etc.
	* Keyword - Language identified or user specified.
	* Exception - try, catch, throw
* PreProc - Pre processor
	* Include - #include
	* Define - #define
	* Macro - 
	* PreCondit - #if, #else, #endif, etc.
* Type - Any data types - int, char, long, etc.
	* StorageClass - static, volatile, register, etc.
	* Structure - struct, union, enum, etc.
	* Typedef -
* Special - Any special symbol
	* SpecialChar - Any special char in a constant.
	* Tag - Ctags tag
	* Delimiter - ;
	* SpecialComment - 
	* Debug - Debug statements
* Underlined - Like HTML links,
* Ignore - left, blank, hidden, etc.
* Error - any erroneous construct
* Todo - Anything that requires attention.

Reserved group names that can't be used are: NONE, ALL, ALLBUT, contains, contained
If you highlight the top group, you don't need to individually define color for each sub-group.
Also, these group names are not case sensitive.

lightblue=32
darkgrey=235

================================================================================================================
Default Colors - Vim 7.3 FreeBSD.

SpecialKey     xxx term=bold ctermfg=4 guifg=Blue
NonText        xxx term=bold cterm=bold ctermfg=4 gui=bold guifg=Blue
Directory      xxx term=bold ctermfg=4 guifg=Blue
ErrorMsg       xxx term=standout cterm=bold ctermfg=7 ctermbg=1 guifg=White guibg=Red
IncSearch      xxx term=reverse cterm=reverse gui=reverse
Search         xxx term=reverse ctermfg=0 ctermbg=3 guibg=Yellow
MoreMsg        xxx term=bold ctermfg=2 gui=bold guifg=SeaGreen
ModeMsg        xxx term=bold cterm=bold gui=bold
LineNr         xxx term=underline ctermfg=3 guifg=Brown
Question       xxx term=standout ctermfg=2 gui=bold guifg=SeaGreen
StatusLine     xxx term=bold,reverse cterm=bold,reverse gui=bold,reverse
StatusLineNC   xxx term=reverse cterm=reverse gui=reverse
VertSplit      xxx term=reverse cterm=reverse gui=reverse
Title          xxx term=bold ctermfg=5 gui=bold guifg=Magenta
Visual         xxx term=reverse cterm=reverse guibg=LightGrey
VisualNOS      xxx cleared
WarningMsg     xxx term=standout ctermfg=1 guifg=Red
WildMenu       xxx term=standout ctermfg=0 ctermbg=3 guifg=Black guibg=Yellow
Folded         xxx term=standout ctermfg=4 ctermbg=7 guifg=DarkBlue guibg=LightGrey
FoldColumn     xxx term=standout ctermfg=4 ctermbg=7 guifg=DarkBlue guibg=Grey
DiffAdd        xxx term=bold ctermbg=6 guibg=LightBlue guisp=DarkCyan
DiffChange     xxx term=bold ctermbg=5 guibg=LightMagenta
DiffDelete     xxx term=bold cterm=bold ctermfg=4 ctermbg=6 gui=bold guifg=Blue guibg=LightCyan
DiffText       xxx term=reverse cterm=bold ctermbg=1 gui=bold guibg=Red
SignColumn     xxx term=standout ctermfg=4 ctermbg=7 guifg=DarkBlue guibg=Grey
Conceal        xxx cleared
SpellBad       xxx term=reverse ctermbg=1 gui=undercurl guisp=Red
SpellCap       xxx term=reverse ctermbg=4 gui=undercurl guisp=Blue
SpellRare      xxx term=reverse ctermbg=5 gui=undercurl guisp=Magenta
SpellLocal     xxx term=underline ctermbg=6 gui=undercurl guisp=DarkCyan
Pmenu          xxx ctermbg=5 guibg=LightMagenta
PmenuSel       xxx ctermbg=7 guibg=Grey
PmenuSbar      xxx ctermbg=7 guibg=Grey
PmenuThumb     xxx cterm=reverse gui=reverse
TabLine        xxx term=underline cterm=underline ctermfg=0 ctermbg=7 gui=underline guibg=LightGrey
TabLineSel     xxx term=bold cterm=bold gui=bold
TabLineFill    xxx term=reverse cterm=reverse gui=reverse
CursorColumn   xxx term=reverse ctermbg=7 guibg=Grey90
CursorLine     xxx term=underline cterm=underline guibg=Grey90
ColorColumn    xxx ctermbg=7 guibg=LightGrey
MatchParen     xxx term=reverse ctermbg=6 guibg=Cyan
Comment        xxx term=bold ctermfg=4 guifg=Blue
Constant       xxx term=underline ctermfg=1 guifg=Magenta
Special        xxx term=bold ctermfg=5 guifg=SlateBlue
Identifier     xxx term=underline ctermfg=6 guifg=DarkCyan
Statement      xxx term=bold ctermfg=3 gui=bold guifg=Brown
PreProc        xxx term=underline ctermfg=5 guifg=Purple
Type           xxx term=underline ctermfg=2 gui=bold guifg=SeaGreen
Underlined     xxx term=underline cterm=underline ctermfg=5 gui=underline guifg=SlateBlue
Ignore         xxx cterm=bold ctermfg=7 guifg=bg
Error          xxx term=reverse cterm=bold ctermfg=7 ctermbg=1 guifg=White guibg=Red
Todo           xxx term=standout ctermfg=0 ctermbg=3 guifg=Blue guibg=Yellow
String         xxx links to Constant
Character      xxx links to Constant
Number         xxx links to Constant
Boolean        xxx links to Constant
Float          xxx links to Number
Function       xxx links to Identifier
Conditional    xxx links to Statement
Repeat         xxx links to Statement
Label          xxx links to Statement
Operator       xxx links to Statement
Keyword        xxx links to Statement
Exception      xxx links to Statement
Include        xxx links to PreProc
Define         xxx links to PreProc
Macro          xxx links to PreProc
PreCondit      xxx links to PreProc
StorageClass   xxx links to Type
Structure      xxx links to Type
Typedef        xxx links to Type
Tag            xxx links to Special
SpecialChar    xxx links to Special
Delimiter      xxx links to Special
SpecialComment xxx links to Special
Debug          xxx links to Special
BufferSelected xxx term=reverse cterm=bold ctermfg=7 ctermbg=7
BufferNormal   xxx ctermfg=0 ctermbg=7
cType          xxx cleared
cOperator      xxx cleared
vimTodo        xxx links to Todo
vimCommand     xxx links to Statement
vimOption      xxx links to PreProc
vimErrSetting  xxx links to vimError
vimAutoEvent   xxx links to Type
vimGroup       xxx links to Type
vimHLGroup     xxx links to vimGroup
vimFuncName    xxx links to Function
vimNumber      xxx links to Number
vimAddress     xxx links to vimMark
vimAutoCmd     xxx links to vimCommand
vimExtCmd      xxx cleared
vimFilter      xxx cleared
vimLet         xxx links to vimCommand
vimMap         xxx links to vimCommand
vimMark        xxx links to Number
vimSet         xxx cleared
vimSyntax      xxx links to vimCommand
vimUserCmd     xxx cleared
vimCmdSep      xxx cleared
vimIsCommand   xxx cleared
vimVar         xxx cleared
vimFBVar       xxx cleared
vimInsert      xxx links to vimString
vimBehaveModel xxx links to vimBehave
vimBehaveError xxx links to vimError
vimBehave      xxx links to vimCommand
vimFTCmd       xxx links to vimCommand
vimFTOption    xxx links to vimSynType
vimFTError     xxx links to vimError
vimFiletype    xxx cleared
vimFunction    xxx cleared
vimFunctionError xxx links to vimError
vimLineComment xxx links to vimComment
vimSpecFile    xxx links to Identifier
vimOper        xxx links to Operator
vimOperParen   xxx cleared
vimComment     xxx links to Comment
vimString      xxx links to String
vimSubst       xxx links to vimCommand
vimRegister    xxx links to SpecialChar
vimCmplxRepeat xxx links to SpecialChar
vimRegion      xxx cleared
vimSynLine     xxx cleared
vimNotation    xxx links to Special
vimCtrlChar    xxx links to SpecialChar
vimFuncVar     xxx links to Identifier
vimContinue    xxx links to Special
vimAugroupKey  xxx links to vimCommand
vimAugroup     xxx cleared
vimAugroupError xxx cleared
vimFunc        xxx links to vimError
vimParenSep    xxx links to Delimiter
vimSep         xxx links to Delimiter
vimOperError   xxx links to Error
vimFuncKey     xxx links to vimCommand
vimFuncSID     xxx links to Special
vimAbb         xxx links to vimCommand
vimEcho        xxx cleared
vimEchoHL      xxx links to vimCommand
vimExecute     xxx cleared
vimIf          xxx cleared
vimHighlight   xxx links to vimCommand
vimNorm        xxx links to vimCommand
vimNotFunc     xxx links to vimCommand
vimUserCommand xxx links to vimCommand
vimFuncBody    xxx cleared
vimFuncBlank   xxx cleared
vimPattern     xxx links to Type
vimSpecFileMod xxx links to vimSpecFile
vimEscapeBrace xxx cleared
vimSetEqual    xxx cleared
vimSetString   xxx links to vimString
vimSubstRep    xxx cleared
vimSubstRange  xxx cleared
vimUserAttrb   xxx links to vimSpecial
vimUserAttrbKey xxx links to vimOption
vimUserAttrbCmplt xxx links to vimSpecial
vimUserCmdError xxx links to Error
vimUserAttrbCmpltFunc xxx links to Special
vimCommentString xxx links to vimString
vimEnvvar      xxx links to PreProc
vimPatSepErr   xxx links to vimPatSep
vimPatSep      xxx links to SpecialChar
vimPatSepZ     xxx links to vimPatSep
vimPatSepZone  xxx links to vimString
vimPatSepR     xxx links to vimPatSep
vimPatRegion   xxx cleared
vimNotPatSep   xxx links to vimString
vimStringCont  xxx links to vimString
vimSubstTwoBS  xxx links to vimString
vimSubstSubstr xxx links to SpecialChar
vimCollection  xxx cleared
vimSubstPat    xxx cleared
vimSubst1      xxx links to vimSubst
vimSubstDelim  xxx links to Delimiter
vimSubstRep4   xxx cleared
vimSubstFlagErr xxx links to vimError
vimCollClass   xxx cleared
vimCollClassErr xxx links to vimError
vimSubstFlags  xxx links to Special
vimMarkNumber  xxx links to vimNumber
vimPlainMark   xxx links to vimMark
vimPlainRegister xxx links to vimRegister
vimSetMod      xxx links to vimOption
vimSetSep      xxx links to Statement
vimMapMod      xxx links to vimBracket
vimMapLhs      xxx cleared
vimAutoCmdSpace xxx cleared
vimAutoEventList xxx cleared
vimAutoCmdSfxList xxx cleared
vimEchoHLNone  xxx links to vimGroup
vimMapBang     xxx links to vimCommand
vimUnmap       xxx links to vimMap
vimMapRhs      xxx cleared
vimMapModKey   xxx links to vimFuncSID
vimMapModErr   xxx links to vimError
vimMapRhsExtend xxx cleared
vimMenuBang    xxx cleared
vimMenuPriority xxx cleared
vimMenuName    xxx links to PreProc
vimMenuMod     xxx links to vimMapMod
vimMenuNameMore xxx links to vimMenuName
vimMenuMap     xxx cleared
vimMenuRhs     xxx cleared
vimBracket     xxx links to Delimiter
vimUserFunc    xxx links to Normal
vimElseIfErr   xxx links to Error
vimBufnrWarn   xxx links to vimWarn
vimNormCmds    xxx cleared
vimGroupSpecial xxx links to Special
vimGroupList   xxx cleared
vimSynError    xxx links to Error
vimSynContains xxx links to vimSynOption
vimSynKeyContainedin xxx links to vimSynContains
vimSynNextgroup xxx links to vimSynOption
vimSynType     xxx links to vimSpecial
vimAuSyntax    xxx cleared
vimSynCase     xxx links to Type
vimSynCaseError xxx links to vimError
vimClusterName xxx cleared
vimGroupName   xxx cleared
vimGroupAdd    xxx links to vimSynOption
vimGroupRem    xxx links to vimSynOption
vimSynKeyOpt   xxx links to vimSynOption
vimSynKeyRegion xxx cleared
vimMtchComment xxx links to vimComment
vimSynMtchOpt  xxx links to vimSynOption
vimSynRegPat   xxx links to vimString
vimSynMatchRegion xxx cleared
VimSynMtchCchar xxx cleared
vimSynPatRange xxx links to vimString
vimSynNotPatRange xxx links to vimSynRegPat
vimSynRegOpt   xxx links to vimSynOption
vimSynReg      xxx links to Type
vimSynMtchGrp  xxx links to vimSynOption
vimSynRegion   xxx cleared
vimSynPatMod   xxx cleared
vimSyncC       xxx links to Type
vimSyncLines   xxx cleared
vimSyncMatch   xxx cleared
vimSyncError   xxx links to Error
vimSyncLinebreak xxx cleared
vimSyncLinecont xxx cleared
vimSyncRegion  xxx cleared
vimSyncGroupName xxx links to vimGroupName
vimSyncKey     xxx links to Type
vimSyncGroup   xxx links to vimGroupName
vimSyncNone    xxx links to Type
vimHiLink      xxx cleared
vimHiClear     xxx cleared
vimHiKeyList   xxx cleared
vimHiBang      xxx cleared
vimHiGroup     xxx links to vimGroupName
vimHiAttrib    xxx links to PreProc
vimFgBgAttrib  xxx links to vimHiAttrib
vimHiAttribList xxx links to vimError
vimHiCtermColor xxx cleared
vimHiFontname  xxx cleared
vimHiGuiFontname xxx cleared
vimHiGuiRgb    xxx links to vimNumber
vimHiCtermError xxx links to vimError
vimHiTerm      xxx links to Type
vimHiCTerm     xxx links to vimHiTerm
vimHiStartStop xxx links to vimHiTerm
vimHiCtermFgBg xxx links to vimHiTerm
vimHiGui       xxx links to vimHiTerm
vimHiGuiFont   xxx links to vimHiTerm
vimHiGuiFgBg   xxx links to vimHiTerm
vimHiKeyError  xxx links to vimError
vimHiTermcap   xxx cleared
vimCommentTitle xxx links to PreProc
vimCommentTitleLeader xxx cleared
vimSearchDelim xxx links to Statement
vimSearch      xxx links to vimString
vimGlobal      xxx cleared
vimEmbedError  xxx links to vimError
vimAugroupSyncA xxx cleared
vimAuHighlight xxx links to vimHighlight
vimError       xxx links to Error
vimKeyCodeError xxx links to vimError
vimWarn        xxx links to WarningMsg
vimAutoCmdOpt  xxx links to vimOption
vimAutoSet     xxx links to vimCommand
vimCondHL      xxx links to vimCommand
vimElseif      xxx links to vimCondHL
vimSynOption   xxx links to Special
vimKeyCode     xxx links to vimSpecFile
vimSpecial     xxx links to Type
vimFold        xxx links to Folded
vimHLMod       xxx links to PreProc
vimKeyword     xxx links to Statement
vimScriptDelim xxx links to Comment
vimStatement   xxx links to Statement
Normal         xxx cleared
RedundantSpaces xxx ctermbg=7
