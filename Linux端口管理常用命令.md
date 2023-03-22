# Linux端口管理常用命令

## 一、开启端口
1）vi /etc/sysconfig/iptables
2）-A INPUT -m state --state NEW -m tcp -p tcp --dport 端口号 -j ACCEPT
3）/etc/init.d/iptables restart  #立即生效
4) /usr/sbin/iptables -L -n  #查看已经打开的端口
5）netstat -talnp  #查看系统已经打开的端口
netstat -anp | grep [端口号]  #监控状态为 listen 表示已经被占用；如果是空白则表示端口未使用


## 二、防火墙
yum install firewalld  #安装firewalld

firewall-cmd --state  #查看防火墙的运行状态
systemctl status firewalld.service  #查看防火墙服务是否开启

systemctl start firewalld.service  #开启防火墙(重启服务器后又会重新关闭)
systemctl enable firewalld.service #设置防火墙开机自动启动
systemctl restart firewalld.service  #重启防火墙

firewall-cmd --get-default-zone  #查看 firewalld 服务当前所使用的区域

firewall-cmd --zone=public --add-port=80/tcp --permanent  
 · 命令含义：
 · –zone #指定区域
 · –add-port=80/tcp #添加端口，格式为：<端口号/协议>
 · –permanent #永久生效，没有此参数重启后失效

firewall-cmd --reload  #重新载入配置

firewall-cmd [--permanent] --list-port  加上 -- permanent ，可以列出永久开放的端口；不加则列出非永久开放的端口

systemctl stop firewalld.service  #关闭防火墙
systemctl disable firewalld  #禁用，禁止开机启动

添加或删除端口后，需要重启防火墙!!!但是每次加完端口后，重新载入配置，就不用重启。

## 三、关闭端口
1、通过杀掉进程的方法来关闭端口
每个端口都有一个守护进程，kill掉这个守护进程就可以了

第一步、用下面命令：
netstat -anp |grep 端口
找出占用这个端口的进程PID，

第二步、用下面命令
kill -9 [PID]
杀掉就行了

2、通过开启关闭服务的方法来开启/关闭端口
因为每个端口都有对应的服务，因此要关闭端口只要关闭相应的服务就可以了。

linux中开机自动启动的服务一般都存放在两个地方：

/etc/init.d/文件夹下的服务：
这个文件夹下的服务都可以通过运行相应的SCRIPT来启动或关闭。
例如：启动sendmail服务： ./sendmail start (打开了TCP 25端口)
关闭sendmail服务： ./sendmail stop （关闭TCP 25 端口）
查看sendmail服务当前状态： ./sendmail? status （查看服务是否运行）

/etc/xinetd.d/文件夹下的服务：
这个文件夹下的服务需要通过更改服务的配置文件，并重新启动xinetd才可以。
例如：要启动其中的auth服务，打开/etc/xinetd.d/auth配置文件，更改“disable=no”，保存退出。
运行/etc/rc.d/init.d/xinetd restart
要停止其中的auth服务，打开/etc/xinetd.d/auth配置文件，更改“disable=yes”，保存退出。
运行/etc/rc.d/init.d/xinetd restart

3、通过防火墙限制端口
以下介绍的方法在Linux命令下使用，很简便。
开端口为：
iptables -A INPUT -p $port -j ACCEPT
关把ACCEPT改为DROP即可，即：
iptables -A INPUT -p $port -j DROP
其中$port即为端口数字，iptables的具体用法可。
