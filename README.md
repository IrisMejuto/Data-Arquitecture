# Data-Arquitecture
Project focused on configuring and integrating a Hadoop cluster in Google Cloud Platform with an ElasticSearch server, enabling data indexing and visualization through Kibana.

---

## 📌 Table of Contents

1. [Hadoop Cluster Setup (GCP)](#1-hadoop-cluster-setup-gcp)  
2. [ElasticSearch Server Setup](#2-elasticsearch-server-setup)  
3. [Connecting Hadoop to ElasticSearch](#3-connecting-hadoop-to-elasticsearch)  
4. [Index Creation in ElasticSearch](#4-index-creation-in-elasticsearch)  
5. [Data Visualization in Kibana](#5-data-visualization-in-kibana)

---

## 1. Hadoop Cluster Setup (GCP)

### Cluster creation in Dataproc  
Setup of a Hadoop cluster using **Dataproc** on Google Cloud Platform. Configuration included number of nodes, Hadoop version, and network settings.

![Cluster](Images/1.1.png)

### Bucket creation in Google Cloud Storage  
A bucket was created to store required JAR files for ElasticSearch integration.

### Uploading JAR files to the bucket  
Upload of the following JAR files into the bucket:

- [elasticsearch-hadoop-x.x.x.jar](https://github.com/IrisMejuto/Data-Arquitecture/blob/main/files/elasticsearch-hadoop-8.14.1.zip)
- [commons-httpclient-3.1.jar](https://github.com/IrisMejuto/Data-Arquitecture/blob/main/files/commons-httpclient-3.1.jar)

![Files in the bucket](https://github.com/IrisMejuto/Data-Arquitecture/blob/main/Images/2.1.png)

### Transferring JAR files to the cluster  
Files were copied from the bucket to the Hadoop cluster nodes using:

```bash
gsutil cp gs://bucket-elastic-pr/commons-httpclient-3.1.jar .

gsutil cp gs://bucket-elastic-pr/elasticsearch-hadoop-8.14.1.jar .

```


## 2. ElasticSearch Server Setup

### Virtual Machine creation  
Creation of a virtual machine (type `e2-medium`) to host ElasticSearch and Kibana. Configuration steps included:

- Selecting Ubuntu as the operating system  
- Enabling HTTP/HTTPS traffic  
- Opening ports `9200` (ElasticSearch) and `5601` (Kibana)  
- Assigning a static external IP

![ElasticSearch VM](Images/2.1.1.png)

---

### Installation of ElasticSearch and Kibana  

Installation of required tools and packages on the virtual machine. Access to the VM was done via SSH:

#### Step 1: Install `wget`  
Installation of `wget` to enable downloading of installation packages:

```bash
sudo apt install wget
```

#### Step 2: Download latest versions of ElasticSearch and Kibana  

Download of `.deb` installation packages for ElasticSearch and Kibana (version 8.14.1):

```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.14.1-amd64.deb
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.14.1-amd64.deb
```

#### Step 3: Install ElasticSearch and Kibana  

Installation of the downloaded packages using `dpkg`:

```bash
sudo dpkg -i elasticsearch-8.14.1-amd64.deb
sudo dpkg -i kibana-8.14.1-amd64.deb
```

#### Step 4: Configure Kibana for external access  

The following line was appended to the Kibana configuration file to allow external access from any host:

```bash
sudo sed -i -e '$aserver.host: 0.0.0.0' /etc/kibana/kibana.yml
```

The configuration was verified with:

```bash
sudo cat /etc/kibana/kibana.yml
```

---

#### Step 5: Adjust ElasticSearch configuration  

ElasticSearch settings were updated manually by editing the main configuration file:

```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
```

The following parameters were set to `false` to disable security features for simplified local access (not recommended in production environments):

![Image False parameter](Images/2.2.1.png)

---

#### Step 6: Restart services  

After configuration, both services were restarted to apply changes:

```bash
sudo service elasticsearch restart
sudo service kibana restart
```



## 3. 
```bash
sudo sed -i '$d' /etc/hive/conf.dist/hive-site.xml

sudo sed -i '$a \  <property>\n    <name>es.nodes</name>\n    <value>35.234.149.191</value>\n  </property>\n' /etc/hive/conf.dist/hive-site.xml

sudo sed -i '$a \  <property>\n    <name>es.port</name>\n    <value>9200</value>\n  </property>\n' /etc/hive/conf.dist/hive-site.xml

sudo sed -i '$a \  <property>\n    <name>es.nodes.wan.only</name>\n    <value>true</value>\n  </property>\n' /etc/hive/conf.dist/hive-site.xml

sudo sed -i '$a \  <property>\n    <name>hive.aux.jars.path</name>\n   <value>/usr/lib/hive/lib/elasticsearch-hadoop-8.14.1.jar,/usr/lib/hive/lib/commons-httpclient-3.1.jar</value>\n  </property>\n</configuration>' /etc/hive/conf.dist/hive-site.xml

sudo cp elasticsearch-hadoop-8.14.1.jar /usr/lib/hive/lib/
sudo cp commons-httpclient-3.1.jar /usr/lib/hive/lib/
```

Enable and start both services:

```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch

sudo systemctl enable kibana
sudo systemctl start kibana
```

---

### Access configuration  
Configuration files were updated to allow external access:

**`/etc/elasticsearch/elasticsearch.yml`**

```yaml
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node
```

**`/etc/kibana/kibana.yml`**

```yaml
server.host: "0.0.0.0"
```

Restart both services after editing:

```bash
sudo systemctl restart elasticsearch
sudo systemctl restart kibana
```



