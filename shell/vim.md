# Vim tricks

## using `execute` `normal` `<Plug>` and `repeat.vim` inside a vim function

```viml
function! s:blankDown()
  execute "normal mzo\<esc>`z"
  silent! call repeat#set("\<Plug>(blankDown)", 1)
endfunction
nnoremap <Plug>(blankDown) :<C-u>call <SID>blankDown()<CR>
nmap K <Plug>(blankDown)
```
## Top rebindable binds

* `g + char`: `gb`, `gf`, `gh`,`gl`,`gp`,`gr`,`gs`,`gx`,`gy`,`gz`, `go`, `g<cr>`, `g<space>`.
* `K`: word lookup does not seem to be of much use.
* `&`: repeat last substitute. rarely useful.
* `s`, `S`: easily replaceable for `cl` and `cc`, respectively.

## navigating backwards/forwards `n` writes

* `h: earlier` and `h: later` (or `:h g+`, `:h g-`)

## native MRU

* just type `:browse oldfiles`

## dictionary and spell

### building a dictionary file

* download [pt_BR.zip](http://linorg.usp.br/OpenOffice.org/contrib/dictionaries/pt_BR.zip) file.
* build the spell archive with:

```vim
:mkspell pt pt_BR
```

* set the spell on

```vim
:setlocal spell spelllang=pt
```

* or set it off

## Simulating a __Zoom__

* `:tab split` will open the current buffer in a new tab.
* `:tab help nvim-from-vim` will open the help topic in a new tab.
* `:tabclose` will close the current tab.

## Regex

### visual block regex (with Dr. Chipp's vis plugin)

* substituting `1    2` for `1,2` using `\zs,\ze`

```vim
:'<,'>B s/\v\d\zs\s+\ze\d/,/g
```

* substituting `1    2` for `1,2` using matching groups

```vim
:'<,'>B s/\v(\d)\s+(\d)/\1,\2/g
```

* substituting anything for a newline. (use `\r` instead of `\n`). More info [here](http://stackoverflow.com/a/71334/4921402).

```vim
:s/acharacter/\r/g
```

## Performance Profiling

* check vim startup time

```sh
$ vim [file] --startuptime startup.log
$ sort -rk 2 < startup.log | vim -
```

*  link: vimcast about performance [vimcast: Profiling vimscript performance](http://vimcasts.org/episodes/profiling-vimscript-performance/)

* vim help on startup time`h: slow-start` 

## movements

* `n|`: moves to the `n-th` column
* `zt | z<cr>`: Shifts page content so current line sits at the top of the viewport
* `zb | z`: "Shifts page content so current line sits at the bottom of the viewport"
* `zz | z`:  "Shifts page content so current line sits at the middle of the viewport"
* 200zt:  positions line 200 on top of the screen.
* `H`: Moves cursor to the top of the viewport
* `M`: Moves cursor to the middle of the viewport
* `L`: Moves cursor to the bottom of the viewport"
* `gv`: selects again the last visual selection.
* `o` | `v_o`: when a block of text is selected you can use `o` to move to the beggining of the selection. Use it again to go to the end of the selection.
* `g; e g,`:  back and forward na changelist (:changes)
* `<c-i>, <c-o>` -> back and forward na jumplist (:jumps ou :ju)
* jump to last edited line: `'.`

## folding

* `zi` toggles all folds of the current buffer.
* `zr` decreases the foldlevel by one.
* `zR` decreases the foldlevel to zero -- all folds will be open.
* `zm` increases the foldlevel by one.
* `zM` closes all open folds.
* `[z` move to start of open fold.
* `]z` move to end of open fold.
* `zj` moves the cursor to the next fold.
* `zk` moves the cursor to the previous fold.
* `zO` opens all folds at the cursor.
* `zo` opens a fold at the cursor.
* `zc` closes a fold at the cursor.
* `zd` deletes the fold at the cursor.
* `zE` deletes all folds.
* `zf#j` creates a fold from the cursor down # lines.
* `zf/string` creates a fold from the cursor to string .

## text objects

* `e`: vim-textobj-entire
* `x`: vim-textobj-xmlattr
* `gn`: -> para fazer o match do last pattern.
* `w`: palavra
* `t`: html tag
* `{`, `(`, `[`, `'`: objetos surround do tim pope
    * `ys + obj + surround`: 'you surround'. ex: `ysiw'` coloca asapas em volta de um objeto `iw`.
    * `ds + surround`: apaga o surround
    * `cs + oldsurround + newsurround`: muda o surround

## block mode

* para 'emparelhar' dois blocos de texto
    1. entra no block mode, seleciona as linhas e aperta `$` para ir até o final de todas
    2. insere 2 espaços no começo de cada linha
    3. seleciona a primeira coluna de espaços em block mode
    4. aperta `p` para substituir a primeira coluna de espaços pelo bloco desejado.

## vimdiff

* `:diffput`
* `:diffget`
* `:diffupdate`
* `:diffoff!`
* `:windo` diffthis
* `[c` | `]c` -> movimentação entre diffs.
* para comparar (diffar) dois buffers abertos verticalmente um do lado do outro
    * para começar o diff `:windo diffthis`
    * para terminar: `diffoff!`

## copy/paste

  * `<C-R> + registrador`: paste from register in insert mode. repeatable with `.` operator.
  * `<C-R> + <C-O>`: resultado "mais dinâmico". ver [link](http://vimcasts.org/episodes/pasting-from-insert-mode).

## searching

* `g*` ou `g#` searches for the partial word

### vimgrep and the arglist

* search for a pattern on the open files

```vim
:bufdo :args ## %
:vim[grep] blah ##
```

## shell interaction

### suspending vim

* `ctrl + z` -> suspends vim.
* seeing what processes are suspended: `$ jobs`
* unsuspend vim: `$ fg`

### invoke a clean vim (as `zsh -f`)

* `vim -u NONE`

### invoke vim with a specific config

```sh
$ vim -u NONE -N -c ':set runtimepath+=~/.vim/bundle/vim-sneak/' -c ':runtime plugin/sneak.vim'
```

## lists

* command list: `q:`
* search list: `q/`
* change list: `:changes`
* jump list -> `:jumps` ou `:ju`


## ex mode commands

* very useful: `<c-v>` on ex-mode escapes special combos like `<C-a>`, or `<ESC>`. Here we simulate a 'surround' on every first word of all lines of the file: `:%norm cw""P`
* increase next number of all selected lines: `:'<,'>norm ^A`
* decrease next number of all selected lines: `:'<,'>norm ^X`
* exec @a macro on visually selected lines: `:'<,'>norm @a`
* exec @a macro on a range defined by marks 'a' and 'b'.  `:'a,'bnorm @a`
* open quickfix window: `:copen`
* turns buffer into a disposable/scratch buffer: `:set buftype=nofile`
* search with 'very magic' mode on:  `/\v searchstring`
* generate help files from within vim:  `:helpt[ags] ~/.vim/bundle/${plugindir}/doc`
* replace each match of the last pattern with 'bar' within the selected lines in visual mode: `:'<,'>s//bar/g`
* for sorting a range. the 'u' optional parameter will delete duplicate lines: `:'<,'>sort [u]`
* toggle line numbers: `:set nu!`
* toggle relative line numbers: `:set rnu!`
* search/replace com atalho: acha os highlight com '/' e depois soca: `%s//nova/g`
* `:e!`: reverte o buffer para o estado original do arquivo sem sair do vim.

## show the name of the current file

* to show the full qualified path/name of the current file: 

```vim
:echo expand('%:p')

" or
:let @a = expand('%:p')
:put a

" or, more easily, just type `<C-g>`
```

* force the syntax of a buffer `:set syntax=html`
* show/hide relative line numbers `:set rnu`, `:set nornu`
* show/hide line numbers `:set nu`, `:set nonu`

## vimrc hacks

* configuring vimrc through environment variables (more info [here](https://soledadpenades.com/2013/05/25/using-environment-variables-for-configuring-vim/))

```viml
let shell_option_fold_start=$SHELL_VIM_FOLD_START
if (shell_option_fold_start ==# "true")
  set foldlevelstart=0
else
  set foldlevelstart=3
endif
```

* highlighting trailing whitespace

```vim
highlight ExtraWhitespace ctermbg=218 guibg=218
match ExtraWhitespace /\s\+$/
autocmd BufWinEnter * match ExtraWhitespace /\s\+$/
autocmd InsertEnter * match ExtraWhitespace /\s\+\%#\@<!$/
autocmd InsertLeave * match ExtraWhitespace /\s\+$/
autocmd BufWinLeave * call clearmatches()
```

* changing diff colors

```vim
fun! SetDiffColors()
  highlight DiffAdd    cterm=bold ctermfg=black ctermbg=DarkGreen
  highlight DiffDelete cterm=bold ctermfg=black ctermbg=DarkGrey
  highlight DiffChange cterm=bold ctermfg=black ctermbg=DarkBlue
  highlight DiffText   cterm=bold ctermfg=black ctermbg=DarkRed
endfun
autocmd FilterWritePre * call SetDiffColors()
```

* draw a vertical ruler

```vim
autocmd BufWinEnter * highlight ColorColumn ctermbg=darkred
set colorcolumn=201
```

## plugin tricks

### junegunn/vim-easy-align

* easy align any delimiter
    1. manually select line with `V`
    2. `<CR>` to enter easy align mode
    3. `*` to select all delimiters on the line
    4. `<C-X>` to enter a regular expression
    5. type your delimiter in regex form.
    6. done!

### jiangmiao/auto-pairs

* fix: inserting `â`
```viml
Plug 'jiangmiao/auto-pairs'
" auto-pairs plugin maps 'â' in insert mode limiting our ability to type 'â'.
<!-- " let g:AutoPairsShortcutBackInsert='' -->
```

### chaoren/vim-wordmotion

* sequester of normal mode `w` sucks sometimes

```viml
Plug 'chaoren/vim-wordmotion'
" just run `$ man cmd` on the terminal.
" it's the same as hitting `K` over a word.
" let us try to leave with :normal!
" eg: `:norm! ciw`
" let g:wordmotion_prefix = 'K'
```

### terryma/vim-multiple-cursors

* better defaults

```viml
Plug 'terryma/vim-multiple-cursors'
let g:multi_cursor_exit_from_visual_mode = 0
let g:multi_cursor_exit_from_insert_mode = 0
```

### netrw (ok, not really a plugin, but bear with me)

* options for configuring netrw

```viml
" options: netrw {{{2
" let g:loaded_netrwPlugin = 1
let g:netrw_banner       = 0
let g:netrw_bufsettings  = 'relativenumber'
let g:netrw_keepdir      = 0
let g:netrw_liststyle    = 1
let g:netrw_sort_options = 'i'
```

## other tricks

* force a specific syntax on a file put this on top or bottom

```vim
# -*- mode: markdown -*-
# vi: set ft=markdown :
```

* break a really long line into smaller lines. more info [here](http://stackoverflow.com/a/1272247/4921402).

```vim
gq{motion} % format the line that {motion} moves over
{Visual}gq % format the visually selected area
gqq        % format the current line
:h gq
:h gw
:set tw=80

" setting textwidth (tw) will give you auto line break when exceeded during typing. 
" It is used in gq too, though if disabled gq breaks on window size or 79 depending on which comes first.
```

* source or edit your vimrc

```viml
" sourcing
:so $MYVIMRC

"editing
:e $MYVIMRC
```

* delete all blank lines (__awesome__)

```vim
:v/./d
```

* edit the current command line in vim

```viml
<C-f>
```

* put the cursor after the end of the line or file. See `:h blockwise-examples` for more info.             a

```viml
" activate
:set virtualedit=all

" deactivate
:set virtualedit=

"now try this
$10l 
```
