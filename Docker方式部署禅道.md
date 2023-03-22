# Docker方式部署禅道

## ⼀、准备环境
运⾏环境需成功部署Docker服务，推荐使⽤Docker 18版本以上，对主机环境没有要求。
查看Docker版本：docker -v

## ⼆、下载禅道镜像
⽬前⽀持在线下载和离线导⼊两种部署禅道镜像的⽅式，可根据⾃⼰环境进⾏选择。
### 1、在线下载
 - 禅道镜像已放于Docker Hub上，地址为： https://hub.docker.com/r/easysoft/zentao/tags
   可根据所需版本拉取对应版本的镜像，默认latest为禅道开源最新版本。
   sudo docker pull [镜像名]:[镜像标签]
   例如拉取禅道开源版12.3.3版本命令如下：
   sudo docker pull easysoft/zentao:12.3.3

### 2、离线导⼊
 - 禅道版本对应的镜像归档⽂件如下表所示：
   禅道版本 镜像名字 镜像归档⽂件下载路径
   开源版12.3.3 easysoft/zentao:12.3.3 /dl/docker/zentao_12.3.3.tar.gz
   专业版8.8.3 easysoft/zentao: pro8.8.3 /dl/docker/zentao_pro8.8.3.tar.gz
   企业版3.7.2 easysoft/zentao: biz3.7.2 /dl/docker/zentao_biz3.7.2.tar.gz

导⼊镜像步骤如下：
1）选择合适的版本进⾏下载，拷⻉⾄主机下某位置；

2）进⼊主机的镜像归档⽂件⽬录，执⾏导⼊镜像命令。
 > sudo gunzip -c [镜像归档⽂件名] | docker load
例如拉取禅道开源版12.3.3版本命令如下：
 > sudo gunzip -c zentao_12.3.3.tar.gz | docker load

## 三、启动禅道容器
### 1、创建docker⽹络驱动
 > sudo docker network create --subnet=[ip范围] [⽹络驱动名]
其中，ip范围：例如172.172.172.0/24的意思ip可以指定范围为172.172.172.1到172.172.172.254；
(在AWS上使⽤172.172.172.0/99时创建失败)
⽹络驱动名：创建的⽹络驱动名，可随意指定；
例如： sudo docker network create --subnet=172.172.172.0/24 zentaonet

### 2、启动禅道容器
命令格式如下：
 > sudo docker run --name [容器名] -p [主机端⼝]:80 --network=[⽹络驱动名] --ip [容器IP] --mac-address [mac地址] -v [主机禅道⽬录]:/www/zentaopms -v [主机mysql⽬录]:/var/lib/ mysql -e MYSQL_ROOT_PASSWORD=[数据库密码] -d easysoft/zentao:[镜像标签]
其中，容器名：启动的容器名字，可随意指定；
 - 主机端⼝：主机端⼝为web访问端⼝；
 - ⽹络驱动名：刚刚创建的⽹络驱动名；
 - 容器IP：在⽹络驱动范围内选择⼀个作为该容器的固定ip；
 - mac地址：指定固定的mac地址，建议范围为02:42:ac:11:00:00 到 02:42:ac:11:ﬀ:ﬀ；
 - 主机禅道⽬录：必须指定，⽅便禅道代码、附件等数据的持久化，⾮升级情况需指定空⽬录；
 - 主机mysql⽬录：必须指定，⽅便禅道数据持久化，⾮升级情况需指定空⽬录；
 - 数据库密码：容器内置mysql⽤户名为root,默认密码123456，如果不修改可以不指定该变量，如果想更改密码可以设置 MYSQL_ROOT_PASSWORD变量来更改密码；
 - 镜像标签：禅道版本。

例如：在主机上创建空⽬录/www/zentaopms和/www/mysqldata，执⾏如下命令：
 > sudo docker run --name zentao -p 80:80 --network=zentaonet --ip 172.172.172.172 -mac-address 02:42:ac:11:00:00 -v /www/zentaopms:/www/zentaopms -v /www/mysqldata:/ var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d easysoft/zentao:12.3.3

IGC【启动禅道容器】
 > sudo docker run --name zentao -p 60080:80 -p 3308:3306 --network=zentaonet --ip 172.172.172.17 --mac-address 02:42:ac:11:00:00 -v /opt/zentaopms:/www/zentaopms -v / var/lib/mysql_zentao:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d easysoft/ zentao:17.8

备注： 如果需要远程连接数据库，可以增加⼀个端⼝映射 “-p [主机端⼝]:3306” 如果在单个主机上部署多个禅道系统，只需要指定不同的[容器名]、[主机端⼝]、[容器IP]、[mac地 址]、[主机禅道⽬录]、[主机mysql⽬录]即可部署多个禅道系统，例如：
 > sudo docker run --name zentao2 -p 8080:80 -p 3306:3306 --network=zentaonet --ip 172.172.172.173 --mac-address 02:42:ac:11:00:01 -v /www/zentaopms:/www/zentaopms -v /www/mysqldata2:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d easysoft/ zentao:12.3.3

### 3、查看容器是否启动成功
执⾏如下命令查看容器是否启动成功，如果没有则启动失败，去掉-d选项进⾏前台运⾏调试容器，如有任何问题请咨询禅道商务同事。
 > docker ps

## 四、安装禅道
浏览器直接访问 http://宿主机ip:宿主机映射端⼝
确认信息后下⼀步即可，在【⽣成配置⽂件】⻚输⼊前⾯启动容器的数据库密码。
。。。。。。

## 五、升级禅道
### 1.先停⽌禅道容器，为备份数据做准备
命令格式如下：
 > docker stop [容器名]
例如：
 > docker stop zentao

### 2.备份禅道数据
为安全起⻅，将上⽂所述的[主机禅道⽬录]和[主机mysql⽬录]进⾏备份，例如将/www/zentaopms和/www/mysqldata拷⻉⾄主机其他⽬录。

### 3.获取最新版源码包，开源版从禅道官⽹获取，其他版本可咨询禅道商务获取

### 4.解压缩后，覆盖 [主机禅道⽬录]，例如解压缩后覆盖主机/www/zentaopms⽬录

### 5.启禅道容器
 > docker start [容器名]
例如：
 > docker start zentao

### 6.访问upgrade.php，选择对应的版本，按照提示进⾏即可

开源版、专业版、企业版的升级相似，具体可以参考开源版禅道的升级 http://www.zentao.net/ help-read-78960.html。


## 六、安装svn、git客户端 (如果不使⽤svn、git集成的话，不⽤安装)
1. 进⼊容器
 > docker exec -it [容器名] /bin/bash
2. 执⾏命令
 > apt-get install git -y
 > apt-get install subversion -y

### 七、FAQ
A：如何修改容器⾥的mysql等配置⽂件
Q：使⽤如下命令
 > docker exec -it [容器名] /bin/bash

⽐如进⼊禅道容器，修改my.cnf后保存，重启mysql服务
 > docker exec -it zentao /bin/bash
 > vi /etc/mysql/my.cnf

同理，修改apache、php.ini的配置也类似，以下列出容器内相关组件的路径。
容器内apache配置⽂件⽬录：/etc/apache2/
容器内禅道⽬录：/www/zentaopms
容器内mysql配置⽂件⽬录： /etc/mysql/
容器内php配置⽂件⽬录：/etc/php/7.0/apache2

A：如何远程连接容器⾥的mysql
Q：请参考：https://www.zentao.net/book/zentaopmshelp/276.html
 - 1)、确认主机与容器中mysql有端⼝映射；
   2)、确认/etc/mysql/mariadb.conf.d/50-server.cnf 中的“ bind-address = 0.0.0.0”；
   3)、确认远程登录⽤户权限，如使⽤root⽤户： rename user 'root'@'localhost' to ‘root'@'%';

A：为什么容器挂载出来的/www/zentaopms⽬录是空⽂件夹
Q：在禅道12.3.1、12.2.stable、biz3.7、pro8.8这四个镜像中，禅道的挂载⽬录为/app/ zentaopms，需修改挂载⽬录
例如：在主机上创建空⽬录/app/zentaopms和/app/mysqldata，执⾏如下命令
 > sudo docker run --name zentao -p 80:80 --network=zentaonet --ip 172.172.172.172 -mac-address 02:42:ac:11:00:00 -v /app/zentaopms:/app/zentaopms -v /app/mysqldata:/ var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d easysoft/zentao:12.3.3
