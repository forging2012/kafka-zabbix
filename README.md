# kafka-zabbix
Consumer metrics from kafka, then send to zabbix.

Java app report metrics data to kafka, the this project receive kafka message, send metrics data to zabbix server.

This project use zabbix-api to auto create zabbix trapper item.

## Dependencies

https://github.com/hengyunabc/zabbix-api

https://github.com/hengyunabc/metrics-kafka

## Example

```java
public class KafkaZabbixSenderTest {

	public static void main(String[] args) throws InterruptedException {
		KafkaZabbixSender kafkaZabbixSender = new KafkaZabbixSender();

		int port = 10051;
		String host = "192.168.90.102";

		ZabbixSender zabbixSender = new ZabbixSender(host , port);
		String url = "http://192.168.90.102/zabbix/api_jsonrpc.php";
		ZabbixApi zabbixApi = new DefaultZabbixApi(url);
		zabbixApi.init();

		String apiVersion = zabbixApi.apiVersion();
		System.err.println("apiVersion:" + apiVersion);

		boolean login = zabbixApi.login("zabbix.dev", "goK0Loqua4Eipoe");

		System.err.println(login);


		String zookeeper = "192.168.90.147:2181,192.168.90.147:3181,192.168.90.147:4181/kafka";
		kafkaZabbixSender.setZookeeper(zookeeper );
		kafkaZabbixSender.setGroup("testGroup");
		kafkaZabbixSender.setTopic("test-kafka-reporter");

		kafkaZabbixSender.setZabbixSender(zabbixSender);
		kafkaZabbixSender.setZabbixApi(zabbixApi);

		kafkaZabbixSender.init();

		TimeUnit.SECONDS.sleep(5000);
	}
}
```

## Licence

Apache License V2
