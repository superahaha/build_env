# MAC安装Mongodb

# 一、Homebrew安装
## 1、预下载
>brew tap mongodb/brew
如果碰到类似于fatal: unable to access 'https://github.com/mongodb/homebrew-brew/': HTTP/2 stream 1 was not closed 
依次执行以下命令，重启终端就可以继续
git config --global --unset http.proxy
git config --global --unset https.proxy

## 2、更新homebrew
>brew update

## 3、安装MongoDB（M1/2芯片的目录不同）
>brew install mongodb-community@6.0
configuration file: /usr/local/etc/mongod.conf
log directory: /usr/local/var/log/mongodb
data directory: /usr/local/var/mongodb

## 4、显式启用/停用MongoDB
>brew services start mongodb-community@6.0
>brew services stop mongodb-community@6.0

## 5、手动启用MongoDB后台服务
>mongod --config /usr/local/etc/mongod.conf --fork

## 6、查看服务
>brew services list
>ps aux | grep -v grep | grep mongod

## 7、连接并使用MongoDB
在终端输入：mongosh

## 8、使用MongoDB工具
在终端输入：mongotop


# 二、手动安装
## 1、到官网下载 MongoDB Community 安装包

## 2、解压
>tar -zxvf mongodb-macos-x86_64-6.0.tgz

## 3、把mongodb包移到环境变量目录下
>sudo mv mongodb-macos-x86_64-6.0 mongodb
>sudo mv mongodb /usr/local/

## 4、创建mongo数据库目录
>sudo mkdir -p /usr/local/var/mongodata

## 5、创建日志目录
>sudo mkdir -p /usr/local/var/log/mongolog

## 6、修改目录所有者
>sudo chown my_mongodb_user /usr/local/var/mongodata
>sudo chown my_mongodb_user /usr/local/var/log/mongolog

## 7、运行MongoDB
方式一
>mongod --dbpath /usr/local/var/mongodata --logpath /usr/local/var/log/mongolog/mongo.log --fork
alias mongod='mongod --dbpath /usr/local/var/mongodata --logpath /usr/local/var/log/mongolog/mongo.log --fork'

方式二
>mongod --config /usr/local/etc/mongod.conf
#mongod.conf
#指定数据库文件存放目录
dbpath=/usr/local/var/mongodata
#数据库日志存放目录
logpath=/usr/local/var/log/mongolog/mongo.log
#以追加方式记录日志
logappend=true
#以后台运行方式运行进程
fork=true

## 8、查看运行情况
>ps aux | grep -v grep | grep mongod

## 9、开始使用MongoDB
>mongosh  # 需要先下载安装mongosh（MongoDB Shell）
