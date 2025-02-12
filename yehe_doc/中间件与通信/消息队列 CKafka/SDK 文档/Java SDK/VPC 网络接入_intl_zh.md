## 操作场景

该任务以 Java 客户端为例指导您使用 VPC 网络接入消息队列 CKafka 并收发消息。

## 前提条件

- [安装1.8或以上版本 JDK](https://www.oracle.com/java/technologies/javase-downloads.html)
- [安装2.5或以上版本 Maven](http://maven.apache.org/download.cgi#)
- [下载 Demo](https://github.com/TencentCloud/ckafka-sdk-demo/tree/main/javakafkademo/VPC)

## 操作步骤

### 步骤1：准备配置

1. 将下载的 Demo 中的 javakafkademo 上传至 Linux 服务器。
2. 登录 Linux 服务器，进入 javakafkademo 目录，并配置相关参数。
    2.在 pom.xml 中添加以下依赖。 
    <dx-codeblock>
    :::  xml
      <dependency>
          <groupId>org.apache.kafka</groupId>
          <artifactId>kafka-clients</artifactId>
          <version>0.10.2.2</version>
      </dependency>
    :::
    </dx-codeblock>
   2.创建消息队列 CKafka 配置文件 kafka.properties。
    <dx-codeblock>
    :::  bash
      ## 配置接入网络，在控制台的实例详情页面接入方式模块的网络列复制。
      bootstrap.servers=xx.xx.xx.xx:xxxx
      ## 配置Topic，在控制台上topic管理页面复制。
      topic=XXX
      ## 配置Consumer Group，您可以自定义设置
      group.id=XXX
    :::
    </dx-codeblock>
<table>
    <thead>
    <tr>
        <th>参数</th>
        <th>说明</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>bootstrap.servers</td>
        <td>接入网络，在控制台的实例详情页面<strong>接入方式</strong>模块的网络列复制。<br><img
                src="https://main.qcloudimg.com/raw/6b12eca18662d26a334d55b743c825ef.png" referrerpolicy="no-referrer">
        </td>
    </tr>
    <tr>
        <td>topic</td>
        <td>Topic 名称，您可以在控制台上 <strong>topic 管理</strong>页面复制。<br><img
                src="https://main.qcloudimg.com/raw/1b34ab83490f228ba0683609e0202c54.png" referrerpolicy="no-referrer">
        </td>
    </tr>
    <tr>
        <td>group.id</td>
        <td>您可以自定义设置，demo 运行成功后可以在 <strong>Consumer Group</strong> 页面看到该消费者。</td>
    </tr>
    </tbody>
</table>
3. 创建配置文件加载程序 CKafkaConfigurer.java。 
<dx-codeblock>
:::  java
      public class CKafkaConfigurer {
   
          private static Properties properties;
          
          public synchronized static Properties getCKafkaProperties() {
              if (null != properties) {
                  return properties;
              }
              //获取配置文件kafka.properties的内容。
              Properties kafkaProperties = new Properties();
              try {
                  kafkaProperties.load(CKafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
              } catch (Exception e) {
                  System.out.println("getCKafkaProperties error");
              }
              properties = kafkaProperties;
              return kafkaProperties;
          }
      }  
:::
</dx-codeblock>


### 步骤2：发送消息

1. 编写生产消息程序 CKafkaProducerDemo.java。
<dx-codeblock>
:::  java
public class CKafkaProducerDemo {

    public static void main(String args[]) {
        //加载kafka.properties。
        Properties kafkaProperties = CKafkaConfigurer.getCKafkaProperties();

        Properties properties = new Properties();
        //设置接入点，请通过控制台获取对应Topic的接入点。
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
        
        //消息队列Kafka版消息的序列化方式, 此处demo 使用的是StringSerializer。
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringSerializer");
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringSerializer");
        //请求的最长等待时间。
        properties.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
        //设置客户端内部重试次数。
        properties.put(ProducerConfig.RETRIES_CONFIG, 5);
        //设置客户端内部重试间隔。
        properties.put(ProducerConfig.RECONNECT_BACKOFF_MS_CONFIG, 3000);
        //构造Producer对象。
        KafkaProducer<String, String> producer = new KafkaProducer<>(properties);
        
        //构造一个消息队列Kafka版消息。
        String topic = kafkaProperties.getProperty("topic"); //消息所属的Topic，请在控制台申请之后，填写在这里。
        String value = "this is ckafka msg value"; //消息的内容。
        
        try {
            //批量获取Future对象可以加快速度, 但注意, 批量不要太大。
            List<Future<RecordMetadata>> futureList = new ArrayList<>(128);
            for (int i = 0; i < 10; i++) {
                //发送消息，并获得一个Future对象。
                ProducerRecord<String, String> kafkaMsg = new ProducerRecord<>(topic,
                        value + ": " + i);
                Future<RecordMetadata> metadataFuture = producer.send(kafkaMsg);
                futureList.add(metadataFuture);
        
            }
            producer.flush();
            for (Future<RecordMetadata> future : futureList) {
                //同步获得Future对象的结果。
                RecordMetadata recordMetadata = future.get();
                System.out.println("produce send ok: " + recordMetadata.toString());
            }
        } catch (Exception e) {
            //客户端内部重试之后，仍然发送失败，业务要应对此类错误。
            System.out.println("error occurred");
        }
    }
}
:::
</dx-codeblock>
2. 编译并运行 CKafkaProducerDemo.java 发送消息。
3. 运行结果。
<dx-codeblock>
:::  bash
Produce ok:ckafka-topic-demo-0@198
Produce ok:ckafka-topic-demo-0@199
:::
</dx-codeblock>
4. 在 [CKafka 控制台](https://console.intl.cloud.tencent.com/ckafka) 的 **topic 管理**页面，选择对应的 Topic ，单击**更多** > **消息查询**，查看刚刚发送的消息。
    ![](https://main.qcloudimg.com/raw/417974c1d8df4a5ff409138e7c6b3def.png)


### 步骤3：消费消息

1. 创建 Consumer 订阅消息程序 CKafkaConsumerDemo.java。
<dx-codeblock>
:::  java
public class CKafkaConsumerDemo {

    public static void main(String args[]) {
        //加载kafka.properties。
        Properties kafkaProperties = CKafkaConfigurer.getCKafkaProperties();

        Properties props = new Properties();
        //设置接入点，请通过控制台获取对应Topic的接入点。
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
        //两次Poll之间的最大允许间隔。
        //消费者超过该值没有返回心跳，服务端判断消费者处于非存活状态，服务端将消费者从Consumer Group移除并触发Rebalance，默认30s。
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
        //每次Poll的最大数量。
        //注意该值不要改得太大，如果Poll太多数据，而不能在下次Poll之前消费完，则会触发一次负载均衡，产生卡顿。
        props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
        //消息的反序列化方式。
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG,
                "org.apache.kafka.common.serialization.StringDeserializer");
        //属于同一个组的消费实例，会负载消费消息。
        props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
        //构造消费对象，也即生成一个消费实例。
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
        //设置消费组订阅的Topic，可以订阅多个。
        //如果GROUP_ID_CONFIG是一样，则订阅的Topic也建议设置成一样。
        List<String> subscribedTopics = new ArrayList<>();
        //如果需要订阅多个Topic，则在这里添加进去即可。
        //每个Topic需要先在控制台进行创建。
        String topicStr = kafkaProperties.getProperty("topic");
        String[] topics = topicStr.split(",");
        for (String topic : topics) {
            subscribedTopics.add(topic.trim());
        }
        consumer.subscribe(subscribedTopics);
        
        //循环消费消息。
        while (true) {
            try {
                ConsumerRecords<String, String> records = consumer.poll(1000);
                //必须在下次Poll之前消费完这些数据, 且总耗时不得超过SESSION_TIMEOUT_MS_CONFIG。
                //建议开一个单独的线程池来消费消息，然后异步返回结果。
                for (ConsumerRecord<String, String> record : records) {
                    System.out.println(
                            String.format("Consume partition:%d offset:%d", record.partition(), record.offset()));
                }
            } catch (Exception e) {
                System.out.println("consumer error!");
            }
        }
    }
}
:::
</dx-codeblock>
2. 编译并运行 CKafkaConsumerDemo.java 消费消息。
3. 运行结果。
<dx-codeblock>
:::  bash
Consume partition:0 offset:298
Consume partition:0 offset:299
:::
</dx-codeblock>
4. 在  [CKafka 控制台](https://console.intl.cloud.tencent.com/ckafka) 的**Consumer Group**页面，选择对应的消费组名称，在主题名称输入 Topic 名称，单击**查询详情**，查看消费详情。
    ![](https://main.qcloudimg.com/raw/22b1e4dd27a79cb96c76f01f2aa7e212.png)
