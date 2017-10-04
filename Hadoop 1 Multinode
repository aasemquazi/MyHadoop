-----*** upload .pem key to DC **----
$ scp -i abc.pem abc.pem ubuntu@<pub-ip>:/home/ubuntu/.ssh
>> do this for all nodes

$ ssh -i abc.pem <private-ip of any node>

-----*** Adding hosts to NN SNN JT and DN's ***-----
$ sudo nano /etc/hosts
172.31.23.8  ip-172-31-23-8.eu-west-1.compute.internal  nn
172.31.23.7  ip-172-31-23-7.eu-west-1.compute.internal  snn
172.31.23.10  ip-172-31-23-10.eu-west-1.compute.internal jt 
172.31.23.9  ip-172-31-23-9.eu-west-1.compute.internal 1dn
172.31.23.10  ip-172-31-23-10.eu-west-1.compute.internal 2dn
172.31.23.9  ip-172-31-23-9.eu-west-1.compute.internal 3dn

>> do this on all nodes 

-----*** Configure .profile ***-----
$ nano .profile
eval `ssh-agent` ssh-add /home/ubuntu/.ssh/abc.pem

$ source .profile

>>> scp .profile to each node

-----*** Install dsh ***-----
$ sudo apt-get update
$ sudo apt install dsh -y

>>>> Edit machines.list for dsh
$ sudo nano /etc/dsh/machines.list
#localhost
nn
snn
jt
1dn
2dn
3dn

$ dsh -a uptime

$ dsh -a sudo apt-get update


-----*** Install Java ***----
$ dsh -a sudo apt install openjdk-8-jdk -y
$ dsh -a java -version


-----*** Download Hadoop ***-----
$ dsh -a wget -c http://apache.mirror.gtcomm.net/hadoop/common/hadoop-1.2.1/hadoop-1.2.1.tar.gz

$ dsh -a tar -xzvf hadoop-1.2.1.tar.gz

$ dsh -a sudo mv hadoop-1.2.1 /usr/local/hadoop


-----*** Edit .bashrc ***-----

nano .bashrc
export HADOOP_PREFIX=/usr/local/hadoop/
export PATH=$PATH:$HADOOP_PREFIX/bin
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:$JAVA_HOME

>>> scp .bashrc to all nodes 
$ scp .bashrc ubuntu@snn:~

$ dsh -a source .bashrc

-----*** Configuring xml's ***-----

$ cd /usr/local/hadooop/conf

$ nano hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_OPTS=-Djava.net.preferIPV4Stack=true

$ nano core-site.xml
<property>
<name>fs.default.name</name>
<value>hdfs://nn:9000</value>
</property>
<property>
<name>hadoop.tmp.dir</name>
<value>/usr/local/hadoop/tmp</value>
</property>

$ nano hdfs-site.xml
<property>
<name>dfs.replication</name>
<value>3</value>
</property>
<property> 
<name>dfs.permissions</name> 
<value>false</value> 
</property>

$ nano mapred-site.xml
<property>
<name>mapred.job.tracker</name>
<value>hdfs://jt:9001</value>
</property>


-----*** Configure masters and slaves ***-----
$ nano masters
#localhost
snn

$ nano slaves
#localhost
1dn
2dn
3dn

>>> on SNN
$ ssh snn
nano /usr/local/hadoop/conf/masters
#localhost

>>> on JT
$ ssh jt
nano /usr/local/hadoop/conf/masters
#localhost
jt

-----*** scp configurations xml's and slaves to all nodes ***-----

$ cd /usr/local/hadoop/conf/

$ scp  hadoop-env.sh core-site.xml hdfs-site.xml mapred-site.xml slaves ubuntu@snn:/usr/local/hadoop/conf/
Same for all nodes

----*** Creating tmp directory (make sure you are on NN) ***-----
$ dsh -a mkdir /usr/local/hadoop/tmp

-----*** Exec bash ***-----
$ dsh -a exec bash


-----*** Formatting namenode(make sure you are on NN) ***-----
$ hadoop namenode -format



-----*** starting dfs daemons(from NN) ***----- 
$ start-dfs.sh

-----*** Starting mapred daemons (make sure you are on JT) ***-----
$ start-mapred.sh


-----*** Check java process ***-----
$ dsh -a jps
