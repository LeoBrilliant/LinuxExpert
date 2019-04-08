
# 加载文件内容
:read filename

# vim中文乱码

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8

# vim字符串计数
假设你要统计的字符为"test",现在输入
%s/test/&/gn(回车)
vim设置4个空格
http://www.cnblogs.com/wi100sh/p/4938996.html

# vim reload
:e
:edit

# vim
Undo u :undolist later
Redo Ctrl + R
- http://stackoverflow.com/questions/1555779/how-do-i-do-redo-i-e-undo-undo-in-vim
copy y
copy the line yy or Y
paste after the current line p
paste before the current line P
- http://stackoverflow.com/questions/73319/duplicate-a-whole-line-in-vim

## select with mouse but can't copy
:set mouse=v #set mouse=r on some platform, or put this in .vimrc
- https://unix.stackexchange.com/questions/139578/copy-paste-for-vim-is-not-working-when-mouse-set-mouse-a-is-on
- https://superuser.com/questions/1223924/cant-select-text-in-terminal-after-upgrading-to-vim-8-on-debian-9-stretch
- https://superuser.com/questions/436890/cant-copy-to-clipboard-from-vim

# syntax
:syntax on
- https://www.cyberciti.biz/faq/turn-on-or-off-color-syntax-highlighting-in-vi-or-vim/

# vim
## 以16进制显示内容
```
:%!xxd
# 返回正常显示
:%!xxd -r

# 以两个字节为一组显示
:%!xxd -g 2
```

## 复制多行
y10j # 复制了11行

# Search
/int\> # 查找以int结尾的字符串
/\<int # 查找以int开头的字符串
在所有行中查找 字符串 出现的次数
:%s/字符串/&/gn

# Gvim Ctrl + V快捷键冲突
- 使用Ctrl + Q，然后Shift + 方向键选中区域
- 或者在.vimrc文件中注释掉 bahave mswin
- https://stackoverflow.com/questions/14163642/keyboard-only-column-block-selection-in-gvim-win32-or-why-does-ctrl-q-not-emula
- https://stackoverflow.com/questions/426896/vim-ctrl-v-conflict-with-windows-paste

## Linux vim中文乱码
:set fileencoding # 查看文件编码
:set fileencodings # 查看文件可用编码
:set termencoding # 查看终端显示编码

编辑~/.vimrc文件，加上如下几行：

set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8

- http://www.cnblogs.com/joeyupdo/archive/2013/03/03/2941737.html


# 查找忽略大小写
/\csettlement
/settlement\c

设置查找高亮
:set hlsearch

# 翻转行序
:g/^/m0
- http://vim.wikia.com/wiki/Reverse_all_lines

# 查找时返回最后一个命中
?make_pair
/make_pare 回车 ? 回车

## SSH免密码登录
$ssh-keygen -t rsa #生成密钥对
- 将~/.ssh/id_rsa.pub的内容复制到目标机器的~/.ssh/authorized_keys中，需要注意是否有多余的空格。
- 如果出现"Agent admitted failure to sign using the key."，需要将生成的id_rsa加载到SSH Agent中。
$ssh-add ~/.ssh/id_rsa
这时，就可以实现SSH免密码登录了。

- 显示登录过程中的详细信息
ssh -v sc@toyger0.wks.in.shancetech.com

- http://www.cnblogs.com/dlutxm/archive/2011/10/14/2212019.html
- https://help.github.com/articles/error-agent-admitted-failure-to-sign/

# 查找
:vimgrep /\<handleClientMsg\>/ **/*

ll -t *.log | awk '{ if (NR == 1) print $NF }' | xargs dfview | tf | vim - -R

- https://unix.stackexchange.com/questions/102089/how-to-pipe-the-result-of-a-grep-search-into-a-new-vi-file 将Shell的执行结果重定向到vim
- https://askubuntu.com/questions/510890/how-do-i-redirect-command-output-to-vim-in-bash 同上

- http://uniquejava.iteye.com/blog/1485883 改变工作路径
- http://stackoverflow.com/questions/7880372/how-to-jump-between-patterns-when-using-vimgrep-quickfix-list vimgrep跳转匹配项
- http://vim.wikia.com/wiki/Find_in_files_within_Vim vimgrep tutorial
- http://stackoverflow.com/questions/1747091/how-do-you-use-vims-quickfix-feature vim QuickFix
- http://stackoverflow.com/questions/6852763/vim-quickfix-list-launch-files-in-new-tab 在新tab中打开查找结果
- https://vi.stackexchange.com/questions/6996/how-to-make-enter-open-new-tabs-for-the-quickfix-window-when-it-is-opened-with/6997 同上
- http://stackoverflow.com/questions/7950558/how-can-i-search-a-word-in-whole-project-folder-recursively grep基本使用
- http://coolshell.cn/articles/11312.html Vim无插件使用技巧

## 正则表达式查找当前文档
/abc.*xyz # 查找abc*xyz
- https://stackoverflow.com/questions/30975389/vim-search-using-regular-expression

## 正则表达式概要
- http://www.cnblogs.com/myyan/p/4969872.html

# 替换Replace
## 替换^M符号
:%s/^M//g # Ctrl + V + M
:%s/\r//g
:%s/\r/\r/g
- https://stackoverflow.com/questions/5939142/replacing-carriage-return-m-with-enter

% sed -e "s/^M//" filename > newfilename
- https://its.ucsc.edu/unix-timeshare/tutorials/clean-ctrl-m.html

%s的方法试过了没用，sed的没试过。后来不知道在哪里看到了一个方法，用:set fileformat=unix然后保存一下就OK了。因为文件被Mac或者Windows打开再保存之后，fileformat就变成了dos。

## 替换行末空格
:%s/\s\+$//g

## 替换空格
:retab
- https://stackoverflow.com/questions/426963/replace-tab-with-spaces-in-vim

## _vimrc文件修改后无法保存
文件权限问题，因为VIM装在C盘，不能随便修改文件。可以修改文件的权限，或者将VIM装在其他目录。
- https://www.oschina.net/question/250634_80651 

## Gvim中文菜单乱码及语言设置
- http://blog.csdn.net/laruence/article/details/2603031 菜单乱码
- http://blog.csdn.net/njnu_mjn/article/details/7935103 界面设置成英文

## vim-color-solarized
- https://github.com/altercation/vim-colors-solarized 

## 设置主题颜色
:colorscheme morning 

## 粘贴时取消缩进
:set paste
- https://stackoverflow.com/questions/2514445/turning-off-auto-indent-when-pasting-text-into-vim 

## 当前文件取消缩进
:setlocal noautoindent " nocindent nosmartindent 
:setl noai nocin
- http://vim.wikia.com/wiki/How_to_stop_auto_indenting 
## Book
- http://vimdoc.sourceforge.net/index.php
- http://www.oualline.com/vim-cook.html
- https://github.com/doomzhou/vlb/blob/master/Practical-Vim-Edit-Text-at-the-Speed-of-Thought.pdf

- https://github.com/redguardtoo/mastering-emacs-in-one-year-guide/blob/master/guide-zh.org

# Emacs
## 重新加载.emacs
M-x load-file
M-x eval-buffer
C-x C-e
- https://stackoverflow.com/questions/2580650/how-can-i-reload-emacs-after-changing-it
- https://stackoverflow.com/questions/167705/reload-configurations-without-restarting-emacs

## 教程
- http://orgmode.org/worg/org-tutorials/orgtutorial_dto.html 初级入门

## 时区及环境
- https://stackoverflow.com/questions/1571141/setting-timezone-of-org-mode
- https://www.gnu.org/software/emacs/manual/html_node/elisp/Time-Zone-Rules.html
- https://www.gnu.org/software/emacs/manual/html_node/calc/Time-Zones.html

## 自动换行 auto line wrapping
- https://emacs.stackexchange.com/questions/18480/wrap-line-text-in-org-mode-when-i-re-open-a-file
(setq org-startup-truncated nil)
- https://gist.github.com/liangzan/2939812
- https://superuser.com/questions/299886/linewrap-in-org-mode-of-emacs

## 自动重新载入文件
:set autoread
:au CursorHold * checktime

也可以将上面两句加入到.vimrc。不过感觉延时还是挺大的。
- https://stackoverflow.com/questions/2157914/can-vim-monitor-realtime-changes-to-a-file
- https://superuser.com/questions/181377/auto-reloading-a-file-in-vim-as-soon-as-it-changes-on-disk

## Ubuntu上将vim中的内容复制到系统剪贴板
需要使用vim-gnome或者vim-gtk。主要是使用了X11和clipboard两个features。当前vim的属性可以通过vim --version查看。可以从vim的源代码编译，注意编译开关。

- https://stackoverflow.com/questions/10101488/cut-to-the-system-clipboard-from-vim-on-ubuntu

# vim
## search
- first match ggn
- last match ggN
- https://stackoverflow.com/questions/15707618/vim-find-last-occurrence-of

- current word *
- search word count :%s/pattern//gn :%s/pattern//n
- http://vim.wikia.com/wiki/Count_number_of_matches_of_a_pattern
## replace
- http://vim.wikia.com/wiki/Search_and_replace

## insert
- http://vim.wikia.com/wiki/Inserting_text_in_multiple_lines
:read foo.txt
- http://vim.wikia.com/wiki/Insert_a_file

## jump
gd or gD
- https://stackoverflow.com/questions/635770/jump-to-function-definition-in-vim
- http://vim.wikia.com/wiki/Using_tab_pages

- to the file gf C+w gf C+w f
- the definition of the function [+C+i [+C+d
- to the body of the function [[ ]] [] ][ {}
- jump to the last position '' or ``
- https://stackoverflow.com/questions/5052079/vim-move-cursor-to-its-last-position

## scroll
zz - move current line to the middle of the screen
zt - move to the top
zb - move to the bottom

C-y - move the screen up one line
C-e - move the screen down one line
C-u - move the cursor & screen up 1/2 page
C-d - move the cursor & screen down 1/2 page
C-b - move screen up one page, cursor to last line
C-f - move screen down one page, cursor to first line. This may conflict or collide with Find.
- https://stackoverflow.com/questions/3458689/how-to-move-screen-without-moving-cursor-in-vim

## move
h
j
k
l
w
b
e
W
B
E
_
g_
H move to the top of screen
M move to the middle of screen
L move to the bottom of screen
C-o jump to the last cursor position
C-i jump to the next cursor position (after C-o)
* next whole word under cursor
# previous whole word under cursor
g* next matching search (not whole word) pattern under cursor
g# previous matching search (not whole word) pattern under cursor
gd go to definition/first occurence of the word under cursor

% jump to matching brachet { } [ ] ( )
fX to next 'X' after cursor, in the same line (X is any character)
FX to previous 'X' before cursor (f and F put the cursor on X)
tX til next 'X' (similar to above, but cursor is before X)
TX til previous 'X'
; repeat above, in same direction
, repeat above, in reverse direction
- http://vim.wikia.com/wiki/All_the_right_moves
- https://stackoverflow.com/questions/9501529/how-can-i-move-my-cursor-to-the-bottom-line-displayed-in-the-current-window

## quick switch between tabs
gt goto next tab
gT goto previous tab
{i} gt goto tab in position i
:tabs list all tabs
:tabm 0 move current tab to first
:tabm move current tab to last
:tabm {i} move current tab to position i + 1
:tabn go to the next tab
:tabp go to the previous tab
Ctrl + PgDn go to next tab
Ctrl + PgUp go to previous tab

- http://vim.wikia.com/wiki/Using_tab_pages
- https://superuser.com/questions/410982/in-vim-how-can-i-quickly-switch-between-tabs

## 删除空行
:g/^$/d	空行
:g/^\s*$/d 全是空格的行*
:g/^\s*#/d 以#或空格# 或TAB#开头的行*
- http://www.cnblogs.com/carbon3/p/5915282.html


## vimdiff
:bn 下一个文件
:bp 上一个文件
C - w h/j/k/l 或者方向键
C - ww 下一个窗口
dp put 当前文件复制到另一个文件里
do get 另一个文件的内容复制到当前文件
zo open 打开折叠
zc close 收起折叠
]c 下一个diff
[c 上一个diff

- http://www.cnblogs.com/xuxm2007/archive/2010/10/22/1858139.html


# buffer
:vert sb N 在当前buffer旁边打开第N号buffer
- https://stackoverflow.com/questions/4571494/open-a-buffer-as-a-vertical-split-in-vim

## tab and buffer
:tab sball
:bufdo tabsplit
:tabnew | b N
- https://stackoverflow.com/questions/5481028/vim-open-each-buffer-in-a-new-tab

# cursor
## open file under cursor
- go to file 
gf open in the same window
<c-w>f in a new window
<c-w>gf in a new tab.
- http://vim.wikia.com/wiki/Open_file_under_cursor 
## return from 'gf' in vim
Ctrl-O
Ctrl-6
Ctrl-^
:e#
- https://stackoverflow.com/questions/133626/how-do-you-return-from-gf-in-vim


# Visual mode
## Visual selection
C + v
use hjkl to select

## Visual insertion
S + i
insert something
ESC ESC

## Visual deletion and insertion
c # delete the text and stay in insert mode
insert something
ESC ESC
- https://vi.stackexchange.com/questions/13267/deleting-and-inserting-in-a-single-visual-block-selection

# Highlight the current line and column
:set cursorcolumn
:set cursorline
:highlight CursorLine cterm=NONE
:highlight CursorLine ctermbg=LightBlue

- http://vim.wikia.com/wiki/Highlight_current_line
- http://firefoxmmx.is-programmer.com/posts/37819.html
- https://vi.stackexchange.com/questions/2673/how-can-i-change-the-colour-of-the-line-highlighted-with-the-cursorline-option
- https://stackoverflow.com/questions/8640276/how-do-i-change-my-vim-highlight-line-to-not-be-an-underline

# unset a variable
:set nohighlight
:set listchars&
- https://superuser.com/questions/311992/how-do-i-unset-a-specific-vim-command/311998

:unlet s:count
- https://stackoverflow.com/questions/5755850/how-do-i-unset-a-variable-in-vim

# color not recognized
:help cterm-colors
- https://stackoverflow.com/questions/15633125/e421-getting-color-name-not-recognized-in-perfectly-valid-statement

# backspace not always work
:set backspace=indent,eol,start
- https://stackoverflow.com/questions/11560201/backspace-key-not-working-in-vim-vi

# input an unicode char
in insert mode
C + V u nnnn
for example C+Vu23ac
- http://vim.wikia.com/wiki/Entering_special_characters
- https://stackoverflow.com/questions/9119649/vim-enter-unicode-characters-with-8-digit-hex-code

# display tab or trailing whitespace
:set listchars=eol:⏎,tab:␉·,trail:␠,nbsp:⎵
:set list
- https://vi.stackexchange.com/questions/422/displaying-tabs-as-characters
- https://stackoverflow.com/questions/1675688/make-vim-show-all-white-spaces-as-a-character

# display unicode char in gvim
Edit -> Select Font -> Source Code Pro 10.
- https://superuser.com/questions/201091/getting-gvim-to-show-unicode
- https://stackoverflow.com/questions/235671/how-do-i-add-a-font-in-gvim-on-windows-system
- https://stackoverflow.com/questions/5166652/how-to-view-utf-8-characters-in-vim-or-gvim

