# Prerequisites
To run this 4 machines are required. They need a dedicated network with at least 10Gb bandwidth in between members. For this POC AWS was using and c4.8xlarge instances

# Configure the environment
In the ServerConfigs project locate the environment.sh file and add the IP addresses of the server. The first address will be the primary locator, the second address the secondary locator and the last two addresses data servers. Server processes will also be started on the locator machine. It is assumed that there will be a directory called /home/ubuntu/cluster. This is the home directory all scripts will work from.

# Upload the files
By running the scp script in the ServerConfigs project a prompt will appear to determine which files should be uploaded:

```shell
Luke-Shannons-Macbook-Pro:ServerConfigs lshannon$ ./scp.sh 
What to upload?
1: Everything
2: scripts
3: conf
4: lib
5: data
6: Client App
```

Everything is all the dependancies, scripts is the shell scripts used to start and stop the Gemfire processes on each member, conf is the Gemfire configurations for each member, lib is where custom code Gemfire uses can be loaded (everything in here will be loaded in the CLASSPATH of Gemfire), data is the sample data for the POC and Client App is a sample application and shell scripts to start and stop it.

# Starting the Cluster
Once the environment is configured and files have been loaded, from a control machine the start_cluster script can be executed.
The following output is typical (this is starting a 6 member cluster, 2 locators and 4 servers, across 4 machines).
```shell
Start Time : 13:51:23
Start the locators
Created the ip-172-31-39-160-locator directory
Starting Up: ip-172-31-39-160-locator running on 172.31.39.160 on port 10334

(1) Executing - start locator --name=ip-172-31-39-160-locator --locators=172.31.39.161[10334] --bind-address=ip-172-31-39-160 --enable-cluster-configuration=false --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-locator --port=10334 --J=-Xms1g --J=-Xmx1g --log-level=error --J=-Dcom.sun.management.jmxremote --J=-Dcom.sun.management.jmxremote.port=15666 --J=-Dcom.sun.management.jmxremote.ssl=false --J=-Dcom.sun.management.jmxremote.authenticate=false --J=-Dcom.sun.management.jmxremote.local.only=false --J=-Djava.rmi.server.hostname=172.31.39.160

..............................
Locator in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-locator on ip-172-31-39-160.ec2.internal[10334] as ip-172-31-39-160-locator is currently online.
Process ID: 1865
Uptime: 15 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-locator/ip-172-31-39-160-locator.log
JVM Arguments: -Dgemfire.locators=172.31.39.161[10334] -Dgemfire.enable-cluster-configuration=false -Dgemfire.load-cluster-configuration-from-dir=false -Dgemfire.log-level=error -Xms1g -Xmx1g -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=15666 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.local.only=false -Djava.rmi.server.hostname=172.31.39.160 -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar

Successfully connected to: [host=ip-172-31-39-160.ec2.internal, port=1099]

Created the ip-172-31-39-161-locator directory
Starting Up: ip-172-31-39-161-locator running on 172.31.39.161 on port 10334

(1) Executing - start locator --name=ip-172-31-39-161-locator --locators=172.31.39.160[10334] --bind-address=ip-172-31-39-161 --enable-cluster-configuration=false --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-locator --port=10334 --J=-Xms1g --J=-Xmx1g --log-level=error --J=-Dcom.sun.management.jmxremote --J=-Dcom.sun.management.jmxremote.port=15666 --J=-Dcom.sun.management.jmxremote.ssl=false --J=-Dcom.sun.management.jmxremote.authenticate=false --J=-Dcom.sun.management.jmxremote.local.only=false --J=-Djava.rmi.server.hostname=172.31.39.161

.......
Locator in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-locator on ip-172-31-39-161.ec2.internal[10334] as ip-172-31-39-161-locator is currently online.
Process ID: 1927
Uptime: 4 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-locator/ip-172-31-39-161-locator.log
JVM Arguments: -Dgemfire.locators=172.31.39.160[10334] -Dgemfire.enable-cluster-configuration=false -Dgemfire.load-cluster-configuration-from-dir=false -Dgemfire.log-level=error -Xms1g -Xmx1g -Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port=15666 -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.local.only=false -Djava.rmi.server.hostname=172.31.39.161 -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar

Successfully connected to: [host=ip-172-31-39-160.ec2.internal, port=1099]

Start the servers
Created ip-172-31-39-160-server directory

(1) Executing - start server --name=ip-172-31-39-160-server --use-cluster-configuration=false --classpath=:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/*:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/* --bind-address=ip-172-31-39-160 --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-server --locators=172.31.39.160[10334],172.31.39.161[10334] --properties-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties --rebalance=true --J=-Xms31g --J=-Xmx31g --cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml

.....
Server in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-server on ip-172-31-39-160.ec2.internal[49937] as ip-172-31-39-160-server is currently online.
Process ID: 2196
Uptime: 3 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-server/ip-172-31-39-160-server.log
JVM Arguments: -DgemfirePropertyFile=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties -Dgemfire.bind-address=ip-172-31-39-160 -Dgemfire.cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml -Dgemfire.locators=172.31.39.160[10334],172.31.39.161[10334] -Dgemfire.use-cluster-configuration=false -Xms31g -Xmx31g -XX:OnOutOfMemoryError=kill -KILL %p -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar::/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-api-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/activation.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/pulse-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jansi-1.8.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-core-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-io-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-util-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/servlet-api-3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-core-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jline-1.0.S2-B.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-web-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-gemfire-1.5.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-annotations-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-beans-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/antlr.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-databind-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/ra.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-modeler-2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-core-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-servlet-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/agent-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-api-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jna-4.0.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/xom-1.2.9.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-commons-1.9.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfSecurityImpl.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfsh-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-webapp-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mail.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-expression-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-server-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-logging-1.1.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-http-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-security-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-support-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/snappy-java-1.0.4.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-aop-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-shell-1.0.0.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-tx-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-xml-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-remote.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-io-2.3.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j.jar:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/HelloGByeModel-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/DataLoaderFunction-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/gson-2.3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar

Created ip-172-31-39-161-server directory

(1) Executing - start server --name=ip-172-31-39-161-server --use-cluster-configuration=false --classpath=:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/*:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/* --bind-address=ip-172-31-39-161 --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-server --locators=172.31.39.160[10334],172.31.39.161[10334] --properties-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties --rebalance=true --J=-Xms31g --J=-Xmx31g --cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml

......
Server in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-server on ip-172-31-39-161.ec2.internal[40892] as ip-172-31-39-161-server is currently online.
Process ID: 2190
Uptime: 3 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-server/ip-172-31-39-161-server.log
JVM Arguments: -DgemfirePropertyFile=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties -Dgemfire.bind-address=ip-172-31-39-161 -Dgemfire.cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml -Dgemfire.locators=172.31.39.160[10334],172.31.39.161[10334] -Dgemfire.use-cluster-configuration=false -Xms31g -Xmx31g -XX:OnOutOfMemoryError=kill -KILL %p -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar::/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-api-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/activation.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/pulse-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jansi-1.8.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-core-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-io-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-util-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/servlet-api-3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-core-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jline-1.0.S2-B.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-web-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-gemfire-1.5.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-annotations-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-beans-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/antlr.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-databind-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/ra.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-modeler-2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-core-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-servlet-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/agent-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-api-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jna-4.0.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/xom-1.2.9.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-commons-1.9.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfSecurityImpl.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfsh-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-webapp-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mail.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-expression-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-server-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-logging-1.1.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-http-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-security-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-support-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/snappy-java-1.0.4.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-aop-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-shell-1.0.0.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-tx-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-xml-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-remote.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-io-2.3.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j.jar:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/HelloGByeModel-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/DataLoaderFunction-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/gson-2.3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar

Created ip-172-31-39-162-server directory

(1) Executing - start server --name=ip-172-31-39-162-server --use-cluster-configuration=false --classpath=:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/*:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/* --bind-address=ip-172-31-39-162 --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-162-server --locators=172.31.39.160[10334],172.31.39.161[10334] --properties-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties --rebalance=true --J=-Xms31g --J=-Xmx31g --cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml

......
Server in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-162-server on ip-172-31-39-162.ec2.internal[52074] as ip-172-31-39-162-server is currently online.
Process ID: 1863
Uptime: 3 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-162-server/ip-172-31-39-162-server.log
JVM Arguments: -DgemfirePropertyFile=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties -Dgemfire.bind-address=ip-172-31-39-162 -Dgemfire.cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml -Dgemfire.locators=172.31.39.160[10334],172.31.39.161[10334] -Dgemfire.use-cluster-configuration=false -Xms31g -Xmx31g -XX:OnOutOfMemoryError=kill -KILL %p -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar::/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-api-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/activation.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/pulse-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jansi-1.8.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-core-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-io-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-util-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/servlet-api-3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-core-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jline-1.0.S2-B.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-web-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-gemfire-1.5.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-annotations-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-beans-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/antlr.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-databind-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/ra.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-modeler-2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-core-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-servlet-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/agent-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-api-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jna-4.0.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/xom-1.2.9.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-commons-1.9.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfSecurityImpl.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfsh-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-webapp-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mail.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-expression-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-server-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-logging-1.1.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-http-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-security-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-support-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/snappy-java-1.0.4.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-aop-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-shell-1.0.0.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-tx-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-xml-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-remote.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-io-2.3.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j.jar:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/HelloGByeModel-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/DataLoaderFunction-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/gson-2.3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar

Created ip-172-31-39-163-server directory

(1) Executing - start server --name=ip-172-31-39-163-server --use-cluster-configuration=false --classpath=:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/*:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/* --bind-address=ip-172-31-39-163 --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-163-server --locators=172.31.39.160[10334],172.31.39.161[10334] --properties-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties --rebalance=true --J=-Xms31g --J=-Xmx31g --cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml

......
Server in /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-163-server on ip-172-31-39-163.ec2.internal[36679] as ip-172-31-39-163-server is currently online.
Process ID: 1862
Uptime: 3 seconds
GemFire Version: 8.1.0
Java Version: 1.8.0_45
Log File: /home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-163-server/ip-172-31-39-163-server.log
JVM Arguments: -DgemfirePropertyFile=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/gemfire.properties -Dgemfire.bind-address=ip-172-31-39-163 -Dgemfire.cache-xml-file=/home/ubuntu/cluster/gemfire-ubuntu-package/conf/cache.xml -Dgemfire.locators=172.31.39.160[10334],172.31.39.161[10334] -Dgemfire.use-cluster-configuration=false -Xms31g -Xmx31g -XX:OnOutOfMemoryError=kill -KILL %p -Dgemfire.launcher.registerSignalHandlers=true -Djava.awt.headless=true -Dsun.rmi.dgc.server.gcInterval=9223372036854775806
Class-Path: /home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar::/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-api-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/activation.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/pulse-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jansi-1.8.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-core-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-io-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-util-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/servlet-api-3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-core-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jline-1.0.S2-B.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-web-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-gemfire-1.5.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-annotations-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-beans-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/antlr.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jackson-databind-2.2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/ra.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-modeler-2.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/log4j-core-2.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-servlet-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/agent-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/locator-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-api-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gemfire-web.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jna-4.0.0.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/xom-1.2.9.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-data-commons-1.9.1.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfSecurityImpl.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/gfsh-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-webapp-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mail.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-expression-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-server-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-logging-1.1.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-http-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-security-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-support-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/snappy-java-1.0.4.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-aop-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-shell-1.0.0.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-tx-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/jetty-xml-9.2.3.v20140905.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j-remote.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/spring-context-3.2.12.RELEASE.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/commons-io-2.3.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/mx4j.jar:/usr/lib/jvm/java-8-oracle/lib/tools.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/:/home/ubuntu/cluster/gemfire-ubuntu-package/conf/*:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/HelloGByeModel-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/DataLoaderFunction-0.0.1-SNAPSHOT.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/lib/gson-2.3.1.jar:/home/ubuntu/cluster/gemfire-ubuntu-package/Pivotal_GemFire_810_b50625_Linux/lib/server-dependencies.jar

End Time : 13:53:33
Done!
```
Once the cluster is started, pulse can be accessed on the primary locator. This will give an overview of the cluster and how data is distributed. Example:
http://52.7.9.136:7070/pulse

# Using the client
The client application is uploaded to the second Locator/Server machine. Due to how this cluster is configured, the client must be on the same network as the members. To start the client, run the startSampleApp.sh command. It will generate a log and a txt file with the PID (this is used to shut the program down when stopping).
```shell
ubuntu@ip-172-31-39-161:~/cluster/gemfire-ubuntu-package/client$ ls
JavaClientSample-0.0.1-SNAPSHOT.jar  startSampleApp.sh
java-client-sample.log               stopSampleApp.sh
java-client-sample_pid.txt
```
The program can then be accessed with a URL similar to this:
http://52.7.17.211:8080/home
Load Data first, then use the Get Data link, otherwise there will be no data to get.

By viewing Pulse, the entries can be seen streaming into the cluster. The summary of data loading should look something like this. On the test environment 23 GB took about 4 minutes and 7 seconds. This was done with no tuning and not an ideal disk store for the persistent file updates.

```shell
Function returned: LoadingSummary [memberName=ip-172-31-39-160(ip-172-31-39-160-server:2196)<v2>:40637, startTime=1432058645301, endTime=1432058918333, totalSegments=100001, segmentsSkipped=75101, segmentsLoaded=24900]
Function returned: LoadingSummary [memberName=ip-172-31-39-163(ip-172-31-39-163-server:1862)<v5>:46968, startTime=1432058645256, endTime=1432058921403, totalSegments=100001, segmentsSkipped=74788, segmentsLoaded=25213]
Function returned: LoadingSummary [memberName=ip-172-31-39-161(ip-172-31-39-161-server:2190)<v3>:17976, startTime=1432058645355, endTime=1432058927782, totalSegments=100001, segmentsSkipped=75051, segmentsLoaded=24950]
Function returned: LoadingSummary [memberName=ip-172-31-39-162(ip-172-31-39-162-server:1863)<v4>:36367, startTime=1432058645300, endTime=1432058928238, totalSegments=100001, segmentsSkipped=75063, segmentsLoaded=24938]
```

# Accessing the machines
By running the ssh script from a target machine a prompt will be provided to access the machines.
```shell
Luke-Shannons-Macbook-Pro:ServerConfigs lshannon$ ./ssh.sh 
Which machine (just type the leading number ie:'1')?
1: Locator 1
2: Locator 2
3: Server 1
4: Server 2

```
By inputing one of the numbers, an SSH session will be started with that machine. This is useful for viewing log files, checking running processes and other OS level monitoring and admin.
```shell
Last login: Tue May 19 12:26:41 2015 from 135-23-188-178.cpe.pppoe.ca
ubuntu@ip-172-31-39-161:~$ ps -ef | grep java
ubuntu     1798   1781  0 17:49 pts/0    00:00:00 grep --color=auto java
ubuntu@ip-172-31-39-161:~$ 
```

# Stopping the Cluster
By running the stop_cluster.sh command on a control machine, the following output can be seen:
```shell
Stop the servers
Stop Commands Completed
Done!
Stop Commands Completed
Done!
Stop Commands Completed
Done!
Stop Commands Completed
Done!
Stop the locators

(1) Executing - stop locator --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-locator

...
ip-172-31-39-161-locator is stopped
Done!

(1) Executing - stop locator --dir=/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-locator

.......
ip-172-31-39-160-locator is stopped
Done!
```
This results in all the Gemfire processes shutting down, but their persistent files not being deleted.

# Clean Up
This will delete all the working directories. This is used for a fresh start. Note, this will not remove any running Java processes, the shutdown_cluster script needs to be used for that. Also, this will delete a persistent files resulting in data loss. To perform this run the clean_cluster script on a target machine.

```shell
Cleaning Up
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-163-server deleted
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-162-server deleted
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-server deleted
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-161-locator deleted
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-server deleted
/home/ubuntu/cluster/gemfire-ubuntu-package/members/ip-172-31-39-160-locator deleted
Done!
```

