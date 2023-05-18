# 在Windows的idea中连接Ubuntu的hbase

## Hbase的web界面

HBase的Web界面是HBase Master和RegionServers的Web UI，它们提供了关于HBase集群状态、性能指标和其他详细信息的实时信息。

### HBase Master的web界面

可以通过访问`http://<master-ip>:<master-info-port>`来访问HBase Master的Web界面，其中`<master-ip>`是HBase Master所在的服务器的IP地址，`<master-info-port>`是HBase Master的信息端口。

要配置HBase的Web界面，需要在`hbase-site.xml`文件中设置一些属性。

```xml
<property>
  <name>hbase.master.info.port</name>
  <value>60010</value>
</property>
```

此属性将HBase Master的Web界面端口设置为60010。然后可以通过访问`http://<master-ip>:60010`来查看HBase Master的Web界面。

### RegionServers的web界面

设置RegionServers的Web界面端口，使用`hbase.regionserver.info.port`属性：

```xml
<property>
  <name>hbase.regionserver.info.port</name>
  <value>60030</value>
</property>
```

这将HBase RegionServers的Web界面端口设置为60030。您可以通过访问`http://<regionserver-ip>:60030`来查看各个RegionServer的Web界面，其中`<regionserver-ip>`是RegionServer所在的服务器的IP地址。

在完成这些配置后，确保HBase集群正在运行，然后尝试访问Web界面。