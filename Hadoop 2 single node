# Install Java
sudo apt-get update
sudo apt install default-jdk -y
java -version

#Create a Hadoop user for accessing HDFS and MapReduce
sudo addgroup hadoop
sudo adduser hduser --ingroup hadoop 
sudo adduser hduser sudo
sudo su hduser

#Install SSH
sudo apt-get install openssh-server -y

#Configure SSH
ssh-keygen
cd .ssh
cat id_rsa.pub >> authorized_keys
ssh localhost

#Disable IPV6
sudo nano /etc/sysctl.conf
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
sudo sysctl -p

#Download Hadoop
wget -c https://downloads.apache.org/hadoop/common/stable2/hadoop-2.10.1.tar.gz

#Extract and Install Hadoop tar ball
tar -xzvf hadoop-2.10.1.tar.gz
sudo mv hadoop-2.10.1 /usr/local/hadoop
sudo chown hduser:hadoop -R /usr/local/hadoop

# Set Enviornment Variable
readlink -f $(which java)
nano ~/.bashrc

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export PATH=$PATH:/usr/local/hadoop/bin/
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop

source ~/.bashrc

cd /usr/local/hadoop/etc/hadoop/

#Update hadoop-env.sh
nano hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_LOG_DIR=/var/log/hadoop
sudo mkdir /var/log/hadoop
sudo chown hduser:hadoop -R /var/log/hadoop

#Update core-site.xml
nano core-site.xml
<property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
 </property>
 <property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:54310</value>
</property>

sudo mkdir -p /app/hadoop/tmp
sudo chown hduser:hadoop /app/hadoop/tmp


#Update mapred-site.xml
cp mapred-site.xml.template mapred-site.xml
nano mapred-site.xml
<property>
  <name>mapreduce.jobtracker.address</name>
  <value>localhost:54311</value>
   </property>
<property>
   <name>mapreduce.framework.name</name>
   <value>yarn</value>
 </property>


#Update hdfs-site.xml
sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
sudo chown -R hduser:hadoop /usr/local/hadoop_store
nano hdfs-site.xml
<property> 
<name>dfs.replication</name>
  <value>1</value>
 </property>
 <property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
 </property>
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
 </property>


#Update yarn-site.xml
nano yarn-site.xml
<property>
      <name>yarn.nodemanager.aux-services</name>
      <value>mapreduce_shuffle</value>
   </property>


#Format Namenode
hdfs namenode -format

start-all.sh 
or start-dfs.sh
start-yarn.sh


jps



hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/ubuntu
hdfs dfs -put --- /user/ubuntu

hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-*examples*.jar pi 5 10




<----- Troubleshooting ----->
# if you are getting >>>  ..WARN.util.NativeCodeLoader: Unable to load native-hadoop library
Edit the bashrc file
nano ~/.bashrc
export HADOOP_HOME_WARN_SUPPRESS=1
export HADOOP_ROOT_LOGGER="WARN,DRFA"
