# Kafka的topic
1. 安装
  - 到官网下载，然后解压进去：
    > $ tar -xzf kafka_2.13-3.4.0.tgz
    > $ cd kafka_2.13-3.4.0

2. 启动(已装好JDK8+、ZooKeeper)
  - 启动服务
    > $ bin/zookeeper-server-start.sh config/zookeeper.properties
  - 在另一个终端窗口执行
    > $ bin/kafka-server-start.sh config/server.properties

3. 创建一个名为test_topic的topic，该topic有3个分区，每个分区分配3个副本
  - 新开一个终端窗口执行
    > $ bin/kafka-topics.sh --create --topic test_topic --bootstrap-server node1:9092
  - 查询已知Kafka消息
    > $ bin/kafka-topics.sh --describe --topic test_topic --bootstrap-server node1:9092

4. 写消息到topic
  - 在一个终端写入下面的命令作为producer终端，发送的每一行消息都会写入topic
    > $ bin/kafka-console-producer.sh --topic test_topic --bootstrap-server node1:9092

5. 读取消息
  - 新开一个终端窗口执行下面命令
    > $ bin/kafka-console-consumer.sh --topic test_topic --from-beginning --bootstrap-server node1:9092

6. 在项目中使用Kafka
  - 编辑config/connect-standalone.properties文件，添加/修改plugin.path配置
   > echo "plugin.path=libs/connect-file-3.4.0.jar"
