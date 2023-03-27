# Mac常用的环境变量配置

## ll
vim ~/.zshrc
alias ll="ls -l"

## 终端颜色
 > vim ～/.zshrc
 > export CLICOLOR=1
 > export LSCOLORS=ExGxFxdaCxDaDahbadeche
- LSCOLORS=后，共22个字母，每个字母对应一种颜色。每组2个字母，第一个字母为前景色，第二个字母为背景色；共11组，每一组对应一种文件类型。
11组文件类型的意思如下：
 > 1. directory 
   2. symbolic link 
   3. socket 
   4. pipe 
   5. executable (可执行文件，x权限) 
   6. block special 
   7. character special 
   8. executable with setuid bit set (setuid=Set User ID，属主身份) 
   9. executable without setgid bit set 
   10. directory writable to others, with sticky bit 
   11. directory writable to others, without sticky bit

- LSCOLORS中，各个字母代表的颜色如下，注意大小写是有区别的：  
 > a 黑色  
   b 红色 	 代表压缩文件或者压缩包  
   c 绿色	  代表可执行文件  
   d 棕色   代表块文件  
   e 蓝色 	 代表目录  
   f 洋红色  
   g 青色 	 代表链接  
   h 浅灰色  
   A 黑色粗体  
   B 红色粗体  
   C 绿色粗体  
   D 棕色粗体  
   E 蓝色粗体  
   F 洋红色粗体  
   G 青色粗体  
   H 浅灰色粗体  
   x 系统默认颜色 

## JAVA
 - export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_361.jdk/Contents/Home
   export PATH=$JAVA_HOME/bin:$PATH:.
   export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.

## Maven
 - export MAVEN_HOME=/usr/local/maven-3.9.0
   export PATH=$PATH:$MAVEN_HOME/bin

## Scala
 - export SCALA_HOME=$HOME/.scala/scala-2.11.12
   export PATH=$PATH:$SCALA_HOME/bin

