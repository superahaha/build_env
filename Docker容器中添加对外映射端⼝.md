# Docker容器中添加对外映射端⼝

⼀般在运⾏容器时，我们都会通过参数 -p（使⽤⼤写的-P参数则会随机选择宿主机的⼀个端⼝进⾏ 映射）来指定宿主机和容器端⼝的映射，例如：
 > docker run -it -d --name [container_name] -p 8088:80 -p 3308:3306 [image_name]
上⾯的命令是将容器内的80端⼝映射到宿主机的8088端⼝，容器的3306端⼝映射到宿主机的3308 端⼝
参数说明
 > -d 表示后台运⾏容器
 > -t 为docker分配⼀个伪终端并绑定到容器的标准输⼊上
 > -i 是让容器的标准输⼊保持打开状态
 > -p 指定映射端⼝
在运⾏容器时指定映射端⼝运⾏后，如果想要添加新的端⼝映射，可以使⽤以下两种⽅式：

## ⽅式⼀、将现有的容器打包成镜像，然后使⽤新打包的镜像运⾏容器时重新指定要映射的端⼝
1. 停⽌现有容器
 > docker stop container_name

2. 把容器commit为镜像
 > docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
Options:
 > -a, --author string 作者
 > -m, --message string 提交信息

3. ⽤新镜像运⾏容器
 > docker run -it -d --name container_name -p p1:p1 -p p2:p2 new_image_name


## ⽅式⼆、修改容器配置⽂件
1. 查看全部容器信息，记下要修改容器的ID
 > docker ps -a

2. 查看容器的端⼝映射情况（在容器外执⾏）
 > docker port [容器ID] 或者 docker port [容器名称]

3. 如果要修改的容器在运⾏，先停掉
 > docker stop [容器ID]

4. 停⽌Docker服务
 > systemctl stop docker

5. 进⼊/var/lib/docker/containers ⽬录下找到与 要修改容器的ID 相同的⽬录

6. 修改 hostconﬁg.json 配置（注意格式）

7. 修改 conﬁg.v2.json 配置（注意格式）

8. 启动Docker
 > systemctl start docker

9.  启动容器
 > Docker start [容器ID]

10. 查看宿主机端⼝是否和容器内端⼝是否映射成功（在容器外执⾏）
 > docker port [容器ID] 或者 docker port [容器名称]
 > netstat -an | grep [宿主机的映射端⼝]
