# Linux

# dpkg
认证中心出错，文件安装使用以下方式：
```bash
$ dpkg -i *.deb  #*/
```
# Ubuntu截图
- http://jingyan.baidu.com/article/4853e1e5756b681909f726e2.html
Ctrl + PrintScreen 截全屏
Alt + PrintScreen 截取当前窗口
Shift + PrintScreen 截图矩形

# Ubuntu下中文乱码
- http://blog.csdn.net/dc3120/article/details/44779669
1. 在/var/lib/locales/supported.d/local文件中添加zh_CN.GBK GBK
2. 执行命令 sudo dpkg-reconfigure --force locales
3. 前往wndow-preferences-general-workspace修改Text file encoding为GBK（可以手工输入）

# sudo
```bash
$ sudo -E 
The -E (preserve environment) option indicates to the security policy that the user wishes to preserve their existing environment variables. The security policy may return an error if the -E option is specified and the user does not have permission to preserve the environment.
```

# sed
## replace some pattern with current date
```bash
$ sed "s/yyyymmdd/$(date '+%Y%m%d)/g" abc.fil
```
- https://unix.stackexchange.com/questions/181283/replace-date-with-current-date

## replace the first 8 digits with current date
```bash
$ sed -i "s/^[[:digit:]]\{8\}/$(date '+%Y%m%d')/g" day_info.csv
```
## sed和grep结合使用
```bash
$ sed -i s/"str1"/"str2"/g `grep "str1" -rl --include="*.[ch]" ./`
```

## sed with piping
```bash
$ echo  "version: '2.1.0-beta.12'," | sed -En "s/version:/\1/p"
$ npm info webpack | grep version: | sed 's/version: //'
```
- https://superuser.com/questions/1107680/how-to-use-sed-with-piping

## delete every second line from a file
```bash
$ sed -n '1~2p' filename
$ sed '1~2d' filename
$ sed -n '1~2!p' filename
```
- https://unix.stackexchange.com/questions/219859/how-to-delete-every-second-line-from-a-file
- https://stackoverflow.com/questions/2560411/how-to-remove-every-other-line-with-sed

## non-greedy regex matching
```bash
$ sed 's|\(http://[^/]*/\).*|\1|g'
```

- <https://stackoverflow.com/questions/1103149/non-greedy-reluctant-regex-matching-in-sed>
- <https://0x2a.at/blog/2008/07/sed--non-greedy-matching/>

```bash
$ grep "something" perf.folded.filt | sed  "s/<[^>]*>/<>/g"
# substitute "<std::string>" like string with "<>"
```

## merge every two lines into one line
```bash
$ sed -n '{N;s/\n/\t/p}' filename
$ sed '$!N;s/\n/\t/' filename
```
- [sed行处理详解(交换行,合并行,删除行等) - 浮沉一梦 - 博客园](https://www.cnblogs.com/jjzd/p/6892891.html)
- [用sed和awk实现将文本中的上下两行合并为一行 - Just Do IT - CSDN博客](https://blog.csdn.net/abinge317/article/details/51287648)
- [sed - 简书](https://www.jianshu.com/p/ee110fc8ed69)

# ubuntu /home目录挂载到独立的分区
- http://blog.csdn.net/fuchaosz/article/details/51980627

# ubuntu路径有空格
使用'\\'进行转义，括号字符也需要转义。

# 设置时区
dpkg-reconfigure tzdata

# tar
## 解压至当前目录
tar -xzf filename.tar.gz
## 创建压缩文件，排除.debug文件
$ tar -czf file.tar.gz --exclude="*/.debug" dest_dir/  #*/
$ tar -czf file.tar.gz --exclude=dir/to/exclude/* dest_dir/ #*/

-C dirpath 切换至dirpath

# telnet
telnet IP Port

# 查看Ubuntu版本
cat /etc/issue
lsb_release -a

# apt-get 缓存位置
/var/cache/apt/archives

# Ubuntu运行程序，symbol查找错误
$ nm /with/bb/root/lib/libthosttraderapi.so | grep CreateFtdcTraderApi | c++filt
$ nm -D ~/workspace/bbsc/build/lib/libbbcorelua.so | grep SYMBOL| c++filt
- see man page _Z
# Ubuntu查看软件安装目录及安装版本
dpkg -L vim
dpkg -l vim
whereis vim

# locale::facet::_S_create_c_locale name not valid

export LC_ALL="en_US.UTF-8"

# Linux Shell命令
## 删除命令行记录
history -c

直接删除.bash_history应该不能清除历史。需要先将/etc/profile中的HISTSIZE改为0或1，然后再删除.bash_history。

可以调用'history -w'命令要求bash立即更新history文件。

## 分割字符串
cut -d \. -f 2
awk -F ' ' '{print $2}'

# 踢出已登录用户
pkill -kill -t pts/1
- http://blog.51cto.com/pizibaidu/1612428


sshpass -p password scp -P 6007 filename  user@dest_host:/dest/dir

# cat /etc/resolve.conf
search in.domain.com
nameserver 192.168.0.11
nameserver 192.168.0.1
nameserver 192.168.0.8
options ndots:2

# network-manager重写/etc/resolve.conf
```bash
sudo vim /etc/init/network-manager.conf
注释其中自动启动的代码
#start on (local-filesystems
#         and started dbus
#         and static-network-up)
```
然后停止network-manager服务
sudo stop network-manager


重启网卡
sudo ifdown eth0
sudo ifup eth0

试过一些其他的方法，都不太管用。
- http://www.cnblogs.com/lanxuezaipiao/p/3613497.html

## ifdown and ifup not working on Ubuntu18.04
```bash
$ sudo ip link set eth1 up
$ sudo ip link set eth1 down 
```
- https://www.digitalocean.com/community/tutorials/how-to-use-iproute2-tools-to-manage-network-configuration-on-a-linux-vps#how-to-configure-network-interfaces-and-addresses
- [Ubuntu 18.04 Server必须使用netplan命令配置IP地址 - 简书](https://www.jianshu.com/p/1378e6abd94d)
# ls显示完整路径
ls -R | sed "s:^:`pwd`/:"
ls -1 | awk '{print i$0}' i=`pwd`'/'

# Linux上创建和删除软连接
ln [options] existingfile newfiew
-s 建立软连接
-f 建立时，将同名文件删除

rm -rf symbolic_link
不是rm -rf symbolic_link/
- http://www.cnblogs.com/xiaochaohuashengmi/archive/2011/10/05/2199534.html

# find
```
$ find .  -name "*.log"
```
## find and remove
```bash
$ find . -name ".DS" -exec rm {} \;
```
- https://unix.stackexchange.com/questions/167823/find-exec-rm-vs-delete

## find and remove directory
```bash
$ find . -name ".debug" -exec rm -rf {} +
```
- https://unix.stackexchange.com/questions/115863/delete-files-and-directories-by-their-names-no-such-file-or-directory

## exclude a directory in find
```bash
$ find . -path ./misc -prune -o -name "*.txt" -print 
$ find . -type d \( -path dir1 -o -path dir2 -o -path dir3 \) -prune -o print
```
- https://stackoverflow.com/questions/4210042/how-to-exclude-a-directory-in-find-command?page=1&tab=votes#tab-top

## find the file in case insensitive mode
```bash
$ find . -iname "*SOMEfile*"
```
```bash
$ find . -name "filename" -exec ls -l {} +
```
## multiple pattern to find
```bash
$ find . -type f \( -name "pattern1.log" -o -name "pattern2.log" -o -name "pattern3.log" \)
$ find .  -name "pattern1.log" -o -name "pattern2.log" -o -name "pattern3.log"
$ find .  -regex '\..*[qs_nanhua_l2.log|qs_nanhua_l1.log|qs_nanhua_l1_sse.dat]$' -regextype 'grep' # note, the regex grammar is a little different from grep. And I have to say, regex for find is really sucks.
```
- https://www.cnblogs.com/jingzhishen/p/3746936.html
- [如何使用'find'命令在Linux中搜索多个文件名（扩展名）](https://www.howtoing.com/linux-find-command-to-search-multiple-filenames-extensions/)
- https://www.cnblogs.com/jiangzhaowei/p/5451173.html
- [macos - why isn't this regex working : find ./ -regex '.*\(m\\|h\)$ - Stack Overflow](https://stackoverflow.com/questions/5905493/why-isnt-this-regex-working-find-regex-m-h)
- 
# Linux 
## enable core dump file generation
ulimit -c
ulimit -c unlimited
- https://stackoverflow.com/questions/17965/how-to-generate-a-core-dump-in-linux-when-a-process-gets-a-segmentation-fault
- https://stackoverflow.com/questions/2065912/core-dumped-but-core-file-is-not-in-current-directory

## coredump on Ubuntu 18.04
$ cd /var/lib/systemd/coredump/
$ sudo lz4 core.binary_name.x.lz4
$ cat /proc/sys/kernel/core_pattern
$ vim /etc/systemd/coredump.conf
$ vim /usr/lib/sysctl.d/50-coredump.conf

## /var/log
syslog
kernlog

signal

# grep 

grep -irn pattern . --exclude=*.debug --exclude=*.a --exclude=*.o --exclude=*.so --exclude-dir=.build
grep -n #显示行号

grep -E "pattern1|pattern2" 查找符合条件之一的内容
grep -e pattern1 -e pattern2 
- http://blog.csdn.net/lkforce/article/details/52862193
- http://blog.csdn.net/quantumpo/article/details/12662889

tail -10  ThostFtdcMdApi.h | iconv -t UTF-8 -f GBK # 将文件内容转换成GBK编码
$ tail -n +NUM # output starting with line NUM

grep -m # -m num, --max-count=num Stop reading the file after num matches.

sudo update-locale LC_ALL=zh_CN.GBK LANG=zh_CN.GBK
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

## grep sort uniq
$ grep -rin onload . | awk -F ':' '{ print $1 }' | sort | uniq -c
- https://superuser.com/questions/111430/how-can-i-filter-out-unique-results-from-grep-output
$ OUTPUT somefile | grep -e 'pattern:20[0-9]\{4\}' -o
$ OUTPUT somefile | egrep -o 'pattern:20[0-9]{4}' | sort | uniq -c
- https://stackoverflow.com/questions/6951223/finding-unique-values-in-a-data-file
- https://www.linuxjournal.com/article/7396
- https://linux.cn/article-6941-1.html

## Binary file xxx matches
$ grep -a "something" binary_file

## non greedy matching

```bash
$ tr -d \\012 < price.html | grep -Po '<tr>.*?</tr>'
```

- [stackoverflow](#<https://stackoverflow.com/questions/19125173/non-greedy-matching-using-with-grep>)
- [stackexchange](#<https://unix.stackexchange.com/questions/267797/non-greedy-grep>)

```bash
$ grep "TakeAction" perf.folded.filt | grep -P "<.*?>"
```

# 终端中文乱码
bliu@sugarbush:~$ locale
LANG=zh_CN.GB18030
LANGUAGE=zh_CN.GB18030:zh_CN.GB2312:zh_CN

export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8:en_US

# Linux
- readelf

# 如何优雅的显示json文件
- python -m json.tool xxx.json
- cat xxx.json | jq .
http://stackoverflow.com/questions/352098/how-can-i-pretty-print-json

# xargs
## 指定参数的位置
$ echo prefix | xargs -I % echo % post
- https://stackoverflow.com/questions/14639206/how-can-i-pass-all-arguments-with-xargs-in-middle-of-command-in-linux
- https://www.cyberciti.biz/faq/linux-unix-bsd-xargs-construct-argument-lists-utility/

## core dump小技巧
```
# 解除core文件大小限制
$ ulimit -c unlimited
$ ulimit -a

# 如果相同目录下已经有同名的core文件，默认不会覆盖原有的文件，换句话说不会生成新的core文件
# ubuntu 14.04 默认的core文件名称就是core

# 修改core文件的名称
$ echo '/tmp/core.%e.%p.%t' | sudo tee /proc/sys/kernel/core_pattern
# 如果不带'/'，就是在当前工作路径下。
```

- https://superuser.com/questions/849099/where-does-ubuntu-14-04-drop-core-files
- http://stackoverflow.com/questions/2065912/core-dumped-but-core-file-is-not-in-current-directory
- http://man7.org/linux/man-pages/man5/core.5.html

- https://stephanfr.com/2012/10/23/debugging-gcc-in-eclipse-cdt-4-2/


# Ubuntu中文乱码问题解决
中文编码多为GBK或者GB18030，如果中文无法正常显示，多半是编码格式不对导致，可以使用export LANG=zh_CN.GBK命令来设置当前会话的编码。
对于虚拟终端，需要注意虚拟终端本身也有一个默认编码设置，一般为UTF-8，如果终端的编码已经设置为GBK，中文仍然乱码的话，去虚拟终端设置中检查一下编码格式。

```
$ locale # 查看当前环境语言配置
# 注意LANG, LANGUAGE, LC_ALL的配置

$ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
# 修改LANG和LC_ALL的默认设置，结果反映在/etc/default/locale中。也可以直接修改这个文件，需要root权限。

$ sudo locale-gen en_US.UTF-8 # 生成en_US.UTF-8的区域配置
$ sudo dpkg-reconfigure locales # 重新设置区域选项，使之生效

```
- https://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue
- http://stackoverflow.com/questions/5306153/how-to-get-terminals-character-encoding

# Putty中文乱码
主要是选一个支持中文的字体，然后确定Putty的编码和Linux显示或者文件的编码是否一致。
- https://github.com/GangZhuo/BaiduPCS/wiki/%E8%A7%A3%E5%86%B3-PuTTY-%E6%98%BE%E7%A4%BA%E4%B8%AD%E6%96%87%E4%B9%B1%E7%A0%81%E9%97%AE%E9%A2%98
# netstat
$sudo netstat -anpe #显示哪个进程占用了端口，注意要用root权限，否则进程信息可能显示不全。
- http://www.111cn.net/sys/linux/53257.htm

# Crontab 
日志写到了/var/log/syslog。可以通过grep 查看。
- https://askubuntu.com/questions/56683/where-is-the-cron-crontab-log
## ENV for crontab
$ crontab -e
SHELL=/bin/bash
MAILTO=liu
LD_LIBRARY_PATH=/usr/local/lib
PATH=$PATH:/usr/bin
:q

$ sudo vim /etc/crontab

## tunneling
- https://wiki.linuxfoundation.org/networking/tunneling
- http://www.361way.com/linux-tunnel/5199.html

## wget
加入参数：
```bash
--post-data="email=$EMAIL&password=$PASSWRD"
```
- https://stackoverflow.com/questions/8599760/variables-in-wget-post-data

## BASH
$find . -maxdepth 1 -type f
$ls -p | grep -v /
- https://stackoverflow.com/questions/10574794/bash-how-to-list-only-files

## if
```bash 
- http://ryanstutorials.net/bash-scripting-tutorial/bash-if-statements.php
- https://blog.csdn.net/qimingstack/article/details/80527094
!Expression
-n The length of STRING is greater than zero
-z The length of STRING is zero
STRING1 = STRING2 str equal
-eq numerically equal to
-gt greater than
-lt less than
-le less or equal
-ge greater or equal
-ne not equal
-d FILE exists and is a directory
-e FILE exists
-r FILE exists and the read permission is granted
-w write permission is granted
-x excute permission
-s FILE exists and it's size is greater than zero
-c charactor device
-b block device
-s size is not 0
-nt FILE1 is newer than FILE2
-ot FILE1 is older than FILE2
-a and
-o or
! not
- see man page of test
if
else
elif
&&
||
case
```
# if-elif-else-fi
```bash
if condition1
then
    # some command
elif condition2
then
    # some command
else
    # some command
fi
```

# integer comparison
```bash
if [ "$a" -lt "$b" ]
(("$a" < "$b"))
```
# string comparison
```bash
if [[ "$a" < "$b" ]]
if [ "$a" \< "$b" ]
```
- https://stackoverflow.com/questions/18668556/comparing-numbers-in-bash
- http://tldp.org/LDP/abs/html/comparison-ops.html

## declare and use boolean variables in shell script
```bash
bool=true
if [ "$bool" = true ]; then ...; fi
```
- https://stackoverflow.com/questions/2953646/how-to-declare-and-use-boolean-variables-in-shell-script

## if/else statement in one line
```bash
$ if ps aux | grep some_proces[s] > /tmp/test.txt; then echo 1; else echo 0; fi
```
- https://stackoverflow.com/questions/17203122/bash-if-else-statement-in-one-line

## while in one line
```bash
$ while true; do foo; sleep; done
```
- https://stackoverflow.com/questions/1289026/syntax-for-a-single-line-bash-infinite-while-loop

```bash
counter=0
while [ $counter -lt 10 ]
do 
    echo "$counter"
    let counter+=1
done

# redirection
while grep "1"
do
    echo "This line contains 1"
done<test.txt>result.txt
```
- https://www.cnblogs.com/liuyihua1992/p/9689295.html
 
## continue
$ for i in something
do
	[ condition ] && continue
	cmd1
	cmd2
done
- https://www.cyberciti.biz/faq/unix-linux-bsd-appleosx-continue-in-bash-loop/

## Shell Parameter Expansion
${parameter:-word} If parameter is unset or null, the expansion of word is substituted. Otherwise, the value of parameter is substituted.
${parameter#word} If the pattern matches the beginning of the expanded value of parameter, then the result of the expansion is the expanded value of parameter with the shortest matching pattern (the ‘#’ case) or the longest matching pattern (the ‘##’ case) deleted.
- https://www.gnu.org/software/bash/manual/bash.html#Shell-Parameter-Expansion
- https://segmentfault.com/a/1190000002539169
- https://stackoverflow.com/questions/41806725/what-is-the-meaning-of-1-in-bash
- https://stackoverflow.com/questions/1163145/what-does-2-1-mean-in-bash

## 添加用户及sudo权限
sudo adduser dwang
sudo usermod -aG sudo dwang

## 查看用户uid
$ id $USER
$ cat /etc/passwd

## Redirect Error Output to File
$command -arg > out.txt 2> err.txt
- https://www.cyberciti.biz/faq/linux-redirect-error-output-to-file/

# Shell
```bash
$@ or $* */all parameters passed to the function
$# the number of positional parameters passed to the function
${FUNCNAME} the names of shell functions currently in the execution call stack
```
- https://www.cyberciti.biz/faq/unix-linux-bash-function-number-of-arguments-passed/

# file exists
```bash
if ls /path/to/your/files* 1> /dev/null 2>&1; then # */
    echo "files do exist"
else
    echo "files do not exist"
fi
- https://stackoverflow.com/questions/6363441/check-if-a-file-exists-with-wildcard-in-shell-script

file="/etc/hosts"
if [ -f $file ]
then 
    echo "$file found"
else
    echo "$file not found"
fi

-d FILE file exists and is a directory
-f FILE file exists and is a regular file
-e FILE file exists
```
- https://www.cyberciti.biz/faq/unix-linux-test-existence-of-file-in-bash/

# array empty
- https://serverfault.com/questions/477503/check-if-array-is-empty-in-bash

# gtest
```bash
this works on Ubuntu 14.04
$ sudo apt-get install libgtest-dev
$ cd /usr/src/gtest
$ sudo cmake -DBUILD_SHARED_LIBS=on .
$ sudo make
$ sudo cp -a libgtest_main.so libgtest.so /usr/lib
$ sudo ldconfig -v | grep gtest
```

# timerfd and eventfd

# Change editor
$ export EDITOR=vim
$ select-editor
- https://askubuntu.com/questions/55022/changing-default-crontab-editor

# What is LD_PRELOAD

```bash
ls | grep pattern | awk '{ tmp = $1; sub(origin_string, "pattern", $1); print "\"cp " tmp  " " $1 " -v\"" }' | xargs -I cmd  sh -c "cmd"
```

# 设置terminal能够彩色显示
$ export TERM="xterm" # 重新登录后失效，需要重新执行。
或者复制一个.profile文件到target 的home目录下，重新登录即可。# 其作用是在ssh登录时，使.bashrc文件生效。

# screen 
## screen 中文乱码
在~/.screenrc中加入
defencoding GBK
encoding UTF-8 GBK

或者？
defencoding UTF-8
encoding UTF-8 UTF-8

## copy mode
```bash
C-a [ 进入copy mode
C-a [ -> 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样 
C-b Backward，PageUp 
C-f Forward，PageDown 
H(大写) High，将光标移至左上角 
L Low，将光标移至左下角 
0 移到行首 
$ 行末 
w forward one word，以字为单位往前移 
b backward one word，以字为单位往后移 
Space 第一次按为标记区起点，第二次按为终点 
Esc 结束 copy mode 
C-a ] -> paste，把刚刚在 copy mode 选定的内容贴上
```
- http://man.linuxde.net/screen

## 屏幕分割
```text
C-a S 水平分割
C-a | 垂直分割
C-a <tab> 在区块间切换
C-a X 关闭当前焦点所在的区域
C-a Q 关闭除当前区块外的其他所有区块
```
- http://man.linuxde.net/screen

# Tmux
## Panel
```text
prifix, Direction   change the current panel with the Direction key
prifix, o           Much like what the above does
prifix, z           Maximize/Restore the current panel
prifix, q + NUM     Select the NUM panel of the current screen
prefix, x           Close current pane
prefix, &           Close current window
prefix, %           split-window -h
prefix, "           split-window
prefix, :join-pane -t $windowname
```
# 向Bash数组中添加元素
Array=()
Array+=("Foo")
Array+=("Bar")
- https://stackoverflow.com/questions/1951506/add-a-new-element-to-an-array-without-specifying-the-index-in-bash

## 数组大小
$ echo ${#Array[@]}

## 引用整个数组
```bash
$ echo ${Array[@]}
$ echo ${Array[*]} # this should work also. there are some differences between the two commands. See the link below.
```
- http://www.tldp.org/LDP/abs/html/internalvariables.html#APPREF
- [Arrays](http://www.tldp.org/LDP/abs/html/arrays.html)
 
## array indices
$ echo ${!array[@]}
- https://unix.stackexchange.com/questions/193039/how-to-count-the-length-of-an-array-defined-in-bash

## assign ls output to an array
$ array=($(ls .))
- https://stackoverflow.com/questions/18884992/how-do-i-assign-ls-to-an-array-in-linux-bash

```bash
$ array=(`ls -l`)
```
[baidu](https://zhidao.baidu.com/question/758336233142073604.html)

## find output to an array
```bash
array=()
while IFS=  read -r -d $'\0'; do
    array+=("$REPLY")
done < <(find . -name ${input} -print0)
```
- https://stackoverflow.com/questions/23356779/how-can-i-store-find-command-result-as-arrays-in-bash/23357277

## sort an array
```bash
sorted=($(printf '%s\n' "${array[@]}"|sort)) # this looks simple enough
```
- https://stackoverflow.com/questions/7442417/how-to-sort-an-array-in-bash

## sort strings with numbers in Bash
```bash
$ sort -t _ -k 2 -g data.file
```
- https://stackoverflow.com/questions/17061948/sorting-strings-with-numbers-in-bash

## assign the output of a command into an array
```bash
arr=($(grep -n "search term" file.txt))
```
- https://stackoverflow.com/questions/9449417/how-do-i-assign-the-output-of-a-command-into-an-array

## Loop through an array in Bash
```bash
for i in "${arr[@]}" # element
for ((i=0; i < ${#connTypes[@]}; ++i)) # index 
```
- https://stackoverflow.com/questions/8880603/loop-through-an-array-of-strings-in-bash
- https://www.cyberciti.biz/faq/bash-for-loop-array/

## print an array
```bash
echo ${array[@]}
printf "%s\n" "${array[@]}"
```
# Nested for loop
```bash
for i in 0 1 2 3 4 5 6 7 8 9
do
    for j in 0 1 2 3 4 5 6 7 8 9
    do 
        echo "$i$j"
    done
done

for i in 0 1 2 3 4 5 6 7 8 9; do for j in 0 1 2 3 4 5 6 7 8 9; do echo "$i$j"; done; done
```
- https://stackoverflow.com/questions/4847854/bash-shell-nested-for-loop

# Iterate over a range of numbers in Bash
```bash
for i in $(seq 1 $END); do echo $i; done
```
- https://stackoverflow.com/questions/169511/how-do-i-iterate-over-a-range-of-numbers-defined-by-variables-in-bash
- https://unix.stackexchange.com/questions/55392/in-bash-is-it-possible-to-use-an-integer-variable-in-the-loop-control-of-a-for
# 在bash中查找某个字符串的子串
```bash
LIST="some string with a substring you want to match"
SOURCE="substring"
if echo "$LIST" | grep -q "$SOURCE"; then
  echo "matched";
else
  echo "no match";
fi

if [[ "$LIST" == *"$SOURCE"* ]]
```
- https://stackoverflow.com/questions/4467735/how-to-find-substring-inside-a-string-or-how-to-grep-a-variable
# Bash字符串拼接
```bash
a='hello'
b='world'
c=$a$b
echo $c

a+=b

a=2
b=4
a+=$b # a=24
((a+=$b)) # a=6
a+=($b) # a=(2, 4)
```
- https://stackoverflow.com/questions/4181703/how-to-concatenate-string-variables-in-bash

# Add numbers in a bash script
- For integers
```bash
num=$((num1 + num2))
num=$(($num1 + $num2))       # also works
num=$((num1 + 2 + 3))
- For floating point:
num=$(awk "BEGIN {print $num1+$num2; exit}")
num=$(python -c "print $num1+$num2")
num=$(perl -e "print $num1+$num2")
num=$(echo $num1 + $num2 | bc)
```
- https://stackoverflow.com/questions/6348902/how-can-i-add-numbers-in-a-bash-script
- http://tldp.org/LDP/abs/html/arithexp.html

```bash
# expr
num=`expr 4 + 5` # note the spaces between the char
num=`expr $num + 1`
echo $num
num=`expr $num - 4` # substraction
num=`expr $num \* 5` # multiplication
num=`expr $num / 6` # division
# $(())
num=$((4 + 5))
num=$((num + 1))
echo $num
num=$((num * 3))
num=$((num / 4))
num=$((num ** 5)) # power, exponentiation
# $[]
num=$[4 + 5]
num=$[num + 1]
echo $num  # the left operations are the same as $(())
# let
n=9
let n=n+1
let n=3*3
let n=3**2
let n=6/
let n=4%5
```
- https://blog.csdn.net/abcd1f2/article/details/51773660

# top 执行一次
$ top -n 1

# ssh 
## TERM environment variable not set.
$ ssh -t *

## ssh 指定端口
ssh -p 6007 user@dest_host

# 防止SSH连接超时
ssh -o ServerAliveInterval=60 -p 6007 user/password@dest_host

也可以修改client端的etc/ssh/ssh_config添加以下
ServerAliveInterval 60  #client每隔60秒发送一次请求给server，然后server响应，从而保持连接
ServerAliveCountMax 3  #client发出请求后，服务器端没有响应得次数达到3，就自动断开连接，正常情况下，server不会不响应

## use awk in ssh
$ ssh myServer "uname -a | awk '{print \$2}'"
- https://stackoverflow.com/questions/14707307/how-to-use-bash-awk-in-single-ssh-command

## output to an variable
result=$(ssh host time "command" 2>&1)
- https://stackoverflow.com/questions/12048906/capturing-ssh-output-as-variable-in-bash-script

## copy id_rsa.pub to other server
$ ssh-copy-id user@host
- login without password.

## multiple files
$ scp user@host:/path/to/dir/{file1,file2,file3} .

## some useful ssh config tricks
- https://blog.csdn.net/chenqijing2/article/details/79098703

# rsync
## specify a different ssh port when using rsync
$ rsync -rvz -e 'ssh -p 2222' --progress --remove-sent-files ./dir user@host:/path
- https://stackoverflow.com/questions/4549945/is-it-possible-to-specify-a-different-ssh-port-when-using-rsync

## daily use
$ rsync -avz -e 'ssh -p 60296' user@dest_host:/local/dist/bak .
- http://blog.csdn.net/daniel_ustc/article/details/18005925
--progress
--exclude 

-a can deal with symbol link.

## missing trailing-
The issue sucks. Mainly it becauses quotes are parsed before variables are replaced. To save time, just type the parameter as it is in the command line, and don't keep them in a string variable.

- [ssh - rsync complaining about `missing trailing-"` in a Bash script - Super User](https://superuser.com/questions/354361/rsync-complaining-about-missing-trailing-in-a-bash-script)
- [BashFAQ/050 - Greg's Wiki](http://mywiki.wooledge.org/BashFAQ/050)
- [rsync在Bash脚本中抱怨“缺少尾随 - ” - 智库101 - 一个基于CC版权的问答分享平台](http://www.kbase101.com/question/42947.html)

## Warning: the RSA host key for 'shuke' differs from the key for the IP address '192.168.0.25'
$ ssh-keygen -R shuke
- https://serverfault.com/questions/633109/how-to-properly-remove-an-old-ssh-key/633115

## change_dir: no such ile or directory
```bash
$ mkdir -p `dirname filename` && rsync ...
```
[segmentfault](https://segmentfault.com/q/1010000000324927)

# DNS
## Ubuntu
- http://www.cnblogs.com/yudar/p/4723992.html
- http://os.51cto.com/art/201002/184346.htm

# 重定向
program &>> file or &> file 将program的stdout和stderr重定向到file
等价于 program >> file 2>&1
- https://unix.stackexchange.com/questions/170572/what-is-in-a-shell-script

# Bash
## Special Parameters
```
$* Expands to the positional parameters, starting from one.
$@ Expands to the positional parameters, starting from one.
$# Expands to the number of positional parameters in decimal.
$? Expands to the exit status of the most recently executed foreground pipeline.
$- Expands to the current option flags as specified upon invocation, by the set builtin command, or those set by the shell itself (such as the -i option).
$$ Expands to the process ID of the shell.
$! Expands to the process ID of the job most recently placed into the background, whether executed as an asynchronous command or using the bg builtin 
$0 Expands to the name of the shell or shell script.
$_ At shell startup, set to the absolute pathname used to invoke the shell or shell script being executed as passed in the environment or argument list.
```
- https://www.gnu.org/software/bash/manual/bash.html#Special-Parameters

## Disable sleep on laptop lid close in tty1
Try to edit the /etc/systemd/logind.conf file and modify the line:

"#HandleLidSwitch=suspend"

to

HandleLidSwitch=ignore

Then reboot.
- https://askubuntu.com/questions/741271/disable-sleep-on-laptop-lid-close-in-tty1

## Show or hide boot messages when Ubuntu starts
$ sudo vim /etc/default/grub
The presence of the word splash in this entry enables the splash screen, with condensed text output. Adding quiet as well, results in just the splash screen; which is the default for the desktop edition since 10.04 (Lucid Lynx). In order to enable the "normal" text start up, you would remove both of these.

So, the default for the desktop, (i.e. splash screen only):

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" #Hide text and show splash
For the traditional, text display:

GRUB_CMDLINE_LINUX_DEFAULT=        #Show text but not the splash
For the splash, but the ability to show the boot messages by pressing Esc:

GRUB_CMDLINE_LINUX_DEFAULT="splash"
Or, finally, for just a (usually) black screen, try:

GRUB_CMDLINE_LINUX_DEFAULT=quiet   #Don't show Ubuntu bootup text
GRUB_CMDLINE_LINUX="console=tty12" #Don't show kernel text
After editing the file, you need to run update-grub.

$ sudo update-grub

- https://askubuntu.com/questions/248/how-can-i-show-or-hide-boot-messages-when-ubuntu-starts

# Enable/Disable Ubuntu Desktop environment
1. Method 1:
You can use an override:

sudo echo "manual" >> /etc/init/lightdm.override
To start lightdm on command:

sudo start lightdm
To restore your system so that lightdm is always started on boot:

sudo rm /etc/init/lightdm.override
For more information, the upstart cookbook is your friend:

http://upstart.ubuntu.com/cookbook/
- https://askubuntu.com/questions/402083/enable-disable-ubuntu-desktop-environment-on-ubuntu-12-04-1-server-i386

2. Method 2:
Try to disable GUI, just press Ctrl+Alt+T on your keyboard to open Terminal. When it opens, run the command(s) below:

sudo update-rc.d -f gdm remove
When you restart your computer, you’ll get text-mode login. To run GUI again:

startx
To enable GUI again:

sudo update-rc.d -f gdm defaults
- https://askubuntu.com/questions/402083/enable-disable-ubuntu-desktop-environment-on-ubuntu-12-04-1-server-i386

3. Method 3:
From the tested comments discussion this is done use systemd disable argument:

$ systemctl disable lightdm.service
- https://askubuntu.com/questions/823479/how-to-remove-gui-on-ubuntu-server-16-04

# Limit CPU usage of a process
$ cpulimit -l 30 -p $!
$ nice -n 12 ./example
- https://www.techforgeek.info/how_to_limit_cpu_usage.html

$ sudo cgcreate -g cpu:/cpulimited
$ sudo cgset -r cpu.shares=512 cpulimited
$ sudo cgexec -g cpu:cpulimited /usr/local/bin/matho-primes 0 9999999999 > /dev/null &
- http://blog.scoutapp.com/articles/2014/11/04/restricting-process-cpu-usage-using-nice-cpulimit-and-cgroups

$ sudo vim /etc/security/limits.conf
- https://www.tecmint.com/set-limits-on-user-processes-using-ulimit-in-linux/

# cgroup
- https://www.kernel.org/doc/Documentation/cgroup-v2.txt
- https://www.kernel.org/doc/Documentation/cgroup-v1/
- https://coolshell.cn/articles/17049.html

## cpuset
$ echo pid > tasks
# write error: No space left on device
we should allocate cpus and mems for the cpuset first.
$ echo 0 > cpuset.mems
- https://blog.csdn.net/xftony/article/details/80536562
- https://stackoverflow.com/questions/28348627/echo-tasks-gives-no-space-left-on-device-when-trying-to-use-cpuset
- https://serverfault.com/questions/579555/cgroup-no-space-left-on-device
# Extract a deb file
$ ar vx nginx_1.10.0-0ubuntu0.16.04.4_all.deb
$ dpkg-deb -xv {file.deb} {/path/to/where/extract}
$ dpkg -c {file.deb}
$ apt-file list {packageName}
- https://www.cyberciti.biz/faq/how-to-extract-a-deb-file-without-opening-it-on-debian-or-ubuntu-linux/

# Get the last modified date in bash
$ stat -c %y "$entry"
$ date -r <filename>
- https://stackoverflow.com/questions/16391208/print-a-files-last-modified-date-in-bash

# Perform timestamp comparison
$ touch -d '-1 hour' limit
$ if [ limit -nt last_modification ]; then
touch last_modification
fi
or 
$ if test limit -nt last_modification; then
...
fi
- https://stackoverflow.com/questions/205666/what-is-the-best-way-to-perform-timestamp-comparison-in-bash

# Sticky Bit
Sticky Bit is mainly used on folders in order to avoid deletion of a folder and it’s content by other users though they having write permissions on the folder contents. If Sticky bit is enabled on a folder, the folder contents are deleted by only owner who created them and the root user. No one else can delete other users data in this folder(Where sticky bit is set). 

$ chmod o+t /opt/dump
$ chmod +t /opt/dump 
$ chmod 1757 /opt/dump
- https://www.linuxnix.com/sticky-bit-set-linux/

# SGID
SGID (Set Group ID up on execution) is a special type of file permissions given to a file/folder. Normally in Linux/Unix when a program runs, it inherit’s access permissions from the logged in user. SGID is defined as giving temporary permissions to a user to run a program/file with the permissions of the file group permissions to become member of that group to execute the file. In simple words users will get file Group’s permissions when executing a Folder/file/program/command.

$ chmod g+s file1.txt
$ chmod 2750 file1.txt
- https://www.linuxnix.com/sgid-set-sgid-linuxunix/

# SUID
SUID (Set owner User ID up on execution) is a special type of file permissions given to a file. Normally in Linux/Unix when a program runs, it inherit’s access permissions from the logged in user. SUID is defined as giving temporary permissions to a user to run a program/file with the permissions of the file owner rather that the user who runs it. In simple words users will get file owner’s permissions as well as owner UID and GID when executing a file/program/command.

$ chmod u+s file1.txt
$ chmod 4750 file1.txt
- https://www.linuxnix.com/suid-set-suid-linuxunix/

# make
## use LDFLAGS in Makefile
CFLAGS=-Wall
LDFLAGS=-lm # LDLIBS=-lm also works

client: $(OBJECTS)
    $(CC) $(CFLAGS) $(OBJECTS) -o client $(LDFLAGS)

- https://stackoverflow.com/questions/13249610/how-to-use-ldflags-in-makefile

## Passing additional variables from command line to make
- From environment
export FOO=bar
make

- From command line
make FOO=bar target
make target FOO=bar

- Exporting from the parent make
export CFLAGS # from parent Makefile
- https://stackoverflow.com/questions/2826029/passing-additional-variables-from-command-line-to-make

## spcify the installation directory
$ make DESTDIR=/install/directory install
- https://blog.csdn.net/ghost_rt/article/details/53678366

## use absolute path in command of Makefile
```bash
$(abspath wanted_path)
$(realpath wanted_path)
$(realpath $(path_variable))
```
- [makefile将相对路径转换为绝对路径 - 左手's Blog - 我是左手，行走在路上!](http://zshou.is-programmer.com/posts/36212.html)
- GNU Make Manual
# sleep millisecond in bash
$ sleep 0.01
Unlike most implementations that require NUMBER be an
       integer, here NUMBER may be an arbitrary floating point number
- https://serverfault.com/questions/469247/how-do-i-sleep-for-a-millisecond-in-bash-or-ksh
- man 1 sleep

# Bind failed: Address already in use
```C++
int enable = 1;
if (setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, &enable, sizeof(int)) < 0)
    error("setsockopt(SO_REUSEADDR) failed");
```
- https://stackoverflow.com/questions/24194961/how-do-i-use-setsockoptso-reuseaddr
- https://stackoverflow.com/questions/15198834/bind-failed-address-already-in-use/15199016

# Query and set MTU
```bash
$ cat /sys/class/net/eth0/mtu
$ echo "1300" > /sys/class/net/eth0/mtu # need to be root.
$ ifconfig echo0 mtu 1300 up
```
- http://blog.csdn.net/codejoker/article/details/5997447
- http://www.361way.com/linux-mtu-jumbo-frames/4055.html
- https://www.cyberciti.biz/faq/centos-rhel-redhat-fedora-debian-linux-mtu-size/

# Install Wireshark
```bash
Step 1 : Add the official PPA, go to terminal by pressing ctrl+alt+t

sudo add-apt-repository ppa:wireshark-dev/stable
Step 2 : update the repository

sudo apt-get update
Step 3 : install wireshark 2.0

sudo apt-get install wireshark
Step 4 : run wireshark

sudo wireshark
If you get a error couldn't run /usr/bin/dumpcap in child process: Permission Denied. go to the terminal again and

sudo dpkg-reconfigure wireshark-common
Say YES to the message box.This adds a wireshark group.Then add user to the group by typing

sudo adduser $USER wireshark
```
- https://askubuntu.com/questions/700712/how-to-install-wireshark

# delete an exported environment variable
$ unset TESTVAR
- https://stackoverflow.com/questions/6877727/how-do-i-delete-an-exported-environment-variable
- https://unix.stackexchange.com/questions/103555/how-do-i-unset-a-variable-at-the-command-line

$ env -u FOO somecommand
- https://stackoverflow.com/questions/18906350/unset-an-environment-variable-for-a-single-command

# cmake Modules directory not found in
$ sudo apt remove cmake cmake-data # then install cmake package or install from source
- http://blog.csdn.net/lingdi2000/article/details/51866222
- https://stackoverflow.com/questions/18615451/cmake-missing-modules-directory

# LD_PRELOAD

- http://0cx.cc/linux_ld_preload.jspx

# dlsym
- http://blog.csdn.net/bytxl/article/details/48524531

# __init symbol in kernel
- https://stackoverflow.com/questions/8832114/what-does-init-mean-in-the-linux-kernel-code

# socat
```bash
SRC=239.1.2.3; SRCP=1234; IF=eth0
$ socat -T 3 UDP4-RECV:$SRCP,bind=$SRC,ip-add-membership=$SRC:$IF,reuseaddr -
$ socat UDP4-RECVFROM:6666,ip-add-membership=224.1.0.1:192.168.10.2,fork EXEC:hostname
socat STDIO UDP4-DATAGRAM:224.1.0.1:6666,range=192.168.0.0/24

socat UDP4-RECVFROM:6666,ip-add-membership=224.1.0.1:192.168.0.19,fork -
socat - UDP4-DATAGRAM:224.1.0.1:6666 < Makefile
# nc
$ nc -c -w 1 -v -u -s 127.0.0.1 239.1.2.3 1234 < test.txt
nc.traditional -c -w 1 -v -u -s 127.0.0.1 239.1.2.3 1234 < test.txt
```
- http://www.dest-unreach.org/socat/doc/socat-multicast.html
- https://blog.danman.eu/using-socat-for-multicast-receiving-and-proxying/
- https://serverfault.com/questions/814259/use-ip-route-add-to-add-multicast-routes-to-multiple-interfaces
- https://serverfault.com/questions/359536/what-does-this-linux-command-mean-route-add-net-224-0-0-0-netmask-240-0-0-0-et/359539
- https://stackoverflow.com/questions/38907179/multicast-route-to-all-interfaces
- https://unix.stackexchange.com/questions/111539/cat-file-to-multicast

# Kernel
## 2.4内核启动顺序
- https://bg2bkk.github.io/post/tcp%20ip%E5%8D%8F%E8%AE%AE%E6%A0%88%E5%9C%A8linux%E5%86%85%E6%A0%B8%E5%90%AF%E5%8A%A8%E4%B8%AD%E7%9A%84%E9%A1%BA%E5%BA%8F/

## build
$ make defconfig
$ make menuconfig
$ cp /boot/your-distribution-config .config
$ make localconfig

## WRITE_ONCE
- https://stackoverflow.com/questions/34988277/write-once-in-linux-kernel-lists
- http://quant67.com/post/linux/access_once.html

# Glibc
## source file of socket
sysdeps/unix/sysv/linux/**/socket.S or ports/sysdeps/unix/sysv/linux/*/socket.S  #/**/
or socket.c maybe.
- https://stackoverflow.com/questions/14882289/i-was-perusing-glibc-when-i-came-across-the-socket-code-can-someone-explain-wha

# input sudo password in bash script
```bash
$ echo "password" | sudo -S cmd
```
# change hostname without reboot
```bash
$ sudo hostname [new_host_name]
$ sudo vim /etc/hostname
$ sudo vim /etc/hosts
127.0.0.1 [new_host_name]
```
- https://askubuntu.com/questions/87665/how-do-i-change-the-hostname-without-a-restart
- https://sonnguyen.ws/change-hostname-without-reboot-ubuntu-14-04/

# egrep
```
[:alnum:] - Alphanumeric characters
[:alpha:] - Alphabetic characters
[:blank:] - Blank characters: space and tab
[:digit:] - Digits
[:lower:] - Lower-case letters
[:space:] - Space characters: tab, newline, vertical tab, form feed, carriage return and space
[:upper:] - Upper-case letters
. Matches any single character
? The preceding item is optional and will be matched, at most, once.
* matched zero or more times.
+ matched one or more times.
{N} matched exactly N times.
{N, } matched N or more times.
{N, M} matched at least N times, but not more than M times.
- Represents the range if it's not the first or last in a list or the ending point of a range in a list.
^ Matches the empty string at the beginning of a line; also represents the characters not in the range of a list.
$ Matches the empty string at the end of a line.
\b Matches the empty string at the edge of a word.
\B Matches the empty string provided it's not at the edge of a word.
\< Match the empty string at the beginning of word
\> Match the empty string at the end of word.
```
- https://www.cyberciti.biz/faq/grep-regular-expressions/

## grep lines not starting with '#'
```bash
$ egrep "^[^#]" 
$ grep -E "^[^#]
$ grep "^[^#]"
```
- https://unix.stackexchange.com/questions/60994/how-to-grep-lines-which-does-not-begin-with-or

## grep lines starting with spaces or "--"
```
$ grep -rn -E "^[[:space:]]*[-]{2}.*$" . 
```
## grep recursively through .gz files
```
$ zgrep 'pattern' -r --format=gz /path/to/dir
$ find /path/to/dir -name "*.gz" -exec zgrep -- 'pattern' {} +  #*/
```
- https://unix.stackexchange.com/questions/187742/how-do-i-grep-recursively-through-gz-files

## grep only certain file extensions
```
$ grep -r -i --include \*.h --include \*.cpp CP_Image ~/path[12345]
- https://stackoverflow.com/questions/12516937/grep-but-only-certain-file-extensions

$ grep -Rn "Error" --exclude-dir=builds --exclude-dir=dist --exclude-dir=services --exclude-dir=test_dir --exclude-dir=archive --include="*`date +%Y%m%d`.log" . | awk -F '/' '{ print $2 }' | sort | uniq -c #*/
```
## grep follows symlinks
$ grep -R blabla /path/to/dir
- https://stackoverflow.com/questions/21738574/how-do-you-exclude-symlinks-in-a-grep

## Show surrounding lines of search result
$ grep -A 3 -B 3 foo READEME.txt
$ grep -C 3 foo README.txt
- https://stackoverflow.com/questions/9081/grep-a-file-but-show-several-surrounding-lines
- https://stackoverflow.com/questions/1072643/how-can-i-make-grep-print-the-lines-below-and-above-each-matching-line

# expect
- http://xstarcd.github.io/wiki/shell/expect.html
- https://www.cnblogs.com/arlenhou/p/learn_expect.html
- https://blog.csdn.net/catoop/article/details/48289991
- https://blog.csdn.net/wn_hello/article/details/50521956

## send Ctrl + a in expect
send "\x01"; # \x01 is the value of Ctrl-A
- https://stackoverflow.com/questions/27050473/how-to-send-ctrl-a-then-d-in-expect

## escape character of telnet
send "\x0b" or send ""
remember not in this form: send "\x0b\r"
in this case, telnet will escape to command mode first, "\r" will turn telnet into transit mode again. This cost me much time. 
You can only quit the telnet when you input "quit" in command mode.
- http://www.physics.udel.edu/~watson/scen103/ascii.html # ASCII Table

## tcl string concatation
set result "The result is "
append result "Earth 2, Mars 0"

set combined $a$b

set combined "$a${b}c d"

- https://stackoverflow.com/questions/5908496/tcl-string-concat

## get the current timestamp
send_user [timestamp -format "%Y%m%d %X "]
- http://bbs.bugcode.cn/t/138917

## get the current date by invoking shell
set date [exec date "+%Y-%m-%d"]
send_user $date
- http://blog.chinaunix.net/uid-17282739-id-3144257.html

# inotify don't treat vim editing as a modification event
Vim is a program that does not directly save to the file while you save the file. It creates a temporary file usually named .filename.swp and only when you close vim this file gets renamed to filename.
From the inotify's FAQ:
The IN_MODIFY event is emitted on a file content change (e.g. via the write() syscall) while IN_CLOSE_WRITE occurs on closing the changed file. It means each change operation causes one IN_MODIFY event (it may occur many times during manipulations with an open file) whereas IN_CLOSE_WRITE is emitted only once (on closing the file).
- https://stackoverflow.com/questions/13312794/inotify-dont-treat-vim-editting-as-a-modification-event

# Clear cache for specific domain name in chrome
F12 > Chrome Developer Tools > Application tab > Clear storage in left tree > Select the data items to clear, then click Clear site data
- https://superuser.com/questions/278948/clear-cache-for-specific-domain-name-in-chrome

# generate a random string
$ pwgen -s 13 7 # generate 7 passwords of length 13.
$ head /dev/urandom | tr -dc A-Za-z0-9 | head -c 13; echo ''
$ openssl rand -base64 12
- https://unix.stackexchange.com/questions/230673/how-to-generate-a-random-string

# Copy-Paste in terminal adds 0~ and 1~
```bash
$ printf "\e[?2004l"
$ printf "\e[?2004l"

set t_BE= to .vimrc
```
- https://unix.stackexchange.com/questions/196098/copy-paste-in-xfce4-terminal-adds-0-and-1

# English punctuation
```c
. period or full stop
, comma
: colon
; simicolon
! exclamation mark
? question mark
- hyphen
* asterisk
' apostrophe
-- dash
_ underscore
'' single quotation marks
"" double quotation marks
() parenthesis or round parenthesis
[] square brackets
<> angle brackets
{} curly brackets or braces
... ellipsis
" tandom colon
|| parallel
/ slash or virgule or diagonal mark
& ampersand
~ tilde or swung dash
-> arrow
\ back slash
```
- http://www.ruanyifeng.com/blog/2007/07/english_punctuation.html

^ caret
$ dollar sign
% percent sign
# hash mark or pound sign
| pipe

- http://www.sussex.ac.uk/informatics/punctuation/misc/other

# Bash write integer to binary file
```bash
$ int=65534
$ printf "0: %.8x" $int | xxd -r -g0 >> file # this should not work on a little endian cpu, such as X86
$ printf "0: %.8x" $int | sed -E 's/0: (..)(..)(..)(..)/0: \4\3\2\1/' | xxd -r -g0 >> file # this should work on X86
$ cat file | od -tu4 # display the file as a 4-bytes unsigned int.
$ xxd -b file # view the file in binary
```
- https://stackoverflow.com/questions/9955020/bash-write-integer-to-binary-file
- https://stackoverflow.com/questions/27382480/get-numerical-value-of-integer-in-binary-file-header
- https://stackoverflow.com/questions/1765311/how-to-view-files-in-binary-in-the-terminal?rq=1

# Using “${a:-b}” for variable assignment in scripts
if $a is unset or null, set it to b.
- https://unix.stackexchange.com/questions/122845/using-a-b-for-variable-assignment-in-scripts

# make
## addsuffix to some string
$(addsuffix <suffix>,<names...>) 

## filter-out
$(filter-out <pattern...>,<text>)
- http://wiki.ubuntu.org.cn/%E8%B7%9F%E6%88%91%E4%B8%80%E8%B5%B7%E5%86%99Makefile:%E4%BD%BF%E7%94%A8%E5%87%BD%E6%95%B0#.E6.96.87.E4.BB.B6.E5.90.8D.E6.93.8D.E4.BD.9C.E5.87.BD.E6.95.B0

# unrar
$ unrar x filename # extract file from a rar file
$ unrar l filename # list file inside a rar file

# dd
## copy to the last 512 bytes of the output
$ dd if=/dev/zero of=file bs=512 seek=$(( $(blockdev --getsz Win10.vhd) - 1)) count=1
- https://superuser.com/questions/128860/which-way-is-the-fastest-to-dd-to-the-last-512-kilobytes-of-disk

## copy the last 512 bytes to the output
$ dd if=Win10.vhd of=file bs=1 count=512 skip=$(( $(stat -c %s Win10.vhd) - 512 ))
- https://unix.stackexchange.com/questions/32941/use-dd-to-cut-file-end-part

# Hexdump
$ hexdump -v -C file # Output all the contents without folding, with Text stand aside.

# Basic tools for productivity
- vim
- meld
- git
- Eclipse
- XMind
- expect
- pip
- jupyter
- tree
- tmux
- fzf
- thefuck
- tldr

# Excellent Vim Plugins
- ctrlp
- fzf
- vimawesome.com


# Get the core id that the specified process/thread running
- taskset -c -p <pid>
- ps -o pid,psr,comm -p <pid>
- top # press 'F', then select "Last used CPU" using down arrow, press 'd',then press 'q'
- htop # press F2, select Columns->Available Columns->PROCESSOR, then press F10 to save the config.
- https://blog.csdn.net/ibless/article/details/82431101

# set the cpu core for specified process
$ taskset -cp cpu-list pid

# Grub
## Modify the args of kernel
```bash
$ sudo cp /boot/grub/grub.cfg /boo/grub/grub.cfg.bak
$ sudo vim /boot/grub/grub.cfg
change root=xxx to root=xxx isolcpus=4-7
save the change.
$ sudo reboot
One thing to remember, the change will be overwritten after then update-grub or grub-install
```

# View multithread of process
$ pstree
$ ps -eLf
$ ps -eo ruser,pid,ppid,lwp,psr,args -L
$ top -H
- http://smilejay.com/2012/06/linux_view_threads/

# What is a memory fence?
For performance gains modern CPUs often execute instructions out of order to make maximum use of the available silicon (including memory read/writes). Because the hardware enforces instructions integrity you never notice this in a single thread of execution. However for multiple threads or environments with volatile memory (memory mapped I/O for example) this can lead to unpredictable behavior.

A memory fence/barrier is a class of instructions that mean memory read/writes occur in the order you expect. For example a 'full fence' means all read/writes before the fence are comitted before those after the fence.

Note memory fences are a hardware concept. In higher level languages we are used to dealing with mutexes and semaphores - these may well be implemented using memory fences at the low level and explicit use of memory barriers are not necessary. Use of memory barriers requires a careful study of the hardware architecture and more commonly found in device drivers than application code.

The CPU reordering is different from compiler optimisations - although the artefacts can be similar. You need to take separate measures to stop the compiler reordering your instructions if that may cause undesirable behaviour (e.g. use of the volatile keyword in C).
- https://stackoverflow.com/questions/286629/what-is-a-memory-fence

# Generate SSL key
not working for gitea
- [使用openssl 生成免费证书 - 龙恩0707 - 博客园](https://www.cnblogs.com/tugenhua0707/p/10927722.html)
not tried
- [linux命令安装ssl证书 - fresh123456的专栏 - CSDN博客](https://blog.csdn.net/fresh123456/article/details/45368969)

# Powerline
- [Installation on Linux — Powerline beta documentation](https://powerline.readthedocs.io/en/master/installation/linux.html)
- [Installation — Powerline beta documentation](https://powerline.readthedocs.io/en/master/installation.html#installation-patched-fonts)
- [GitHub - powerline/fonts: Patched fonts for Powerline users.](https://github.com/powerline/fonts)
- [linux配置powerline(bash/vim)美化 - weixin_33913332的博客 - CSDN博客](https://blog.csdn.net/weixin_33913332/article/details/94564327)
- [桌面应用\|Powerline：Vim 和 Bash 中的一个强大状态栏插件](https://linux.cn/article-8651-1.html)

# Color of the output
- [Linux:shell脚本字符显示特殊颜色效果 - Spiro-k - 博客园](https://www.cnblogs.com/Spiro-K/p/6592518.html)

# Get the memory information
```bash
$ cat /proc/meminfo

$ sudo dmidecode -t memory
$ sudo dmidecode | grep -A 16 "Memory Device"  # not so accurate as the above
```

# include other files into the script
```bash
$ source FILE
$ . FILE
```
- [linux - How to include file in a bash shell script - Stack Overflow](https://stackoverflow.com/questions/10823635/how-to-include-file-in-a-bash-shell-script/10823650)

# Tree
```bash
$ tree -P "201907[0-9][0-9]|2019062[0-9]" --matchdirs -s -o /devdata/Citic/Femas/dst.tree -n
```

# Get the size of the terminal
```bash
$ tput cols
$ tput lines
$ tput cup 10 1000 # set the cursor to (10, 100)
$ stty size 
$ echo ${COLUMN:-1}
```
- [shell之获取终端信息 - mrwuzs - 博客园](https://www.cnblogs.com/mrwuzs/p/9994327.html)

# Get a random from the Shell
```bash
$ echo $RANDOM
$ awk 'BEGIN { srand(); print rand() * 1000000 }'
$ head -20 /dev/urandom | cksum | cut -f1 -d " "
$ head -n 20 /proc/sys/kennel/random/uuid | cksum | cut -f1 -d " "
$ openssl rand -base64 8 | cksum | cut -c1-8

# Random String
$ openssl rand -hex 8 | md5sum | cut -c1-8
$ date +%s%N | md5sum | head -c 10
$ cat /dev/urandom | head -n 10 | md5sum | head -c 10
```
- [linux shell 生成随机数的方法 - yunxiaoxiehou的博客 - CSDN博客](https://blog.csdn.net/yunxiaoxiehou/article/details/87969930)
- [linux shell实现随机数多种方法（date,random,uuid) - 程默 - 博客园](https://www.cnblogs.com/chengmo/archive/2010/10/23/1858879.html)

# Multiple background jobs
- [Linux Shell多进程并发以及并发数控制 - 简书](https://www.jianshu.com/p/2d60e6513fdd)
- [shell——wait与多进程并发 - 沄持的学习记录 - 博客园](https://www.cnblogs.com/maxgongzuo/p/6414376.html)

# A pure terminal withou prompt
```bash
$ bash --norc --noprofile

```

# Bash
```bash
$ shopt -s extglob
$ echo ${STR: -1}
$ echo ${STR##*( )}
```

# automake
```bash
$ autoscan
$ autoconf
$ aclocal
$ autoheader
$ automake
$ libtoolize # if necessary
$ automake --add-missing # if last automake failed.
$ ./configure && make -j8
```

# NFS
```bash
$ sudo mount -t nfs 192.168.1.25:/data_disk /mnt/nfs_disk
$ fuser -m /mnt/nfs_disk
$ lsod +D /mnt/nfs_disk
$ fuser -km /mnt/nfs_disk
$ umount -l /mnt/nfs_disk
```
- [umount nfs文件系统 显示 umount.nfs: device is busy - mofy - 博客园](https://www.cnblogs.com/z-books/p/9324470.html)

# Bash read
- [](https://blog.csdn.net/guominyou/article/details/80923734)

# Mail
```bash
$ sudo apt install mailutils
# if sendmail is installed, remove it, then install postfix
$ sudo apt remove sendmail
$ sudo apt install postfix

$ mail -s "Test mail from ubuntu server" example@qq.com <<< "Here is the message body"
$ echo "Here is the message body" | mail -s "Test mail from ubuntu server" example@qq.com
$ mail -s "Test mail from ubuntu server" example@qq.com
# then input Cc field, then message body, use Ctrl + D to finish and send
``` 
- [](https://blog.csdn.net/chijiaodaxie/article/details/77893464)
- 
- [](https://blog.csdn.net/qq_34721505/article/details/79359996)
- [](https://blog.csdn.net/u012219371/article/details/84929028)
- [linux安装配置sendmail实现邮件发送 - divor - 博客园](https://www.cnblogs.com/alibai/p/4012060.html)

# Video Card Switching
$ nvidia-settings # If Prime Profiles shows here, then click on the profile and switch the card.
$ prime-select nvidia/intel 
- [Ubuntu系列切换Intel和NVIDIA显卡（我在Linux显卡上遇到的坑） - 代码奔腾 - SegmentFault 思否](https://segmentfault.com/a/1190000009269284)
- [Ubuntu系列切换Intel和NVIDIA显卡（我在Linux显卡上遇到的坑） - 简书](https://www.jianshu.com/p/a539897be45e)

# Clear the system buffer/cache
```bash
# echo 1 > /proc/sys/vm/drop_caches # sysctl -w vm.drop_caches=1 # clear pagecache
# echo 2 > /proc/sys/vm/drop_caches # sysctl -w vm.drop_caches=2 # clear dentries and inodes
# echo 3 > /proc/sys/vm/drop_caches # sysctl -w vm.drop_caches=3 # clear pagecache, dentries, and inodes
# sync
```
- [](https://blog.csdn.net/Leichelle/article/details/87693054)
- [Linux系统清除缓存 - jiu~ - 博客园](https://www.cnblogs.com/jiu0821/p/9854704.html)

# journctl
```bash
$ sudo journalctl -S `date +%Y-%m-%d` | vim "+set filetype=messages" - -R
```

# Bash don't echo right
```bash
$ reset
```
- [UNIX 常用命令_w3cschool](https://www.w3cschool.cn/unix/unix-useful-commands.html)

# Bash redirection
```bash
$ /home/test/prod/bin/admin_term localhost:11111 << EOF
print_clients
quit
EOF
```

# Ubuntu 18.04 No Sound
```bash
$ sudo apt install --reinstall alsa-base pulseaudio pavucontrol
$ sudo alsa force-reload

$ pulseaudio --system
$ pavucontrol
# 看看有没有输出设备。如果没有输出设备，或者输出设备是Dummy，需要重启电脑试试。
```
- [Ubuntu18.04声卡无声音解决方案_multimicro的博客-CSDN博客](https://blog.csdn.net/multimicro/article/details/82528730)

# Bash 初始化加载的几个文件
- /etc/profile 
- /etc/bashrc 或 /etc/bash.bashrc
- ~/.profile
- ~/.bash_login
- ~/.bash_profile
- ~/.bashrc
- ~/.bash_logout

- [bash的几个初始化文件 - 简书](https://www.jianshu.com/p/6a0047ad2d9e)
- [bash初始化过程_longxibendi的专栏-CSDN博客](https://blog.csdn.net/longxibendi/article/details/6528088)
