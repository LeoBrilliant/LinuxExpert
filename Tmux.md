
# protocol version mismatch (client 8, server 6) when trying to upgrade
The old tmux server is still running, kill it and run the new server.
- https://unix.stackexchange.com/questions/122238/protocol-version-mismatch-client-8-server-6-when-trying-to-upgrade

# tmux shortcuts
- https://gist.github.com/MohamedAlaa/2961058

# rename a window
C-b-,
C-b-: rename-window new-name
tmux rename-window new-name
- https://stackoverflow.com/questions/40234553/how-to-rename-a-pane-in-tmux

# Copy mode 
C-b-[ 
in this mode, we can use PageUp or PageDown to scroll the screen
- https://superuser.com/questions/209437/how-do-i-scroll-in-tmux

# copy-mode-vi
set-window-option -g mode-keys vi # or emacs, setw is alias ]
The following commands are supported in copy mode:
Command	vi	emacs
append-selection		
append-selection-and-cancel	A	
back-to-indentation	^	M-m
begin-selection	Space	C-Space
bottom-line	L	
cancel	q	Escape
clear-selection	Escape	C-g
copy-end-of-line	D	C-k
copy-line		
copy-pipe <command>		
copy-pipe-and-cancel <command>		
copy-selection		
copy-selection-and-cancel	Enter	M-w
cursor-down	j	Down
cursor-left	h	Left
cursor-right	l	Right
cursor-up	k	Up
end-of-line	$	C-e
goto-line <line>	:	g
halfpage-down	C-d	M-Down
halfpage-up	C-u	M-Up
history-bottom	G	M-<
history-top	g	M->
jump-again	;	;
jump-backward <to>	F	F
jump-forward <to>	f	f
jump-reverse	,	,
jump-to-backward <to>	T	
jump-to-forward <to>	t	
middle-line	M	M-r
next-paragraph	}	M-}
next-space	W	
next-space-end	E	
next-word	w	
next-word-end	e	M-f
other-end	o	
page-down	C-f	PageDown
page-up	C-b	PageUp
previous-paragraph	{	M-{
previous-space	B	
previous-word	b	M-b
rectangle-toggle	v	R
scroll-down	C-e	C-Down
scroll-up	C-y	C-Up
search-again	n	n
search-backward <for>	?	
search-forward <for>	/	
search-backward-incremental <for>		C-r
search-forward-incremental <for>		C-s
search-reverse	N	N
select-line	V	
start-of-line	0	C-a
stop-selection		
top-line	H	M-R
- http://man.openbsd.org/OpenBSD-current/man1/tmux.1

# Enable the mouse scroll
$ vim ~/.tmux.conf
set -g mouse on
or setw -g mode-mouse on
- https://superuser.com/questions/210125/scroll-shell-output-with-mouse-in-tmux

# select and copy the text
mouse on
press shift key while left mouse button down.
paste the test with shift key and middle-button.
- https://stackoverflow.com/questions/17445100/getting-back-old-copy-paste-behaviour-in-tmux-with-mouse

# break-pane and join-pane
break-pane [-d] [-t target-pane]
                   (alias: breakp)
             Break target-pane off from its containing window to make it the
             only pane in a new window.  If -d is given, the new window does
             not become the current window.

join-pane [-dhv] [-l size | -p percentage] [-s src-pane] [-t dst-pane]
                   (alias: joinp)
             Like split-window, but instead of splitting dst-pane and creating
             a new pane, split it and move src-pane into the space.  This can
             be used to reverse break-pane.

src-pane could not be the current pane.

- https://superuser.com/questions/238702/maximizing-a-pane-in-tmux

# select-pane 
select-pane -t 0
- https://stackoverflow.com/questions/18644359/select-pane-with-c-number-in-tmux

