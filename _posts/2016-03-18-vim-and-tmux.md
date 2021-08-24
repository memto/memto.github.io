---
layout: post
title: Vim and Tmux
categories: tooltips
tags: vim tmux
---

#### Tmux config (~/.tmux.conf)
	# Our .tmux.conf file

	# Setting the prefix from C-b to C-a
	# START:prefix
	set -g prefix C-a  
	# END:prefix
	# Free the original Ctrl-b prefix keybinding
	# START:unbind
	unbind C-b 
	# END:unbind
	#setting the delay between prefix and command
	# START:delay
	set -s escape-time 1
	# END:delay
	# Ensure that we can send Ctrl-A to other apps
	# START:bind_prefix
	bind C-a send-prefix
	# END:bind_prefix

	# Set the base index for windows to 1 instead of 0
	# START:index
	set -g base-index 1
	# END:index

	# Set the base index for panes to 1 instead of 0
	# START:panes_index
	setw -g pane-base-index 1
	# END:panes_index

	# Reload the file with Prefix r
	# START:reload
	bind r source-file ~/.tmux.conf \; display "Reloaded!"
	# END:reload

	# splitting panes
	# START:panesplit
	bind | split-window -h
	bind - split-window -v
	# END:panesplit

	# moving between panes
	# START:paneselect
	bind h select-pane -L 
	bind j select-pane -D 
	bind k select-pane -U
	bind l select-pane -R    
	# END:paneselect

	# Quick pane selection
	# START:panetoggle
	bind -r C-h select-window -t :-
	bind -r C-l select-window -t :+
	# END:panetoggle

	# Pane resizing
	# START:paneresize
	bind -r H resize-pane -L 5 
	bind -r J resize-pane -D 5 
	bind -r K resize-pane -U 5 
	bind -r L resize-pane -R 5
	# END:paneresize
	# mouse support - set to on if you want to use the mouse
	# START:mouse
	setw -g mode-mouse off 
	# END:mouse
	set -g mouse-select-pane off 
	set -g mouse-resize-pane off 
	set -g mouse-select-window off

	# Set the default terminal mode to 256color mode
	# START:termcolor
	set -g default-terminal "screen-256color"
	# END:termcolor

	# enable activity alerts
	#START:activity
	setw -g monitor-activity on
	set -g visual-activity on
	#END:activity

	# set the status line's colors
	# START:statuscolor
	set -g status-fg white
	set -g status-bg black
	# END:statuscolor

	# set the color of the window list
	# START:windowstatuscolor
	setw -g window-status-fg cyan 
	setw -g window-status-bg default 
	setw -g window-status-attr dim
	# END:windowstatuscolor

	# set colors for the active window
	# START:activewindowstatuscolor
	setw -g window-status-current-fg white 
	setw -g window-status-current-bg red 
	setw -g window-status-current-attr bright
	# END:activewindowstatuscolor

	# pane colors
	# START:panecolors
	set -g pane-border-fg green
	set -g pane-border-bg black
	set -g pane-active-border-fg white
	set -g pane-active-border-bg yellow
	# END:panecolors

	# Command / message line
	# START:cmdlinecolors
	set -g message-fg white
	set -g message-bg black
	set -g message-attr bright
	# END:cmdlinecolors

	# Status line left side
	# START:statusleft
	set -g status-left-length 40 
	set -g status-left "#[fg=green]Session: #S #[fg=yellow]#I #[fg=cyan]#P"
	# END:statusleft

	#START:utf8
	set -g status-utf8 on
	#END:utf8

	# Status line right side
	# 15% | 28 Nov 18:15
	# START: statusright
	set -g status-right "#[fg=cyan]%d %b %R"
	# END:statusright

	# Update the status bar every sixty seconds
	# START:updateinterval
	set -g status-interval 60
	# END:updateinterval

	# Center the window list
	# START:centerwindowlist
	set -g status-justify centre
	# END:centerwindowlist

	# enable vi keys.
	# START:vikeys
	setw -g mode-keys vi
	# END:vikeys


#### Plugin vim
	.vim
	├── autoload
	│   └── pathogen.vim
	├── bundle
	│   ├── ag.vim
	│   ├── ctrlp.vim
	│   ├── supertab
	│   ├── syntastic
	│   ├── vim-fugitive
	│   ├── vim-multiple-cursors
	│   ├── vim-neatstatus
	│   └── vimproc.vim
	└── vimrc

#### Vimrc (~/.vim/vimrc)
	set nocompatible "Vim over vi
	let mapleader = ","

	execute pathogen#infect()
	syntax on
	filetype plugin indent on

	"=== Space setting
	set backspace=2 "backspace deletes like most programs in insert mode
	set tabstop=4 "number of visual spaces per TAB
	set softtabstop=4 "number of spaces in TAB when editing
	set expandtab "tab to spaces
	autocmd FileType make set noexpandtab shiftwidth=4 softtabstop=0
	set scrolloff=3 "space between cursor and terminal bottom
	set undolevels=1500 "how many times the user can undo
	set sidescrolloff=3 "space between cursor and terminal side
	set laststatus=2 "always display the status line
	set autowrite "automatically :write before running commands

	" display extra whitespace
	set list listchars=tab:»·,trail:·,nbsp:·
	" removes trailing spaces
	function! TrimWhiteSpace()
	    %s/\s\+$//e
	endfunction

	nnoremap <silent> <Leader>rts :call TrimWhiteSpace()<cr>
	autocmd FileWritePre    * :call TrimWhiteSpace()
	autocmd FileAppendPre   * :call TrimWhiteSpace()
	autocmd FilterWritePre  * :call TrimWhiteSpace()
	autocmd BufWritePre     * :call TrimWhiteSpace()

	"=== UI setting
	set number "show line numbers
	set ruler "show ruler on right margin of screen
	set showcmd "show last command in the bottom bar
	set visualbell "stop that ANNOYING beeping
	set wildmenu "visual autocomplete for command menu
	set wildmode=list:longest,full
	set guicursor+=i:blinkon0 "disable cursor blink

	" Make searching better
	set gdefault "never have to type /g at the end of search / replace again
	set ignorecase "case insensitive searching (unless specified)
	set smartcase
	set incsearch "search as characters are entered
	set hlsearch "highlight matches
	set showmatch "show matching braces
	"stop highlight after searching
	nnoremap <silent> <leader><space> :noh<cr>

	"=== Mics
	cmap w!! w !sudo tee > /dev/null % "help save when open file without root

	"=== Key mapping
	nnoremap <leader>ev :tabedit ~/.vim/vimrc<cr> :tabm<cr>
	nnoremap <leader>sv :source ~/.vim/vimrc<cr>

	inoremap jk <esc>
	inoremap  <Up>     <NOP>
	inoremap  <Down>   <NOP>
	inoremap  <Left>   <NOP>
	inoremap  <Right>  <NOP>
	noremap   <Up>     <NOP>
	noremap   <Down>   <NOP>
	noremap   <Left>   <NOP>
	noremap   <Right>  <NOP>

	noremap   s  <NOP>

	" window movement
	nnoremap <c-j> <c-w>j
	nnoremap <c-k> <c-w>k
	nnoremap <c-h> <c-w>h
	nnoremap <c-l> <c-w>l
	" open new split panes to right and bottom, which feels more natural
	set splitbelow
	set splitright

	" tab movement
	noremap <c-t> :tabnew<cr>
	noremap <F7> :tabp<cr>
	noremap <F8> :tabn<cr>
	noremap <leader>to :tabnew<cr>
	noremap <leader>tp :tabp<cr>
	noremap <leader>tn :tabn<cr>
	noremap <leader>tc :tabclose<cr>

	"=== Plugin map
	if executable('ag')
	    "use Ag over grep, note we extract the column as well as the file and line number
	    set grepprg=ag\ --nogroup\ --nocolor\ --column
	    "set grepformat=%f:%l:%c%m

	    "use Ag in ctrlP for listing files
	    let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
	    "not need cache ctrlp
	    let g:ctrlp_use_caching = 0
	endif
	let g:ctrlp_max_height = 16

	"nnoremap <leader>p :CtrlP<cr>
	nnoremap <leader>f :Ag "
	nnoremap <leader>bf :AgBuffer "

	" bind to grep word under cursor
	nnoremap <leader>k :grep! "\b<c-r><c-w>\b"<cr>:cw<cr>

	" quick fix window movement
	nnoremap <leader>co :copen<cr>
	nnoremap <leader>cn :cn<cr>
	nnoremap <leader>cp :cp<cr>
	nnoremap <leader>cc :ccl<cr>
	nnoremap <F3> :cp<cr>
	nnoremap <F4> :cn<cr>

	" ignore pattern
	set wildignore +=*.o,*/tmp/*,*.so,*.swp,*.zip,.git*


	"""EOF"""