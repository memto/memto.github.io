---
layout: post
ads: true
comments: true
published: false
title: Cassandra DB on Ubuntu
---
#### Ref
https://linuxize.com/post/how-to-install-apache-cassandra-on-ubuntu-18-04/
https://www.guru99.com/cassandra-security.html

#### Notes
- On my system, I need java 11/above for some app/service. However, it seems Cassandra only run stable with java 8. These are java version on my system.

```bash
sudo update-alternatives --config java
There are 3 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode
  1            /mnt/2tb_ext4/installed/java/current/bin/java    1         manual mode
  2            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
  3            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode
```

- Run with this default will cause some error like below:

```bash
$ nodetool  status
ERROR 13:39:56,957 Cannot initialize un-mmaper.  (Are you using a non-Oracle JVM?)  Compacted data files will not be removed promptly.  Consider using an Oracle JVM or using standard disk access mode
java.lang.NoSuchMethodError: 'sun.misc.Cleaner sun.nio.ch.DirectBuffer.cleaner()'
	at org.apache.cassandra.io.util.FileUtils.<clinit>(FileUtils.java:75) ~[apache-cassandra-3.11.10.jar:3.11.10]
	at org.apache.cassandra.utils.FBUtilities.getToolsOutputDirectory(FBUtilities.java:880) ~[apache-cassandra-3.11.10.jar:3.11.10]
	at org.apache.cassandra.tools.NodeTool.printHistory(NodeTool.java:216) ~[apache-cassandra-3.11.10.jar:3.11.10]
	at org.apache.cassandra.tools.NodeTool.execute(NodeTool.java:184) ~[apache-cassandra-3.11.10.jar:3.11.10]
	at org.apache.cassandra.tools.NodeTool.main(NodeTool.java:56) ~[apache-cassandra-3.11.10.jar:3.11.10]
error: null
-- StackTrace --
java.lang.NullPointerException
	at org.apache.cassandra.config.DatabaseDescriptor.getDiskFailurePolicy(DatabaseDescriptor.java:1995)
	at org.apache.cassandra.utils.JVMStabilityInspector.inspectThrowable(JVMStabilityInspector.java:102)
	at org.apache.cassandra.utils.JVMStabilityInspector.inspectThrowable(JVMStabilityInspector.java:60)
	at org.apache.cassandra.io.util.FileUtils.<clinit>(FileUtils.java:81)
	at org.apache.cassandra.utils.FBUtilities.getToolsOutputDirectory(FBUtilities.java:880)
	at org.apache.cassandra.tools.NodeTool.printHistory(NodeTool.java:216)
	at org.apache.cassandra.tools.NodeTool.execute(NodeTool.java:184)
	at org.apache.cassandra.tools.NodeTool.main(NodeTool.java:56)


$ cqlsh
Connection error: ('Unable to connect to any servers', {'127.0.0.1': error(111, "Tried connecting to [('127.0.0.1', 9042)]. Last error: Connection refused")})

$ cassandra
[0.001s][warning][gc] -Xloggc is deprecated. Will use -Xlog:gc:/var/log/cassandra/gc.log instead.
intx ThreadPriorityPolicy=42 is outside the allowed range [ 0 ... 1 ]
Improperly specified VM option 'ThreadPriorityPolicy=42'
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
```
