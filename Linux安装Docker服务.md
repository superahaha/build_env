# Linux安装Docker服务
## 一、Docker及系统版本
Docker从17.03版本之后分为CE（Community Edition: 社区版）和EE（Enterprise Edition: 企业版）。相对于社区版本，企业版本强调安全性，但需付费使用。这里我们使用社区版本即可。

Docker支持64位版本的CentOS 7和CentOS 8及更高版本，它要求Linux内核版本不低于3.10。

查看Linux版本的命令这里推荐两种：`lsb_release -a`或`cat /etc/redhat-release`。

查看内核版本有三种方式：
`cat /proc/version`  
`uname -a`  
`uname -r`  

## 二、自动化安装Docker
新装系统可采用官方一键安装方式：
`curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun`  
或国内 daocloud一键安装命令：
`curl -sSL https://get.daocloud.io/docker | sh`

查看Docker版本：`docker -v`

## 三、手动安装Docker
1、卸载历史版本（按需选择）
`yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine \
                  docker-ce`

2、设置源仓库
2.1 设置仓库前，需安装所需的软件包。yum-utils提供了yum-config-manager，并且device mapper存储驱动程序需要device-mapper-persistent-data和lvm2。  
$ `sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2`  

2.2 上述安装完毕即可进行仓库的设置。使用官方源地址设置命令如下：
$ `sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo`  
通常，大陆访问官方的源地址比较慢，可将上述的源地址替换为国内比较快的地址：
清华大学源：https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
到此，仓库设置完毕，可进行Docker的安装了。

2.3 安装Docker
安装最新版本的 Docker Engine-Community 和 containerd：
`sudo yum install -y docker-ce docker-ce-cli containerd.io`

## 四、启动Docker
启动Docker的命令：
`sudo systemctl enable docker` #允许开机启动  
`sudo systemctl start docker` #启动Docker  
`sudo systemctl restart docker` #重启Docker  

可通过运行hello-world镜像来验证是否正确安装了Docker Engine-Community：  
// 拉取镜像  
`sudo docker pull hello-world`  
// 执行hello-world  
`sudo docker run hello-world`  

控制台显示有“Hello from Docker!”，则说明Docker安装和启动成功。

## 五、删除Docker
删除安装包：
`yum remove docker-ce`

删除镜像、容器、配置文件等内容：
`rm -rf /var/lib/docker`

## 六、Docker其他常见命令
守护进程重启：`systemctl daemon-reload`   
重启Docker服务：`systemctl restart docker / service docker restart`  
关闭Docker服务：`systemctl stop docker / service docker stop`  

搜索仓库镜像：`docker search [镜像名]`  
拉取镜像：`docker pull [镜像名]`  
查看镜像：`docker images`  
删除镜像：`docker rmi [image_id]`  

查看正在运行的容器：`docker ps`  
查看所有容器：`docker ps -a`  
启动（停止的）容器：`docker start [容器ID]`  
重启容器：`docker restart [容器ID]`  
停止容器：`docker stop [容器ID]`  
删除容器：`docker rm [container_id]` 
启动（新）容器：`docker run -it [ubuntu] /bin/bash`  
进入容器：`docker attach [容器ID]` 或 `docker exec -it [容器ID] /bin/bash`（推荐使用后者）  
查看容器日志：`docker logs <docker-container-name>`
更多的命令可以通过docker help命令来查看。


docker run \
  -u root \
  -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkinsci/blueocean
