# 3 准备好Tango

让我们一起出发!为了和Django一起探戈,你需要确保你已经在你的电脑上安装了所有需要的东西,而且你需要掌握你的开发环境.本章让你了解你需要什么并且需要掌握哪些知识.

对于本教程,你需要下面两个关键的软件:

* Python 2.7.5+
* Django 1.7

作为编写Django这个web框架的程序语言,你需要了解Python的一些知识.如果你从来没有使用过Python并且希望掌握这门语言,我们强烈推荐你学习以下至少一个教程.

* A quick tutorial - Learn Python in 10 Minutes by Stavros, http://www.korokithakis.net/tutorials/python/.
* The Official Python Tutorial at http://docs.python.org/2/tutorial/.
* A brilliant book: Think Python: How to Think like a Computer Scientist by Allen B. Downey, available online at http://www.greenteapress.com/thinkpython/.
* An amazing online course: Learn to Program, by Jennifer Campbell and Paul Gries at https://www.coursera.org/course/programming1.

## 3.1 使用终端

学习如何使用你的操作系统提供的命令行交互对设置你的环境来说至关重要.在本教程中,你应当习惯用命令行进行交互.如果你已经熟悉使用命令行那么你可以直接跳到安装软件这一章节.

类UNIX系统都有一个看起来相似的终端.UNIX有许多派生或者衍生的后代,包括苹果的OS X和许多Linux发行版本.所有的这些操作系统都包含一系列命令行界面可以帮助你管理文件和运行程序,他们并不需要图形界面.本节介绍你应当掌握的关键命令.

>注意:这个教程针对的是类UNIX系统.虽然Django和Python可以在Windows系统下运行,但是这本书中的大部分命令都是针对类UNIX终端的.在Windows中,可以用与UNIX相近的命令或者使用仿UNIX终端的PowerShell来运行这些命令.

如果进入一个终端里,你会看到如下的形式:

```
sibu:~leif$
```

这个叫做提示符,提示你系统正在等待执行键入的命令.这个提示符可能因为你系统的不同而有所区别,但是大致上看起来都这样.在上面的例子中给出了3个有用的信息:

* 你的用户名和电脑名(`leif`用户名和`sibu`电脑名)
* 你的当前目录(波浪线,或者`~`)
* 你用户的权限(美元符,或者`$`)

美元符(`$`)一般意味着用户作为一个标准的用户.相反,如果是井字号(`#`)则表示用户具有root权限.不管什么符号都意味着电脑正在等待你的输入.

打开终端窗口,看一看你的提示符是什么.

当你使用终端的时候,知道你处于系统文件的什么位置是非常重要的.为了知道你位于哪里,你可以键入`pwd`命令.它会显示出你当前工作目录.例如:

```
Last login: Mon Sep 23 11:35:44 on ttys003
sibu:~ leif$ pwd
/Users/leif
sibu:~ leif$
```

你可以看到当前工作目录是:`/Users/leif`.

同时你可以注意到我当前工作目录为~.这是因为波浪线(`~`)表示我的主目录.所有类UNIX文件系统的都是根目录作为起始基点的.根目录用`/`表示.

如果你不在你的主目录里,你可以通过键入如下命令改变目录(`cd`).

```
$ cd ~
```

让我们建一个名叫`code`的文件夹.可以键入建立文件夹命令(`mkdir`),如下.

```
$ mkdir code
```

进入新建的`code`文件夹,键入`cd code`.如果你检查你现在的工作目录,你可以看到你处在`~/code/`目录中.同样你可以通过你的提示符看出来.看下面的例子,当前工作目录出现在电主机名字`sibu`后面.

>注意:以后不管什么时候提到工作空间(`<workspace>`),我们指的是你的`code`目录

```
sibu:~ leif$ mkdir code
sibu:~ leif$ cd code
sibu:code leif$
sibu:code leif$ pwd
/Users/leif/code
```

列出目录中的文件,可以用`ls`命令.如果你想查看隐藏文件或目录只需要键入`ls -a`命令,这里`a`参数指代全部.如果`cd`回到你的主目录(`cd ~`)然后键入`ls`,你就会看到在你的主目录里有个叫`code`的文件夹.

为了查看你目录里的更多信息,可以输入`ls -l`.它会展示你文件的许多详细信息包括他是否是目录(通过前面的`d`可以看出)

```
sibu:~ leif$ cd ~
sibu:~ leif$ ls -l

drwxr-xr-x   36 leif  staff    1224 23 Sep 10:42 code
```

输出信息同时包含目录权限的信息,谁建立了这个文件夹(`leif`),它的群组(`staff`),大小,这个文件修改时间,当然还有它的名字.

通过终端你可以发现可以很方便的修改文件.在你的电脑上可能已经安装了许多出色的编辑器.nano编辑器是一个很简洁的编辑器,它不像vi需要花很多时间来学习.下面将介绍一些十分有用的UNIX命令.

### 3.1.1 常用命令

所有的类UNIX操作系统都有一系列的针对文件管理的内建命令.下面将列出一些使用频率非常高的命令,我们将会简单介绍它们是干什么的和如何去使用它们.

* `pwd`:在终端打印出当前工作目录.展示出当前所在目录的绝对路径.
* `ls`:在终端打印出当前工作目录中文件列表.默认情况下,你不会看到文件大小 - 可以通过`ls -lh`命令进行查看.
* `cd`:后边连接路径,可以允许你改变当前工作目录.例如,`cd /home/leif/`改变工作目录到`/home/leif/`.你还可以通过键入两个点(`cd ..`)转移到上一级目录而不用输入绝对路径.
* `cp`:复制文件或者目录.你必须提供源和目标.例如,如果要在同一文件夹里复制`input.py`,那你可以这样输入`cp input.py input_backup.py`
* `mv`移动文件或者目录.像`cp`一样,必须提供源和目标.这个命令还可以重命名文件.例如,把`numbers.txt`重命名为`letters.txt`只需键入`mv numbers.txt letters.txt`.把一个文件移动到另一个目录,你可以使用绝对或者相对路径作为目的地址 - 比如`mv numbers.txt /home/david/numbers.txt`.
* `mkdir`:在当前目录创见文件夹.你需要提供一个新文件夹的名字在`mkdir`命令后面.例如,当前目录是`/home/david/`,运行`mkdir music`,你将会得到一个文件夹`/home/david/music/`.你需要键入`cd`来进入这个文件夹.
* `rm`:remove的简写,这个命令会删除你的文件.你必须提供你要删除文件的名字.如果你输入`rm`命令,它会提示你是否希望删除这个文件的选项.你也可以用递归删除来删除文件夹.用这个命令一定要小心,要恢复删除的文件几乎不可能.
* `rmdir`:删除目录的传统方法.在后面提供你要删除的目录.需要注意的是:这个命令没有提示是否要删除这个目录.
* `sudo`:允许你用其他用户权限来运行这个程序.一般,用`root`(类UNIX或者UNIX衍生系统的超级用户)身份来运行这个程序.

>注意:这里只列出了一小部分命令列表.如果想了解更多请查看ubuntu的关于使用终端的文档,或者FOSSwire写的Cheat Sheet.

## 3.2 安装软件

现在你已经知道怎么和终端打交道了,你可以按着这个教程的要求安装软件了.

### 3.2.1 安装Python

Python2.7.5已经安到你的电脑上了吗?如果你使用的是linux发行版或者OS X系统,那么你的Python已经安装好了.因为许多系统的功能都是有Python实现的,所以他们本身自带Python解析器!

很不幸,几乎所有的现代操作系统的Python版本都比我们在这个教程里需要的要老.有许多方法可以安装Python,许多方法都非常苦难.我们提供了最常用的方法,并且提供获取更多信息的链接.

>警告:这个段落将会详细介绍如何安装Python2.7.5.但是千万不要移除系统默认的Python.这样做可能损坏你的系统.

#### 3.2.1.1 苹果OS X

这个方法最简单,你只需要下载并运行官方的下载器即可.网址在 http://www.python.org/getit/releases/2.7.5/.

>警告:在下载前确保`.dmg`文件的版本和你的OS X系统的版本相符!

1. 下载完`.dmg`文件,在目录里双击.
2. 文件会以磁盘方式挂载,并会出现一个新的Finder窗口.
3. 双击`Python.mpkg`文件.将会运行Python安装器.
4. 你必须输入密码确认你要安装这个软件.
5. 所有以上都完成了,关闭安装器并弹出Python磁盘.现在可以删除`.dmg`文件.

你现在就已经安装了升级过后的Python了,为Django做好准备!是不是灰常简单?

#### 3.2.1.2 Linux发行版

不幸的是,在Linux发行版上下载更新Python有许多不同的方法.更糟糕的是每个发行版的方法都不一样.例如,Fedora安装Python的方法就和Ubuntu不一样.

然而并不是一点方法都没有.一个非常牛B的叫做Pythonbrew工具(或者说是Python环境管理器)可以帮助我们解决这一难题.它提供了一个简单的安装和管理不同版本Python的方法.这意味着你可以不用管系统已经默认安装的Python了.

可从pythonbrew的主页和stackOverflow了解到,安装Python2.7.5需要以下步骤.

1. 打开一个终端
2. 运行命令`curl -kL http://xrl.us/pythonbrewinstall | bash`.你将会下载一个安装器,然后在终端里运行它.pythonbrew将会安装在`~/.pythonbrew`目录.记住`~`符号表示你的主目录.
3. 你需要编辑`~/.bashrc`文件.通过编辑器(比如`gedit`,`nano`,`vi`或者`emacs`)打开,在文件最后加入下面`[[ -s $HOME/.pythonbrew/etc/bashrc ]] && source $HOME/.pythonbrew/etc/bashrc`
4. 保存`~/.bashrc`文件,关闭终端并重新打开一个新的.这样做你的改变就可以生效了.
5. 运行`pythonbrew install 2.7.5`来安装Python 2.7.5.
6. 现在你需要切换Python2.7.5来激活Python的安装.只需要键入`pythonbrew switch 2.7.5`.
7. Python 2.7.5就已经安装好了.

>注意:以点号开头的文件或者目录同windows里的隐藏文件是一样的.Dot file正常情况下是看不见的,他们通常用作配置文件.如果要查看隐藏的文件只需要键入`ls -a`.

#### 3.2.1.3 Windows

Windows并没有默认安装Python.也就是说不必考虑遗留版本的问题,只需下载安装即可.可以从Python官方网站下载64位或者32位的版本.如果你不确定要下载哪一个,你可以通过访问the Microsoft website来确定你的电脑是64位还是32位.

1. 当安装器下好以后,打开它.
2. 根据提示安装Python
3. 完成后关闭安装器,删除下载的文件.

当安装器完成以后,你的Python就能安装好并且能用了.Python2.7.5默认安装在`C:\Python27`文件夹内.我们建议你使用这个默认的路径.

安装完毕后,打开命令提示符输入`python`.如果看到Python的提示符,表示安装成功.然而有些时候安装器不能正确的设置Windows的`PATH`变量.将导致`python`命令不能被找到.在Windows7里可以按照如下:

1. 点击开始按钮,右键点击我的电脑选择属性.
2. 点击高级选项卡.
3. 电机环境变量按钮.
4. 在系统变量列表里,查找名字叫Path的变量,点击并选择编辑按钮
5. 在行末添加`;C:\python27;C:\python27\scripts`.不要忽略分号 - 还有不要添加空格.
6. 点击确定保存配置.
7. 关闭所有的命令行,重新打开,试试看键入`python`.

如果一步步做下来Python应当就能安装成功了.对于Windows XP来说,有一些不同,对于Windows 8的情况也是.

### 3.2.2 设立PYTHONPATH

当Python安装完毕,我们需要检查一下安装是否成功.为了确定安装成功我们需要检查`PYTHONPATH`环境变量成功被设定.`PYTHONPATH`为Python解析器提供了包和模块的位置.如果`PYTHONPATH`设置错误,我们将不能安装和使用Django!

首先,让我们验证`PYTHONPATH`变量存在.在类UNIX系统中,键入如下命令.

```
$ echo $PYTHONPATH
```

在Windows系统中.

```
$ echo %PYTHONPATH%
```

如果成功运行,你将会看到如下相似的输出.在Windows系统中你将会看见一个Windows路径,大多数它们是以C盘作为根目录.

```
/opt/local/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages:
```

这个路径就是Python包和模块存放的路径.如果你得到了这个路径,你可以继续接下来的教程了.如果你什么都没有得到,你需要做一些额外的工作去找到这个路径.如果在Windows系统里`site-packages`一般位于安装Python目录的`lib`文件夹里.例如,如果你在`C:\Python27`安装了Python,那么`site-packages`一般就会在`C:\Python27\Lib\site-packages\`.

对于类UNIX系统,先打开Python解析器.输入如下的命令.

```
$ python

Python 2.7.5 (v2.7.5:ab05e7dd2788, May 13 2013, 13:18:45)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

>>> import site
>>> print site.getsitepackages()[0]

'/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages'

>>> quit()
```

调用`site.getsitepackages()`将会返回Python存储包和模块的地址列表.如果调用`getsitepackages()`函数的时候返回错误,那么需要检查一下Python的版本.只有2.7.5及以上版本才包含这个函数.

`print site.getsitepackages()[0]`语句运行的结果会返回安装的`site-packages`目录.得到这个路径,我们需要把它添加进你的设置.在类UNIX系统里,再次编辑`.bashrc`文件,把下面这句加入文件里.

```
export PYTHONPATH=$PYTHONPATH:<PATH_TO_SITE-PACKAGES>
```

把`<PATH_TO_SITE-PACKAGES>`更换为你的`site-packages`目录地址.保存文件,退出并重新打开终端.

在Windows机器上,如前面的方法一样,设置一个`PYTHONPATH`变量并把它的地址设置为`C:\Python27\Lib\site-package`.

### 3.2.3 使用Setuptools和Pip

对于任何一个项目来说安装和设置开发环境都极为重要.由于像Django这样的包安装都是分别安装的,这将会导致许多问题和麻烦.比如说,你如何和其他的开发人员分享你的设置?如何在一台新的机器上部署同样的环境?如何把包升级到最新版本?使用包管理工具可以解决大部分安装和设置环境的问题.它可以确保安装的包适合你的Python版本,同时它也可以自动安装其他需要依赖的包文件.

在这本书里我们将使用Pip.Pip是一个对用户更友好的Python包管理工具.因为Pip依赖于Setuptools,所以我们必须保证两个程序都安装在你的电脑上.

一开始我们需要在Python package website下载Setuptools.你可以下载`.tar.gz`文件.用你喜欢的解压软件解压它.它们应当被解压到`setuptools-1.1.6`目录里 - 这里`1.1.6`代表Setuptools版本号.在终端里进入目录执行`ez_setup.py`脚本,如下.

```
$ cd setuptools-1.1.6
$ sudo python ez_setup.py
```

在上面的例子中,我们用`sudo`来允许在系统范围内的更改.第二个命令将会为你安装Setuptools.如果输出如下就可以验证安装成功.

```
Finished processing dependencies for setuptools==1.1.6
```

当然这里的`1.1.6`会用你下的版本号代替.如果你看到上面的输出,那么就可以接下来安装Pip了.这个步骤非常的简单,在终端中键入如下.

```
$ sudo easy_install pip
```

这个命令同样会在系统里下载和安装Pip.如果输出如下则安装成功.

```
Finished processing dependencies for pip
```

如果看到上面的输出,你就可以在终端里直接输入`pip`来启动Pip了.如果没有遇到什么错误的话,你将会看到一个Pip提供的命令列表.如果你看到了这个列表,那么恭喜你!

>注意:在Windows主机里,操作都一样.但是在输入命令的时候不用输入`sudo`.

### 3.2.4 安装Django

如果安装Pip成功的话,Django就容易安装了.打开终端让我们来输入.

```
$ pip install -U django==1.7
```

如果你使用类UNIX系统的话你需要在命令前加`sudo`.

```
$ sudo pip install -U django==1.7
```

包管理器将会下载Django到正确的位置.以上做完后,Django就安装成功了.注意,如果没有写`==1.7`,那么你将有可能安装其他版本的Django.

### 3.2.5 安装Python图像库

在建造Rango这个应用的时候,我们需要处理和上传图片.所以我们需要Pillow这个图形库.打开终端输入如下.

```
$ pip install pillow
```

如果需要前面加`sudo`.

### 3.2.6 安装其他Python包

值得注意的是,其他包也可以用这种方法很方便的下载下来.The Python Package Index提供了Pip可以安装包的列表.

可以通过下面的命令查看已经安装包的列表.

```
$ pip list
```

### 3.2.7 分享你的包列表

你可以将你的已安装包列表分享给其他的开发者.

```
$ pip freeze > requirements.txt
```

如果你用`more`,`less`或者`cat`来检查`requirements.txt`文件,你将会看见差不多一样的列表.这个`requirements.txt`文件可以用下面的命令安装.如果你要在另一台电脑上配置环境将变得非常方便.

```
$ pip install -r requirements.txt
```

## 3.3 集成开发环境

尽管不是必须的,但是一个好的集成开发环境(IDE)将会对我们的开发灰常有帮助.像JetBrain系列的PyCharm和PyDev(Eclipse的一个插件)都是比较流行的IDE.Python Wiki会提供一个实时更新的Python IDE列表.

可以试试到底哪个适合你,同时要知道其中一些需要购买认证.理想情况下,你会选择一个集成Django的IDE.PyCharm和PyDev都提供了Django的集成 - 尽管你需要告诉IDE你使用的是哪个版本的Python.

### 3.3.1 虚拟环境

我们马上就快设置完毕了!在我们继续之前,值得注意的是如果我们就这么安装的话会有很多缺点.加入我们有其他的Python应用,需要另一个版本才能运行?或者你想转到新的版本的Django,但仍然像维持Django1.7项目?

解决的方法就是virtual environment.虚拟环境可以允许我们同时安装不同版本的Python和他们的包.现在,这已经成为一个普遍的方法.

安装的话也非常好安装.

```
$ pip install virtualenv
$ pip install virtualenvwrapper
```

第一个包提供了创建虚拟环境的基础.如需更多细节可以参看Jamie Matthews的a non-magical introduction to Pip and Virtualenv for Python Beginners.如果仅仅使用virtualenv将会变得很复杂.安装的第二个包就是使这个过程简化.

如果你使用的是类UNIX系统,那么你需要在命令行中启动这个脚本:

```
$ source virtualenvwrapper.sh
```

为了不必每次使用时都输入这个命令可以在profile里设定.

如果你使用的是Windows环境,那么需要下载virtualenvwrapper-win包:

```
$ pip install virtualenvwrapper-win
```

现在你可以创见虚拟环境了:

```
$ mkvirtualenv rango
```

你可以用`lsvirtualenv`命令列出创建的虚拟环境,如果你要激活输入如下:

```
$ workon rango
(rango)$
```

你的命令提示符会改变而且会显示当前虚拟环境,像上面的rango.现在你可以在环境里安装你想要安装的任何包了,并且他们不会干涉其他的环境.键入`pip list`去检查是否安装了Django或者Pillow包.你可以用pip来安装他们,但是他们只是存在于虚拟环境里.

接下来我们将要开发应用并且设置一个虚拟环境,参看下一章 开发你的应用.

### 3.3.2 代码库

当你进行开发时,可以使用SVN或者GIT来进行版本控制.在我们的教程里不会使用版本控制.但是我们提供了crash course on GIT.我们强烈建议你在自己的项目里建立版本控制.他往往能终究你与水火.

## 3.4 练习

为了更好的设置你的环境,试着完成下列练习.

* 安装Python2.7.5+和Pip
* 练习CLI,创建名字叫`code`目录并把我们的项目放进去.
* 安装Django和Pillow包
* 设置虚拟环境
* 设置你的GitHub账号
* 下载并设置IDE(比如PyCharm)
* 我们将书中的代码放在了GitHub,参见Tango With Django Book和Rango Application
    * 如果你对这本书遇到错误和问题,你可以创建一个request!
    * 如果对练习题有问题,你可以检查代码库看看我们是如何完成的.

