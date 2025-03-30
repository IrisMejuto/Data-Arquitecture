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

### Bucket creation in Google Cloud Storage  
A bucket was created to store required JAR files for ElasticSearch integration.

### Uploading JAR files to the bucket  
Upload of the following JAR files into the bucket via GCP Console:

- `elasticsearch-hadoop-x.x.x.jar`
- Additional compatibility JARs for Hive integration

### Transferring JAR files to the cluster  
Files were copied from the bucket to the Hadoop cluster nodes using:

```bash
gsutil cp gs://bucket-elastic-pr/commons-httpclient-3.1.jar .

gsutil cp gs://bucket-elastic-pr/elasticsearch-hadoop-8.14.1.jar .
