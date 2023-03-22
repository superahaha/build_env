# Mac常用的环境变量配置

## ll
vim ~/.zshrc
alias ll="ls -l"

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

