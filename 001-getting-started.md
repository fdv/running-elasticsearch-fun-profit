**WIP, covers Elasticsearch 5.5.x**

# Getting Started with Elasticsearch

This chapter is for people who have never deployed an Elasticsearch cluster before. It provides a quick step by step hands on and explains Elasticsearch basic concepts before going further into reading this book. Most of these concepts will be detailed further in this book.

In this introduction chapter you will learn:

- The basic concepts behind Elasticsearch.
- What's in an Elasticsearch cluster.
- How to deploy your first Elasticsearch cluster.
- How to use Elasticsearch to index and find documents.

---

## Elasticsearch basic concepts

Elasticsearch is a distributed, scalable, fault tolerant open source search engine written in Java. The project is lead by Elastic, a company created by Shay Banon, who created Elasticsearch on top of Lucene.

- Distributed: Elasticsearch runs on as many hosts as required by the workload or the amount of data. Hosts communicate and synchronise using messages over the network using port 9300. A networked machine running Elasticsearch is called a node, and the whole group of nodes sharing the same cluster name is called a cluster.
- Scalable: Elasticsearch scales horizontally. Horizontal scaling means that the cluster can grow by adding new nodes without restarting the cluster. When a new node joins the cluster, it gets a part of the existing data. Horizontal scaling is the opposite of vertical scaling, where the only way to grow is running a software on a bigger machine.
- Fault tolerant: Elasticsearch ensures the data is replicated at least once - unless specified - on 2 separate nodes. When a node leaves the cluster, Elasticsearch rebuilds the replication on the remaining nodes, unless there\'s no more node to replicate to.

## What's an Elasticsearch cluster?

An Elasticsearch cluster is a node or a group of nodes filling the following roles:

Master node: the master node controls the cluster. It gives joining nodes informations about the cluster, decides where the data is located, and decides to reallocate the missing data when a node leaves. When multiple nodes can handle the master role, Elasticsearch chooses an elected master. When the elected master leaves the cluster, another master node takes over the role of elected master.

HTTP nodes: Elasticsearch provides a REST API for indexing, querying and administrating the cluster. Elasticsearch REST API listens on port 9200.

Data nodes: Nodes where the data is stored. Data nodes are responsible for managing stored data, and performing operations on that data when queried.

A single node can fulfil every required role, at the cost of fault tolerance and horizontal scaling.

TODO: add a cluster schema

## What's an Elasticsearch index

TODO

---

## Deploying your first Elasticsearch cluster

### Deploying Elasticsearch on Debian

TODO

### Deploying Elasticsearch on RHEL / CentOS

TODO

---

## First step using Elasticsearch

TODO