---
layout: post
ads: true
comments: true
published: true
title: Cassandra DB on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - cassandra
  - openjdk
---
#### Ref
- https://linuxize.com/post/how-to-install-apache-cassandra-on-ubuntu-18-04/
- https://www.guru99.com/cassandra-security.html

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

- Fix cassandra

```
# set JAVA_HOME in config file 
$ sudo vim /etc/default/cassandra
  # EXTRA_CLASSPATH provides the means to extend Cassandra's classpath with
  # additional libraries.  It is formatted as a colon-delimited list of
  # class directories and/or jar files.  For example, to enable the
  # JMX-to-web bridge install libmx4j-java and uncomment the following.
  #EXTRA_CLASSPATH="/usr/share/java/mx4j-tools.jar"

  JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

# reload service
sudo systemctl stop cassandra.service
sudo systemctl start cassandra.service
```

- Fix nodetool

```bash
# Run with JAVA_HOME
$ JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 nodetool status

# edit nodetool and hard-code JAVA
$ sudo vim /usr/bin/nodetool
  # Use JAVA_HOME if set, otherwise look for java in PATH
  if [ -x "$JAVA_HOME/bin/java" ]; then
      JAVA="$JAVA_HOME/bin/java"
  else
      JAVA="`which java`"
  fi

  JAVA=/usr/lib/jvm/java-8-openjdk-amd64/bin/java

  if [ "x$JAVA" = "x" ]; then
      echo "Java executable not found (hint: set JAVA_HOME)" >&2
      exit 1
  fi

  if [ -z "$CASSANDRA_CONF" -o -z "$CLASSPATH" ]; then
      echo "You must set the CASSANDRA_CONF and CLASSPATH vars" >&2
      exit 1
  fi
```

#### Configure Authentication and Authorization

```bash
$ sudo vim /etc/cassandra/cassandra.yaml

  # Authentication backend, implementing IAuthenticator; used to identify users
  # Out of the box, Cassandra provides org.apache.cassandra.auth.{AllowAllAuthenticator,
  # PasswordAuthenticator}.
  #
  # - AllowAllAuthenticator performs no checks - set it to disable authentication.
  # - PasswordAuthenticator relies on username/password pairs to authenticate
  #   users. It keeps usernames and hashed passwords in system_auth.roles table.
  #   Please increase system_auth keyspace replication factor if you use this authenticator.
  #   If using PasswordAuthenticator, CassandraRoleManager must also be used (see below)
  #authenticator: AllowAllAuthenticator
  authenticator: PasswordAuthenticator
  #authenticator: com.datastax.bdp.cassandra.auth.PasswordAuthenticator

  # Authorization backend, implementing IAuthorizer; used to limit access/provide permissions
  # Out of the box, Cassandra provides org.apache.cassandra.auth.{AllowAllAuthorizer,
  # CassandraAuthorizer}.
  #
  # - AllowAllAuthorizer allows any action to any user - set it to disable authorization.
  # - CassandraAuthorizer stores permissions in system_auth.role_permissions table. Please
  #   increase system_auth keyspace replication factor if you use this authorizer.
  #authorizer: AllowAllAuthorizer
  authorizer: CassandraAuthorizer
  #authorizer: com.datastax.bdp.cassandra.auth.CassandraAuthorizor 

# reload service
sudo systemctl stop cassandra.service
sudo systemctl start cassandra.service

# login and change password
$ cqlsh -u cassandra -p cassandra
  [cqlsh 5.0.1 | Cassandra 3.11.10 | CQL spec 3.4.4 | Native protocol v4]
  Use HELP for help.
  cassandra@cqlsh> alter user cassandra with password 'newpassword';
```

#### Configure data directory

- https://docs.datastax.com/en/dse/6.7/dse-admin/datastax_enterprise/config/configCassandra_yaml.html#Defaultdirectories

```bash
$ sudo vim /etc/cassandra/cassandra.yaml
```

### Query

- How to list all cassandra tables (https://stackoverflow.com/a/38698339)

  There are system tables which can provide information about stored keyspaces, tables, columns.
  Try run follows commands in cqlsh console:

  1. Get keyspaces info
    ```sql
    SELECT * FROM system.schema_keyspaces ;
    ```
  2. Get tables info
    ```sql
    SELECT columnfamily_name FROM system.schema_columnfamilies WHERE keyspace_name = 'keyspace name';
    ```
  3. Get table info
    ```sql
    SELECT column_name, type, validator FROM system.schema_columns WHERE keyspace_name = 'keyspace name' AND columnfamily_name = 'table name';
    ```

  Since v 5.0.x Docs
  1. Get keyspaces info
    ```sql
    SELECT * FROM system.schema_keyspaces ;
    ```
  2. Get tables info
    ```sql
    SELECT * FROM system_schema.tables WHERE keyspace_name = 'keyspace name';
    ```
  3. Get table info
    ```sql
    SELECT * FROM system_schema.columns
    WHERE keyspace_name = 'keyspace_name' AND table_name = 'table_name';
    ```

  Since v 6.0 Docs
  1. Get keyspaces info
    ```sql
    SELECT * FROM system.schema_keyspaces ;
    ```
  2. Get tables info
    ```sql
    SELECT * FROM system_schema.tables WHERE keyspace_name = 'keyspace name';
    ```
  3. Get table info
    ```sql
    SELECT * FROM system_schema.columns
    WHERE keyspace_name = 'keyspace_name' AND table_name = 'table_name';
    ```
    
- Prettifying results of cqlsh commands (https://stackoverflow.com/a/35898544)
	```sql
    EXPAND ON;
    ```
