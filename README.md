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
Creation of a virtual machine (type `e2-medium`) in **Google Compute Engine** to host ElasticSearch and Kibana. Configuration steps included:

- Selecting Ubuntu as the operating system  
- Enabling HTTP/HTTPS traffic  
- Opening ports `9200` (ElasticSearch) and `5601` (Kibana)  
- Assigning a static external IP

📸 *Insert image of the created virtual machine*  
```markdown
![ElasticSearch VM](path/to/elasticsearch-vm-image.png)
```

---

### Installation of ElasticSearch and Kibana  
Installation of both services using official `.deb` packages. Access to the VM was done via SSH:

```bash
# Download and install ElasticSearch
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.10.2-amd64.deb
sudo dpkg -i elasticsearch-7.10.2-amd64.deb

# Download and install Kibana
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.10.2-amd64.deb
sudo dpkg -i kibana-7.10.2-amd64.deb
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

📸 *Insert screenshot of running services or connection test*  
```markdown
![ElasticSearch & Kibana running](path/to/services-status-image.png)
```

