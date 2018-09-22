```
WIP, COVERS ELASTICSEARCH 5.5.x
```

# Getting Started with Elasticsearch

This chapter is for people who have never used Elasticsearch cluster. It explains Elasticsearch basic concepts and guides you into deploying your first cluster. Most of these concepts will be detailed further in this book.

In this introduction chapter you will learn:

- The basic concepts behind Elasticsearch
- What's in an Elasticsearch cluster
- How to deploy your first Elasticsearch cluster
- How to use Elasticsearch to index and find content
- Elasticsearch configuration basics
- What's an Elasticsearch plugin and how to use them

---

## Elasticsearch basic concepts

Elasticsearch is a distributed, scalable, fault tolerant open source search engine written in Java. It provides a powerful REST API both for adding or searching data and updating the configuration. Elasticsearch is led by Elastic, a company created by Shay Banon, who started the project on top of Lucene.

Open source means that Elasticsearch source code, the recipe to build the software, is public, free, and that anyone can contribute to the project by adding missing feature, documentation or fixing bugs. If accepted by the project, their work is then available to the whole commnunity. Because Elasticsearch is open source, the company behind it can go bankrupt or stop maintaining the project without killing it. Someone else will be able to take over it and keep the project alive.

Java is a programming language created in 1995 by Sun Microsystems. Java applications runs on the top of the Java Virtual Machine (JVM), which means that it is independant of the platform it has been written on. Java is most well known for its Garbage Collector (GC), a powerful way to manage memory.

Distributed: Elasticsearch runs on as many hosts as required by the workload or the amount of data. Hosts communicate and synchronise using messages over the network. A networked machine running Elasticsearch is called a node, and the whole group of nodes sharing the same cluster name is called a cluster.

Scalable: Elasticsearch scales horizontally. Horizontal scaling means that the cluster can grow by adding new nodes. When adding more machines, you don't need to restart the whole cluster. When a new node joins the cluster, it gets a part of the existing data. Horizontal scaling is the opposite of vertical scaling, where the only way to grow is running a software on a bigger machine.

Fault tolerant: Elasticsearch ensures the data is replicated at least once - unless specified - on 2 separate nodes. When a node leaves the cluster, Elasticsearch rebuilds the replication on the remaining nodes, unless there's no more node to replicate to.

A REST API is an application program interface (API) that uses HTTP requests to GET, PUT, POST and DELETE data. An API for a website is code that allows two software programs to communicate with each another. The API spells out the proper way for a developer to write a program requesting services from an operating system or other application. REST is the Web counterpart of databases CRUD (Create, Read, Update, Delete).

---

## What's an Elasticsearch cluster?

An Elasticsearch cluster is a node or a group of nodes filling the following roles:

Master node: the master node controls the cluster. It gives joining nodes informations about the cluster, decides where the data is located, and decides to reallocate the missing data when a node leaves. When multiple nodes can handle the master role, Elasticsearch chooses an elected master. When the elected master leaves the cluster, another master node takes over the role of elected master.

HTTP nodes: Elasticsearch provides a REST API for indexing, querying and administrating the cluster. Elasticsearch REST API listens on port 9200.

Data nodes: Nodes where the data is stored. Data nodes are responsible for managing stored data, and performing operations on that data when queried.

A single node can fulfil every required role, at the cost of fault tolerance and horizontal scaling.

TODO: [add a cluster schema](https://github.com/fdv/running-elasticsearch-fun-profit/issues/7)

---

## What's an Elasticsearch index

TODO [issue #8](https://github.com/fdv/running-elasticsearch-fun-profit/issues/8)

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

TODO [issue #10](https://github.com/fdv/running-elasticsearch-fun-profit/issues/0)

## Elasticsearch Plugins

TODO [issue #10](https://github.com/fdv/running-elasticsearch-fun-profit/issues/10)