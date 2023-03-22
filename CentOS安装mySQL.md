# CentOS安装mySQL

## 1 检查环境 
[liao@localhost ~]$ ``cat /etc/redhat-release``，这种方法只适合Redhat系的Linux

## 2 下载 MySQL 的 Yum 源 
根据CentOS版本选择对应的mySQL，下载MySQL的 Yum Repository。  
[liao@localhost ~]$ ``wget https://dev.mysql.com/get/mysql80-community-release-el7-6.noarch.rpm``

## 3 用yum命令安装下载好的rpm包  
提示没权限，切到root账号  
[liao@localhost ~]$ ``su root``
[root@localhost liao]# ``yum -y install mysql80-community-release-el7-6.noarch.rpm``

## 4 安装mySQL Server  
[root@localhost liao]# ``yum -y install mysql-community-server``  
可能会碰到GPG密钥验证不通过，
> GPG key retrieval failed: [Errno 14] curl#37 - "Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql-2022"

不要慌，可通过URL加载导入  
[root@localhost liao]# ``rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022``

看到"Complete!"表示安装完成。
检查是否安装成功  
先启动mySQL  
[root@localhost liao]# ``systemctl start mysqld.service``  
然后查看mySQL运行状态  
[root@localhost liao]# ``systemctl status mysqld.service``  
看到mySQL运行信息即表示安装成功，Active状态：启动服务后为active (running)、停止后为inactive (dead)

## 5 登录mySQL   
设置开机启动MySQL  
[root@localhost liao]# ``systemctl enable mysqld``  

查看密码  
[root@localhost liao]# ``grep "password" /var/log/mysqld.log``

登录  
[root@localhost liao]# ``mysql -u root -p``

修改密码  
mysql> ``ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';``
> 默认策略默认要求：长度8位，且必须含有数字，小写或大写字母，特殊字符。
> 具体的要求可用下面命令查看
mysql> ``show VARIABLES LIKE 'validate_password%';``

> 若想设置简单密码，需要关闭密码策略  
mysql> ``set global validate_password.policy=0;``

> validate_password_length代表密码长度，最小值为4  
mysql> ``set global validate_password.length=4;``

> 若不想每次执行yum操作的时候自动更新mySQL，把下面的包卸载掉  
[root@localhost ~]# ``yum -y remove mysql80-community-release-el7-2.noarch``

## 6 设置外网访问mySQL  
打开mySQL的配置文件**my.cnf**，增加如下配置  
``sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION``  

查询host值  
mysql> ``select user, host from user;``

查询端口
mysql> ``show global variables like 'port';``

然后一顿操作  
mysql> ``use mysql;``  
mysql> ``update user set host="%" where user='root';``(本地：'root'@'localhost'；远程：'root'@'%')  
mysql> ``flush privileges;``  

允许外网连接  
mysql> ``ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root1234.';``
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

mySQL配置说明
1. /etc/my.cnf 这是mysql的主配置文件
2. /var/lib/mysql mysql数据库的数据库文件存放位置
3. /var/log mysql数据库的日志输出存放位置
4. service mysqld start #启动
5. service mysqld restart #重启
6. service mysqld stop # 停掉

避坑：  
前述配置好之后，Windows还是连不上，因为Windows telnet默认是关闭的；打开“控制面板-程序和功能-打开或关闭windows功能”，启用“Telnet服务器”

## 7 卸载mySQL（不保留数据）
1. 查找已经安装的包  
[root@localhost ~]# ``mysql. rpm -qa | grep -i mysql``
> mysql80-community-release-el7-6.noarch.rpm

2. 删除  
[root@localhost ~]# ``yum -y remove mysql80-*``

3. 找出mySQL相关的目录  
[root@localhost ~]# ``find / -name mysql``

4. 根据查找结果逐一删除  
例如
[root@localhost ~]# ``rm -rf /etc/my.cnf``  
[root@localhost ~]# ``rm -rf /root/.mysql_sercret``

