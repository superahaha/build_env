# 在Mac配置Python和pip

## 1、Mac两个bin目录
相同点
/usr/bin和/usr/local/bin都是用来存储终端命令二进制文件或者命令的软链接
这两个bin目录都是已经包含在环境变量里的目录，程序放在里面或者链接到里面命令就可以在终端里直接执行。

不同点
Mac的/usr/bin目录是不允许增删文件的;
/usr/local/bin增删文件来实现在终端里直接运行，只需要有管理员权限。

注意搜索目录时最前面的“/”不能缺少


## 2、Mac的终端的用户可配置文件
可配置文件根据终端类型分为两种，这些文件都是隐藏的，语法结构相同，可以用来配置环境变量等，需要“Command+Shift+.”才能显示

bash和zsh是两个不同的shell，Shell俗称壳（区别于核），是指“提供使用者使用界面”的软件（命令解析器）。它类似于DOS下的command和后来的cmd.exe。它接收用户命令，然后调用相应的应用程序。bash和zsh之间有一定兼容性。

bash终端：
/users/用户名/.bash_profile

zsh终端：
/users/用户名/.zsh_profile
/users/用户名/.zshrc


## 3、查看位置命令
which pip
which pip3

查看python 的pip 包管理工具的启动路径（软链接的位置），一般都在上述提到的两个bin目录中间
pip –version 			
pip3 –version

用来展示命令的真实地址存储位置


实例
下面以pip3为例，在zsh中的针对pip3具体操作，同理要将终端中2.7版本的pip改为自己下载的pip版本，直接将下述所有的pip3改为pip

所有命令需根据自己的Python版本和真实位置而修改


首先需要保证/usr/local/bin的环境变量位置在/usr/bin前面，这样才能先读/usr/local/bin的数据，因为前者的数据可以更改

zsh终端下执行：
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc

注意此步可以先不操作，如果出现了permission denied或者command not find问题说明你碰到了/usr/bin，到时候再执行第一步


找到pip3的真实位置

一般来说，你下载的python 3.x的pip在

/Library/Frameworks/Python.framework/Versions/3.x/lib/python3.x/site-packages/pip (python 3.x)

删除已经存在的冗余数据
rm -rf /usr/local/bin/pip3

在/usr/local/bin/中重新创建pip3的软链接至上述pip3的真实位置

ln -s /Library/Frameworks/Python.framework/Versions/3.x/bin/pip   /usr/local/bin/pip3

此时在命令行输入pip3会自动指向你的Python版本的真实位置


验证
pip3 --version

我的终端显示：
pip 19.0.3 from /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages/pip (python 3.6)

which pip3
我的终端显示：
/usr/local/bin/pip3


## 4、Python 相关配置
1.查看 python3 安装路径
~查看 mac下都有哪些 Python3的安装路径   
viatorsun@MacBook ~ % where python3
/Library/Frameworks/Python.framework/Versions/3.7
/usr/local/bin/python3
/usr/local/bin/python3
/usr/local/bin/python3
/usr/bin/python3

不必在意出现多个 /usr/local/bin/python3


2.删除Python 3.7 框架：
$ ls /Library/Frameworks/Python.framework/Versions/3.7
$ sudo rm -rf /Library/Frameworks/Python.framework/Versions/3.7


3.删除Python 3.7 应用目录：
$ cd /Applications
$ sudo rm -rf Python\ 3.7/   # Python 3.7存在空格
 或者
sudo rm -rf "/Applications/Python 3.7"
查看launchpad中python3的IDLE就被删除了


4.删除/usr/local/bin 目录下指向的Python3.7 的连接：
$ cd /usr/local/bin/
$ ls -l /usr/local/bin
$ rm Python3.7相关的文件和链接  # Python3.7相关的文件和链接需要你自行确认
# 或者
cd /usr/local/bin/
ls -l /usr/local/bin | grep '/Library/Frameworks/Python.framework/Versions/3.7'   # 查看链接
brew prune   # 清除链接和目录

清除后可再次查看链接，会发现链接已清除



5.删除python的环境路径
$ vi ~/.bash_profile
删除Python3.7设置的环境路径。


6.确认python是否已经删除
$ python3
-bash: python3: command not found


## 5、删除/usr/local/bin 目录下指向的Python3.7 的连接：
进入目录
cd /usr/local/bin/

列出目录下所有与“python”字符串相关的文件
ls -al /usr/local/bin | grep python

清除所有相关链接与目录
brew prune  

如报错：Error: Unknown command: prune
则换用：
brew cleanup


## 6、将Python3设置为系统默认Python
1.打开终端，下载python3
brew install python3

2.查看下载的python3 位置
which python3
得到类似 /usr/local/bin/python3 的路径

3.修改 bash_profile 文件
vim ~/.bash_profile
在insert模式(按i)将python3 路径写入：
alias python="/usr/local/bin/python3"
esc, 然后‘:’底线命令模式, 输入 ‘wq!’

4.在命令行输入：
source ~/.bash_profile

即可修改成功，通过python --version查看，永久改为python3的默认。
