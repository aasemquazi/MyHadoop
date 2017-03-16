*** Update system ***
$ apt-get update

*** Install java ***
$ sudo apt-get install openjdk-8-jdk -y

*** Configure SSH ***
$ ssh-keygen
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

*** Download Hadoop *** 
$ wget -c wget http://apache.mirror.gtcomm.net/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz

$ tar -xzvf hadoop-1.2.1.tar.gz

$ sudo mv hadoop-1.2.1 /usr/local/hadoop

*** Configure .bashrc for env var setup ***
nano ~/.bashrc
export HADOOP_PREFIX=/usr/local/hadoop/
export PATH=$PATH:$HADOOP_PREFIX/bin
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:$JAVA_HOME

*** Setting Hadoop env ***
nano /usr/local/hadoop/conf/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_OPTS=-Djava.net.preferIPV4Stack=true

*** Configuring xml's ***
nano /usr/local/hadoop/conf/core-site.xml
<property>
<name>fs.default.name</name>
<value>hdfs://localhost:9000</value>
</property>
<property>
<name>hadoop.tmp.dir</name>
<value>/usr/local/hadoop/tmp</value>
</property>

nano /usr/local/hadoop/conf/hdfs-site.xml
<property>
<name>dfs.replication</name>
<value>1</value>
</property>

sudo nano /usr/local/hadoop/conf/mapred-site.xml
<property>
<name>mapred.job.tracker</name>
<value>hdfs://localhost:9001</value>
</property>

*** Creating tmp directory ***
mkdir /usr/local/hadoop/tmp

*** execute bash ***
exec bash

*** formating hadoop namenode ***
hadoop namenode -format

*** Start hadoop daemons/services
start-all.sh

*** check process ***
jps
