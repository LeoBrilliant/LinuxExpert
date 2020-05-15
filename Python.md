# Python
- 空字符串或者空格转换成float报异常
float("")
float(" ")

- dict to a string
dict1 = {'one':1, 'two':2, 'three': {'three.1': 3.1, 'three.2': 3.2 }}
str1 = str(dict1)

dict2 = eval(str1)

import json
# convert to string
input = json.dumps({'id': id })

# load to dict
my_dict = json.loads(input)
- http://stackoverflow.com/questions/4547274/convert-a-python-dict-to-a-string-and-back

# json
## Write JSON data to a file
with open('data.json', 'w') as outfile:
    json.dump(data, outfile)
- https://stackoverflow.com/questions/12309269/how-do-i-write-json-data-to-a-file

## Pretty-Printing
    json.dumps(data, indent=4)
    json.dump(data, outfile, indent=4)
- https://stackoverflow.com/questions/12943819/how-to-prettyprint-a-json-file
- https://docs.python.org/2/library/json.html

## Read JSON from a file
with open('data.json', 'r') as json_file:
    data = json.load( json_file )
- https://stackabuse.com/reading-and-writing-json-to-a-file-in-python/

## Open a file for both reading and writing
with open(filename, "r+") as f:
    data = f.read()
    f.seek( 0 )
    f.write( output )
    f.truncate
- https://stackoverflow.com/questions/6648493/how-to-open-a-file-for-both-reading-and-writing


## 如何删除string list中的string的首尾空格？
```
# datafields is a list of string.

datafields = map(str.strip, datafields)

datafields = [s.strip() for s in datafields]
```

## 时间和日期转换
- https://stackoverflow.com/questions/19801727/convert-datetime-to-unix-timestamp-and-convert-it-back-in-python
- https://stackoverflow.com/questions/466345/converting-string-into-datetime

## File
### exists
os.path.exists()
- https://stackoverflow.com/questions/82831/how-do-i-check-whether-a-file-exists-using-python
### open 
- https://docs.python.org/2/library/functions.html#open

### json
- https://docs.python.org/2/library/json.html
- http://python3-cookbook.readthedocs.io/zh_CN/latest/c06/p02_read-write_json_data.html

## Exceptions
- https://docs.python.org/2/tutorial/errors.html#raising-exceptions

## Unit test
- https://docs.python.org/2/library/unittest.html?highlight=mock
- http://pyunit.sourceforge.net/pyunit_cn.html

# List
## Find in list
item in myList
myList.index(item)
- https://stackoverflow.com/questions/9542738/python-find-in-list

## concatenate two lists in Python
mergedlist = listone + listtwo
- https://stackoverflow.com/questions/1720421/how-to-concatenate-two-lists-in-python

## del, remove, pop on lists
remove removes the first matching value, not a specific index.
del removes the item at a specific index.
pop removes the item at a specific index and returns it.
- https://stackoverflow.com/questions/11520492/difference-between-del-remove-and-pop-on-lists

# Get the output of shell script from python
process = subprocess.Popen(cmd, stdout=subprocess.PIPE, shell=True)
out = commands.getoutput( cmd )
- https://stackoverflow.com/questions/4760215/running-shell-command-from-python-and-capturing-the-output

# Notebook
## start up
$ jupyter-notebook --notebook-dir=/home/bliu/workspace/PythonDev/NoteBook &

## Run matplotlib plot inside the Notebook 
%matplotlib inline  # make sure matlotlib installed first

import matplotlib
import numpy as np
import matplotlib.pyplot as plt

- https://stackoverflow.com/questions/19410042/how-to-make-ipython-notebook-matplotlib-plot-inline

## setup python2 and python3 kernels for jupyter
$ python -m pip install ipykernel
$ python -m ipykernel install --user

- https://stackoverflow.com/questions/30492623/using-both-python-2-x-and-python-3-x-in-ipython-notebook

## allow remote access
$ jupyter-notebook --generate-config
$ vim ~/.jupyter/jupyter_notebook_config.py
c.NotebookApp.allow_origin = '*'    */
c.NotebookApp.ip = '0.0.0.0'
- https://stackoverflow.com/questions/42848130/why-i-cant-access-remote-jupyter-notebook-server

## disable authentication
$ vim ~/.jupyter/jupyter_notebook_config.py
c.NotebookApp.token = ''
c.NotebookApp.password = ''
- https://jupyter-notebook.readthedocs.io/en/latest/security.html#server-security

## can't use python2 kernel after upgrading ipykernel
I installed RISE someday before, but it didn't work well, so I reinstalled or upgrade the jupyter-notebook to the newest version. Then I restarted the jupyter-notebook, but all the notebook using python2 kerenl got stucked. After taking some effort, I found two solutions: 1. down-grade the ipykernel to a former specified version; 2. use python3 instead of python2. I choose the second one, and encounter some differences between python2 and python3.

### TypeError: a bytes-like object is required, not 'str' when writing to a file
other, oid = child.after.split(":") => other, oid = child.after.decode('utf-8').split(":")
- https://stackoverflow.com/questions/33054527/python-3-5-typeerror-a-bytes-like-object-is-required-not-str-when-writing-t

### Error: “ 'dict' object has no attribute 'iteritems' ”
capital.iteritems() => capital.items()
- https://stackoverflow.com/questions/30418481/error-dict-object-has-no-attribute-iteritems

### TypeError: the JSON object must be str, not 'bytes'
json.loads(child.after) => json.loads(child.after.decode('utf-8'))
- https://blog.csdn.net/bwlab/article/details/54019735

# pip 
# install pip itself
$ wget https://bootstrap.pypa.io/get-pip.py
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
$ python get_pip.py
$ python3 get_pip.py
- https://pip.pypa.io/en/stable/installing/

$ python -m pip install/search/uninstall
```
# Another way to install pip
$ sudo apt install python-pip # or python3-pip
$ sudo pip install pip -U # update the pip to the current version
$ sudo apt remove python-pip # this version is too old, it's 9.0x on Ubuntu 18.04. Otherwise, we'll get "ImportError: cannot import name main".
```

## Python 3.6 No module named pip
install pip as the above
- https://stackoverflow.com/questions/44622182/python-3-6-no-module-named-pip


## using domestic source
$ pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

$ mkdir -p ~/.config/pip
$ vim ~/.config/pip/pip.conf
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple

- https://mirrors.tuna.tsinghua.edu.cn/help/pypi/

## error: Python.h: No such file or directory
$ sudo apt-get install python-dev
- https://stackoverflow.com/questions/4450111/cython-install-gcc-error

It is available as python2-devel or python3-devel under Python section for some.
- https://stackoverflow.com/questions/21843714/cygwin-gcc-issue-cannot-find-python-h

## error: **partially uninstall the project(six)
$ sudo pip install six --upgrade --ignore-installed six

## pip install specific version of package
$ sudo pip install 'stevedore>=1.3.0,<1.4.0'
$ pip install -Iv MySQL_python==1.2.2 # -I --ignore-installed
- https://stackoverflow.com/questions/5226311/installing-specific-package-versions-with-pip

## pip install with socks5 proxy
```
$ pip --proxy socks5://localhost:1080 install pandas
# we may get "Missing dependencies for SOCKS support", try this to fix
$ sudo apt install python-socks
```

# PyDev
## Remove trailing whitespace in PyDev for Eclipse
Preferences > PyDev > Editor > Code Style > Code Formatter
and check the "Right trim lines?" check box.
- https://stackoverflow.com/questions/7744258/how-to-remove-trailing-whitespace-in-pydev-plugin-for-eclipse

# MySQL
$ pip install mysqlclient # MySQL-python for legacy Python only
- https://stackoverflow.com/questions/25865270/how-to-install-python-mysqldb-module-using-pip

# Convert epoch time with nanoseconds to human-readable
from datetime import datetime
dt = datetime.fromtimestamp(1360287003083988472 // 1000000000)
>>> nanos = 1360287003083988472
>>> secs = nanos / 1e9
>>> dt = datetime.datetime.fromtimestamp(secs)
>>> dt.strftime('%Y-%m-%dT%H:%M:%S.%f')

def format_my_nanos(nanos):
    dt = datetime.datetime.fromtimestamp(nanos / 1e9)
    return '{}{:03.0f}'.format(dt.strftime('%Y-%m-%dT%H:%M:%S.%f'), nanos % 1e3)

- https://stackoverflow.com/questions/15649942/python-convert-epoch-time-with-nanoseconds-to-human-readable

# ImportError: No module named _tkinter, please install the python-tk package
$ sudo apt-get install python-tk
$ sudo apt-get install python3-tk # for python3
- https://stackoverflow.com/questions/4783810/install-tkinter-for-python
- https://askubuntu.com/questions/815874/importerror-no-named-tkinter-please-install-the-python3-tk-package

# Format float numbers to string
>>> a = 13.9465
>>> print("%.2f" % a )
>>> print("%.2f" % round(a, 2))
>>> print("{0:.2f}".format(a))
>>> print("{0:.2f}".format(round(a, 2)))
- https://stackoverflow.com/questions/455612/limiting-floats-to-two-decimal-points
- https://stackoverflow.com/questions/8885663/how-to-format-a-floating-number-to-fixed-width-in-python

>>> str_a = "%.2f" % a
>>> str_a = "{:.2f}".format(a)
- https://stackoverflow.com/questions/15263597/convert-floating-point-number-to-certain-precision-then-copy-to-string
- https://stackoverflow.com/questions/1566936/easy-pretty-printing-of-floats-in-python

## format throws KeyError
"{{}}{}".format(" ") # to output the {} to the string
- https://stackoverflow.com/questions/9623134/python-format-throws-keyerror

# Pandas
## iterate over rows in a DataFrame
for index, row in dfSummary.iterrows():
    print index, row['Onload'], row['Type'], row['MsgSize'], row['Avg']

- https://stackoverflow.com/questions/16476924/how-to-iterate-over-rows-in-a-dataframe-in-pandas

## update index after sorting DataFrame
df2 = df2.reset_index(drop=True)
- https://stackoverflow.com/questions/33165734/update-index-after-sorting-data-frame

## get column mean
df['weight'].mean()
- https://stackoverflow.com/questions/31037298/pandas-get-column-average-mean

## get the row count of DataFrame
total_rows=len(df.axes[0])
total_cols=len(df.axes[1])
Count_Row=df.shape[0] #gives number of row count
Count_Col=df.shape[1] #gives number of col count
- https://stackoverflow.com/questions/15943769/how-do-i-get-the-row-count-of-a-pandas-dataframe

## datetime to string
Series.dt.strftime("%Y%m%d")
- https://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.dt.strftime.html

## set the font size of xticks
plt.xticks(fontsize=14, rotation=90)
plot.tick_params(axis='both', which='minor', labelsize=8)
- https://stackoverflow.com/questions/6390393/matplotlib-make-tick-labels-font-size-smaller
- https://stackoverflow.com/questions/11264521/date-ticks-and-rotation-in-matplotlib
- https://stackoverflow.com/questions/11264521/date-ticks-and-rotation-in-matplotlib/42888374
- https://stackoverflow.com/questions/19273040/rotating-axis-text-for-each-subplot/19273945

## set the font size of label
fig.suptitle('test title', fontsize=20)
plt.xlabel('xlabel', fontsize=18)
- https://stackoverflow.com/questions/12444716/how-do-i-set-the-figure-title-and-axes-labels-font-size-in-matplotlib

## save plot to image file
plt.save("foo.png")
savefig('foo.png', bbox_inches='tight')
have to do this before show.
- https://stackoverflow.com/questions/9622163/save-plot-to-image-file-instead-of-displaying-it-using-matplotlib

## no numeric data to plot
df = df.astype(float)
- https://stackoverflow.com/questions/31494870/pandas-dataframe-no-numeric-data-to-plot-error

## ValueError: cannot reindex from a duplicate axis
There may be duplicate values.
- https://stackoverflow.com/questions/27236275/what-does-valueerror-cannot-reindex-from-a-duplicate-axis-mean

## Make new column in Pandas dataframe by adding values from other columns
df['C'] = df['A'] + df['B']
- https://stackoverflow.com/questions/34023918/make-new-column-in-panda-dataframe-by-adding-values-from-other-columns

## Adding new column to existing DataFrame
df1['e'] = Series(np.random.randn(sLength), index=df1.index)
df1['e'] = p.Series(np.random.randn(sLength), index=df1.index)
df1 = df1.assign(e=p.Series(np.random.randn(sLength)).values)
- https://stackoverflow.com/questions/12555323/adding-new-column-to-existing-dataframe-in-python-pandas

## Convert column with dtype as object to string
df['column'] = df['column'].astype('str') 
df['column_new'] = df['column'].str.split(',') 
- https://stackoverflow.com/questions/33957720/how-to-convert-column-with-dtype-as-object-to-string-in-pandas-dataframe

## Getting the row which has the max value in groups using groupby
df.groupby(['Mt'], sort=False)['count'].max()
idx = df.groupby(['Mt'])['count'].transform(max) == df['count']
df['count_max'] = df.groupby(['Mt'])['count'].transform(max)
- https://stackoverflow.com/questions/15705630/python-getting-the-row-which-has-the-max-value-in-groups-using-groupby

## sort dataframe by index
df.sort_index(inplace=True) # may cause some warning in jupyter.
- https://stackoverflow.com/questions/22211737/python-pandas-how-to-sort-dataframe-by-index

## filter rows of dataframe
df[(df.A == 1) & (df.D == 6)]
- https://stackoverflow.com/questions/11869910/pandas-filter-rows-of-dataframe-with-operator-chaining

## plot line in dataframe
df.plot.line( y='latency' )
- https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.line.html

## delete column from dataframe
df.drop('column_name', 1)
- https://stackoverflow.com/questions/13411544/delete-column-from-pandas-dataframe-using-del-df-column-name

## convert dataframe column to datatime
df['date'] = df['date'].astype('datetime64[ns]')
df['date'] = df['date'].astype('datetime64[s, UTC]') # This may cause error when running for the first time.
- https://stackoverflow.com/questions/17134716/convert-dataframe-column-type-from-string-to-datetime

## converting timezones from pandas Timestamp
df.Date.dt.tz_localize('UTC').dt.tz_convert('Asia/Kolkata')
- https://stackoverflow.com/questions/42826388/using-time-zone-in-pandas-to-datetime

df['date'] = pd.to_datetime(df['date'].astype(int), unit='s') # less useful
- https://stackoverflow.com/questions/34291522/timestamp-conversion-to-datetime-python-pandas

## set index in dataframe
df.set_index(['column_names'])
- https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.set_index.html




# Matplotlib
## Enable or disable grid
ax.grid(False)
ax.set_xtick([])
- https://stackoverflow.com/questions/45148704/how-to-hide-axes-and-gridlines-in-matplotlib-python
- https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.grid.html
- https://stackoverflow.com/questions/14908576/how-to-remove-frame-from-matplotlib-pyplot-figure-vs-matplotlib-figure-frame

## colors to use
- https://matplotlib.org/users/colors.html

## bar and line
- https://stackoverflow.com/questions/38810009/matplotlib-plot-bar-and-line-charts-together
- https://stackoverflow.com/questions/32474434/trying-to-plot-a-line-plot-on-a-bar-plot-using-matplotlib

## bar stacked
- https://matplotlib.org/examples/pylab_examples/bar_stacked.html

## Warning about too many open figures
plt.cla() clears an axis, i.e. the currently active axis in the current figure. It leaves the other axes untouched.

plt.clf() clears the entire current figure with all its axes, but leaves the window opened, such that it may be reused for other plots.

plt.close() closes a window, which will be the current window, if not specified otherwise. plt.close('all') will close all open figures.
- https://stackoverflow.com/questions/21884271/warning-about-too-many-open-figures

plt.clf()
plt.close(fig)
fig is the return value of plt.figure() 

## Change data type of columns
pd.to_numeric()
- https://stackoverflow.com/questions/15891038/change-data-type-of-columns-in-pandas

## Append string to the start of each value
df['col'] = 'str' + df['col'].astype(str)
- https://stackoverflow.com/questions/20025882/append-string-to-the-start-of-each-value-in-a-said-column-of-a-pandas-dataframe

## Import pandas dataframe column as string
read_csv('sample.csv', dtype={'ID': object})
- https://stackoverflow.com/questions/13293810/import-pandas-dataframe-column-as-string-not-int

## Add one row in a DataFrame
for i in range(5):
    df.loc[i] = [randint(-1,1) for n in range(3)]
pandas.concat() or DataFrame.append()
- https://stackoverflow.com/questions/10715965/add-one-row-in-a-pandas-dataframe
- http://pandas.pydata.org/pandas-docs/stable/merging.html

# Map
## Iterating over map

for key in d:

for key, value in d.iteritems(): # Python 2.x

for key, value in d.items(): # Python 3.x
- https://stackoverflow.com/questions/3294889/iterating-over-dictionaries-using-for-loops

## sort a dictionary by key
import collections
d = {2:3, 1:89, 4:5, 3:0}
od = collections.OrderedDict(sorted(d.items()))

for key in sorted(d):
    print "%s: %s" % (key, d[key])
- https://stackoverflow.com/questions/9001509/how-can-i-sort-a-dictionary-by-key

# Magic function
__len__
__getitem__
__setitem__
__delitem__

# Pexpect
## multiple expects
i = child.expect(['first', 'second'])
if i == 0:
    # do something
else:
    # do some other thing.
- https://stackoverflow.com/questions/35132976/pexpect-multiple-expects/35134678

## expect regex
ValidIpAddressRegex = "^(([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])\.){3}([0-9]|[1-9][0-9]|1[0-9]{2}|2[0-4][0-9]|25[0-5])$";
- https://stackoverflow.com/questions/106179/regular-expression-to-match-dns-hostname-or-ip-address

## get the matched string
child = pexpect.spawn();
child.expect();
child.after();
child.match;
- https://stackoverflow.com/questions/12825363/capture-output-from-pexpect

# string
str.join();
str.replace();
str.split();

## Unbalanced parenthesis python
may be we treat the string as a regex expression, but the parenthesis is not complete.
- https://stackoverflow.com/questions/10318248/unbalanced-parenthesis-python

## create an long multi-line string
s = """ this is a very
        long string if I had the
        energy to type more and more ..."""

s = ("this is a very"
      "long string too"
      "for sure ..."
     )
- https://stackoverflow.com/questions/10660435/pythonic-way-to-create-a-long-multi-line-string

## Why don’t shell pipe and redirect (| and >) work when I spawn a command?
shell_cmd = 'ls -l | grep LOG > log_list.txt'
child = pexpect.spawn('/bin/bash', ['-c', shell_cmd])
child.expect(pexpect.EOF)
- https://pexpect.readthedocs.io/en/stable/FAQ.html

# path
## list all files of a directory
os.listdir()
os.walk()
- https://stackoverflow.com/questions/3207219/how-do-i-list-all-files-of-a-directory

# Python 3
## Python 2 to Python 3

NameError: name 'file' is not defined
Use open() instead
- https://stackoverflow.com/questions/16736833/python-nameerror-name-file-is-not-defined

email module ImportError: No module named Utils
import email.utils
- https://stackoverflow.com/questions/32874326/python-email-module-importerror-no-module-named-utils/32895112

error: No module named 'email.MIMEMultipart'
from email.mime.multipart import MIMEMultipart
- https://stackoverflow.com/questions/39541394/why-am-i-getting-the-error-no-module-named-email-mimemultipart

Unable to install the package "email"
it's a standard library
- https://stackoverflow.com/questions/35425998/unable-to-install-the-package-email

ImportError: No module named 'cStringIO'
from io import StringIO
- https://stackoverflow.com/questions/28200366/python-3-4-0-email-package-install-importerror-no-module-named-cstringio

## MySQLdb
$ pip3 install mysqlclient
- https://stackoverflow.com/questions/23376103/python-3-4-0-with-mysql-database

# Python Parallel programming
- https://python-parallel-programmning-cookbook.readthedocs.io/zh_CN/latest/chapter1/index.html

# jinja2
- https://www.xncoding.com/2016/09/05/python/jinja2.html

# airflow
- http://airflow.apache.org/index.html

# Excelent Python Libraries
- pandas
- matplotlib
- pip
- 

# bytes
## convert bytes to a string
b'abcde'.decode('utf-8')
b'abcde'.decode('ascii')
- https://stackoverflow.com/questions/606191/convert-bytes-to-a-string

# struct
- https://docs.python.org/3.6/library/struct.html#struct-examples

# datetime
- https://docs.python.org/3/library/datetime.html#timedelta-objects

# distutils
## ModuleNotFoundError: No module named 'distutils.*'
$ sudo apt install python3-distutils
- https://superuser.com/questions/1319047/cant-install-virtual-interpreter-in-pycharm-in-linux

