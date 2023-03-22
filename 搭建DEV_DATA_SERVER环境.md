# 搭建DEV_DATA_SERVER环境
  服务器(DEV_DATA_SERVER_16.163.6.178)：
  - Java，Scale，Python，Aws cli，Maven

  服务器(dev_Data1_16.163.219.20, dev_Data2_18.162.200.100, dev_Data3_18.162.187.168)：
  - Kafka，Zookeeper

  本地电脑：
  - Java，Scale，Python，Maven

## 1. 安装JDK8
先卸载旧版
1) 查看CentOS自带JDK是否已安装
 - yum list installed | grep java
2) 假使存在自带的jdk，删除centos自带的JDK
 - sudo yum -y remove java-11-openjdk*
   sudo yum -y remove tzdata-java.noarch
结果显示为Complete!表示卸载完成!

Fedora, Oracle Linux, Red Hat Enterprise Linux, etc.
 - sudo yum install java-1.8.0-openjdk
   sudo yum install java-1.8.0-openjdk-devel

配置环境变量
sudo vim /etc/profile
追加下列代码：
```
 export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.362.b08-1.amzn2.0.1.x86_64
 export PATH=$JAVA_HOME/bin:$PATH
 export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JAVA_HOME/jre/lib/rt.jar
 ```

source /etc/profile
检查是否安装成功：
 - java -version
 - javac -version


## 2. 安装Scala2.11.12
### 2.1 创建～/.scala/文件夹，并下载安装包
 - mkdir .scala 
   cd .scala
   wget https://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.tgz
   wget https://downloads.lightbend.com/scala/2.11.12/scala-2.11.12.tgz

### 2.2 解压，并配置环境变量
 - tar -zxvf scala-2.11.12.tgz
   sudo vim /etc/profile
   追加下列代码：
   ``` 
    export SCALA_HOME=$HOME/.scala/scala-2.11.12
    export PATH=$PATH:$SCALA_HOME/bin
   ```
 - source /etc/profile

### 2.3 检查是否安装成功
 - scala -version

### 2.4 安装配置sbt
 - sudo rm -f /etc/yum.repos.d/bintray-rpm.repo || true
 - curl -L https://www.scala-sbt.org/sbt-rpm.repo > sbt-rpm.repo
 - sudo mv sbt-rpm.repo /etc/yum.repos.d/
 - sudo yum install sbt


## 3. 编译安装Python3.7.8
（**如果yum安装的可用，则使用yum安装的python3**，跳过此步骤；否则编译安装）
### 3.1 安装所需依赖
 - sudo yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel libffi-devel

### 3.2 下载python3.7.8源码包
 - wget https://www.python.org/ftp/python/3.7.8/Python-3.7.8.tgz

### 3.3 编译安装
 - tar -xzvf Python-3.7.8.tgz
 - cd Python-3.7.8
 - sudo mkdir /usr/local/python3.7.8
 - ./configure --prefix=/usr/local/python3.7.8
 - make
 - sudo make install

### 3.4 建立软链接
 - ln -s /usr/local/python3/bin/python3 /usr/bin/python3
   ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3


## 4. 安装AWS-cli
 - curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
 - unzip awscliv2.zip
 - sudo ./aws/install
 - aws --version
详情可参考官方文档：
https://docs.aws.amazon.com/zh_cn/cli/latest/userguide/cli-chap-welcome.html


## 5. 安装配置Maven
### 5.1 到官网下载合适的版本
官网地址：https://maven.apache.org/download.cgi
下载链接：https://dlcdn.apache.org/maven/maven-3/3.9.0/binaries/apache-maven-3.9.0-bin.tar.gz

### 5.2 解压
 - unzip apache-maven-3.9.0-bin.zip
or
 - tar xzvf apache-maven-3.9.0-bin.tar.gz

### 5.3 部署和配置环境变量
把解压出来的Maven包放到/usr/local/下
vim ~/.bash.profile，追加下列代码：
```
export MAVEN_HOME=/usr/local/maven-3.9.0
export PATH=$PATH:$MAVEN_HOME/bin
```

### 5.4 查看安装结果
在终端输入：mvn -version

### 5.5 配置Maven仓库
在Maven包目录下打开conf->settings.xml；
找到<localRepository>/path/to/local/repo</localRepository>，复制一条到注释区外，改为需要放置依赖包的目录；
找到<mirrors>，在其范围内的非注释区，添加下列代码(注意格式)：
```
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>https://maven.aliyun.com/repository/public/</url>
      <mirrorOf>public</mirrorOf>
     </mirror>
```

### 5.6 如果还有Maven相关报错
请参考https://developer.aliyun.com/mvn/guide


## 6. Kafka和Zookeeper配置信息
安装了Kafka 的机器：
 - IGC_DEV_DATA_1 (16.163.219.20，172.31.23.9，node1)
   IGC_DEV_DATA_2 (18.162.200.100，172.31.27.225，node2)
   IGC_DEV_DATA_3 (18.162.187.168，172.31.18.245，node3)

Kafka installation location:
 - /usr/local/kafka_2.13-3.3.2/

Zookeeper:
 - /usr/local/zookeeper-3.4.6/

IGC_DEV_DATA_1 上：
 - ～/NOTES：
   - Zookeeper_Kafka_Install.txt: 安装的笔记。 
     zookeeper集群脚本.txt: Sam 的 Zookeeper 集群启动笔记。


## 7. 配置Git
生成新SSH密钥
1. 打开终端。
2. 粘贴下面的文本（替换为您的 GitHub 电子邮件地址），不设密码的话，一路回车就行。
 - $ ssh-keygen -t ed25519 -C "your_email@example.com"
 - $ ssh-keygen -t ed25519 -C "huiren.liao@igcexpert.com"

将 SSH 密钥添加到 ssh-agent
1. 在后台启动 ssh 代理。
 - $ eval "$(ssh-agent -s)"
2. 将 SSH 私钥添加到 ssh-agent。 如果使用其他名称创建了密钥或要添加具有其他名称的现有密钥，请将命令中的 id_ed25519 替换为私钥文件的名称。
 - $ ssh-add ~/.ssh/id_ed25519

将 SSH 密钥添加到 GitHub 上的帐户
1. 将 SSH 公钥复制到剪贴板。
 - 如果您的 SSH 公钥文件与示例代码不同，请修改文件名以匹配您当前的设置。 在复制密钥时，请勿添加任何新行或空格。
 - $ cat ~/.ssh/id_ed25519.pub
2. 登录Github，打开Settings-->SSH 和 GPG 密钥，单击“新建 SSH 密钥”或“添加 SSH 密钥” 。
3. 在 "Title"（标题）字段中，为新密钥添加描述性标签。选择密钥类型（身份验证或签名）为Authentication Key。
4. 将前面复制的公钥粘贴到“密钥”字段。
5. 单击“添加 SSH 密钥”。可以愉快地使用ssh推拉代码了。
