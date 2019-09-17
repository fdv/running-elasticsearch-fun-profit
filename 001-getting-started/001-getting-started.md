```
WIP, COVERS ELASTICSEARCH 5.5.x, UPDATING TO ES 6.5.x
```

# Getting Started with Elasticsearch

This chapter is for people who have not used Elasticsearch yet. It covers Elasticsearch basic concepts and guides you into deploying and using your first single node cluster. Every concept explained here are detailed further in this book.

In this introduction chapter you will learn:

- The basic concepts behind Elasticsearch
- What's an Elasticsearch cluster
- How to deploy your first, single node Elasticsearch cluster on the most common operating systems
- How to use Elasticsearch to index documents and find content
- Elasticsearch configuration basics
- What's an Elasticsearch plugin and how to use them

---

## Prerequisites

In order to read this book and perform the operations described along its chapters, you need:

- A machine or virtual machine running one of the popular Linux or Unix environments: Debian / Ubuntu, RHEL / CentOS or FreeBSD. Running Elasticsearch on Mac OS or Windows is not covered in this book
- A basic knowledge of UNIX command line and the use of a terminal
- Your favorite text editor

If you have never used Elasticsearch before, I recommend to create a virtual machine so you won't harm your main system in case of mistake. You can either run it locally using a virtualization tool like [Virtualbox](https://www.virtualbox.org/) or on your favorite cloud provider.

---

## Elasticsearch basic concepts

Elasticsearch is a distributed, scalable, fault tolerant open source search engine written in Java. It provides a powerful REST API both for adding or searching data and updating the configuration. Elasticsearch is led by Elastic, a company created by Shay Banon, who started the project on top of Lucene.

### REST APIs

A REST API is an application program interface (API) that uses HTTP requests to `GET`, `PUT`, `POST` and `DELETE` data. An API for a website is code that allows two software programs to communicate with each another. The API spells out the proper way for a developer to write a program requesting services from an operating system or other application. REST is the Web counterpart of databases CRUD (Create, Read, Update, Delete).

### Open Source

Open source means that Elasticsearch source code, the recipe to build the software, is public, free, and that anyone can contribute to the project by adding missing feature, documentation or fixing bugs. If accepted by the project, their work is then available to the whole community. Because Elasticsearch is open source, the company behind it can go bankrupt or stop maintaining the project without killing it. Someone else will be able to take over it and keep the project alive.

### Java

Java is a programming language created in 1995 by Sun Microsystems. Java applications runs on the top of the Java Virtual Machine (JVM), which means that it is independent of the platform it has been written on. Java is most well known for its Garbage Collector (GC), a powerful way to manage memory.

Java is not Javascript, which was developed in the mid 90s by Netscape INC. Despite having very similar names, Java and Javascript are two different languages, with a different purpose.

> Javascript is to Java what hamster is to ham. – Jeremy Keith

### Distributed

Elasticsearch runs on as many hosts as required by the workload or the amount of data. Hosts communicate and synchronize using messages over the network. A networked machine running Elasticsearch is called a node, and the whole group of nodes sharing the same cluster name is called a cluster.

### Scalable

Elasticsearch scales horizontally. Horizontal scaling means that the cluster can grow by adding new nodes. When adding more machines, you don't need to restart the whole cluster. When a new node joins the cluster, it gets a part of the existing data. Horizontal scaling is the opposite of vertical scaling, where the only way to grow is running a software on a bigger machine.

### Fault tolerant

Elasticsearch ensures the data is replicated at least once - unless specified - on 2 separate nodes. When a node leaves the cluster, Elasticsearch rebuilds the replication on the remaining nodes, unless there's no more node to replicate to.

---

## What's an Elasticsearch cluster?

A cluster is a host or a group of hosts running Elasticsearch and configured with the same `cluster name`.  The default `cluster name` is `elasticsearch` but using it in production is not recommended.

Each host in an Elasticsearch cluster can fulfill one or multiple roles in the following: 

### Master node

The master nodes control the cluster. They give joining nodes information about the cluster, decide where to move the data, and reallocate the missing data when a node leaves. When multiple nodes can handle the master role, Elasticsearch elects an acting master. The acting master is called `elected master` When the elected master leaves the cluster, another master node takes over the role of elected master.

### Ingest  nodes

An ingest node pre-process's documents before the actual document indexing happens. The ingest node intercepts bulk and index requests, it applies transformations, and it then passes the documents back to the index or bulk APIs. 

All nodes enable ingest by default, so any node can handle ingest tasks. You can also create dedicated ingest nodes.

### Data Nodes

Data nodes store the indexed data. They are responsible for managing stored data, and performing operations on that data when queried.

### Tribe Nodes

Tribe nodes connect to multiple Elasticsearch clusters and performs operations such as search accross every connected clusters. 

### A Minimal, Fault Tolerant Elasticsearch Cluster

![A Minimal Elasticsearch cluster](images/image1.svg)

A minimal fault tolerant Elasticsearch cluster should be composed of:

* 3 master nodes
* 2 ingest nodes
* 2 data nodes

Having 3 master nodes is important to make sure that the cluster won't be in a state of split brain in case of network separation, by making sure that there are at least 2 eligible master nodes present in the cluster. If the number of eligible master nodes falls behind 2, then the cluster will refuse any new indexing until the problem is fixed. 

---

## What's an Elasticsearch index

An `index` is a group of documents that with similar characteristics. It is identified by a name which is used when performing operations against stored documents or the `index` structure itself. An `index` structure is defined by a `mapping`, a `JSON` file describing both the document characteristics and the `index` options such as the replication factor. In an Elasticsearch cluster, you can define as many `indexes` as you want.

An Elasticsearch `index` is composed of 1 or multiple `shards`. A `shard` is a Lucene index, and the number of `shards` is defined at the `index` creation time.  Elasticsearch allocates an `index` `shards` accross the cluster, either automatically or according to user defined rules.

Lucene is the name of the search engine that powers Elasticsearch. It is an open source project from the Apache Foundation. You most probably never hear about Lucene when operating an Elasticsearch cluster, but this book covers the basics you need to know.

A `shard` is made of one or multiple `segments`, which are binary files where Lucene indexes the stored documents.

![Inside an Elasticsearch index](images/image2.svg)

If you're familiar with relational databases such as MySQL, then an `index` is a database, the `mapping` is the database schema, and the shards represent the database data. Due to the distributed nature of Elasticsearch, and the specificities of Lucene, the comparison with a relational database stops here.

---

## Deploying your first Elasticsearch cluster

### Deploying Elasticsearch on Debian

TODO [issue #9](https://github.com/fdv/running-elasticsearch-fun-profit/issues/9)

### Deploying Elasticsearch on RHEL / CentOS

TODO [issue #9](https://github.com/fdv/running-elasticsearch-fun-profit/issues/9)

---

## First step using Elasticsearch

TODO [issue #10](https://github.com/fdv/running-elasticsearch-fun-profit/issues/10)

---

## Elasticsearch Configuration

TODO [issue #10](https://github.com/fdv/running-elasticsearch-fun-profit/issues/10)

## Elasticsearch Plugins

TODO [issue #10](https://github.com/fdv/running-elasticsearch-fun-profit/issues/10)
