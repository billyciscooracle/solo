## JMX 监控

JMX 监控我们采用 Jmxtrans ＋ InfluxDB 的方案。

依赖的配置项为：
- JMXTRANS_JMX_PORT：监听 JMX 端口；
- JMXTRANS_JMX_HOST: 监听 JMX 主机；
- JMXTRANS_INFLUXDB_URL: 写入 InfluxDB 的 URL；

配置参考：

```json
{
  "servers" : [ {
    "port" : "8081",
    "host" : "192.168.99.101",
    "queries" : [
      {
        "obj" : "java.lang:type=Memory",
        "attr" : [ "HeapMemoryUsage", "NonHeapMemoryUsage" ],
        "resultAlias":"jvmMemory",
        "outputWriters" : [ {
          "@class" : "com.googlecode.jmxtrans.model.output.InfluxDbWriterFactory",
          "url" : "http://192.168.1.186:8086/",
          "username" : "root",
          "password" : "root",
          "database" : "shurenyun"
        }]
      },
      {
        "obj" : "java.lang:type=Threading",
        "attr" : [ "ThreadCount", "PeakThreadCount", "TotalStartedThreadCount", "DaemonThreadCount"],
        "resultAlias":"jvmThreading",
        "outputWriters" : [ {
          "@class" : "com.googlecode.jmxtrans.model.output.InfluxDbWriterFactory",
          "url" : "http://192.168.1.186:8086/",
          "username" : "root",
          "password" : "root",
          "database" : "shurenyun"
        }]
      },
      {
        "obj" : "java.lang:type=OperatingSystem",
        "attr" : [ "SystemCpuLoad", "ProcessCpuLoad", "SystemLoadAverage"],
        "resultAlias":"jvmOS",
        "outputWriters" : [ {
          "@class" : "com.googlecode.jmxtrans.model.output.InfluxDbWriterFactory",
          "url" : "http://192.168.1.186:8086/",
          "username" : "root",
          "password" : "root",
          "database" : "shurenyun"
        }]
      }
    ]
  }]
}
```
