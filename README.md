```
WIP, COVERS ELASTICSEARCH 5.5.x, UPDATING TO ES 6.5.x
```

# Operating Elasticsearch
## for Fun and Profit

---

![](images/image1.png)


## [Fred de Villamil](https://thoughts.t37.net)

---

## [Read online](https://fdv.github.io/running-elasticsearch-fun-profit)

---

## Code of Conduct

- Behave like normal, friendly, welcoming human beings or get the hell out.
- Any reference to a non scientific, verifiable element is irrelevant.

---

## TOC

- [Getting Started with Elasticsearch](001-getting-started/001-getting-started.md/#getting-started-with-elasticsearch)
  * [Prerequisites](001-getting-started/001-getting-started.md/#prerequisites)
  * [Elasticsearch basic concepts](001-getting-started/001-getting-started.md/#elasticsearch-basic-concepts)
    + [REST APIs](001-getting-started/001-getting-started.md/#rest-apis)
    + [Open Source](001-getting-started/001-getting-started.md/#open-source)
    + [Java](001-getting-started/001-getting-started.md/#java)
    + [Distributed](001-getting-started/001-getting-started.md/#distributed)
    + [Scalable](001-getting-started/001-getting-started.md/#scalable)
    + [Fault tolerant](001-getting-started/001-getting-started.md/#fault-tolerant)
  * [What's an Elasticsearch cluster?](001-getting-started/001-getting-started.md/#what-s-an-elasticsearch-cluster-)
    + [Master node](001-getting-started/001-getting-started.md/#master-node)
    + [Ingest  nodes](001-getting-started/001-getting-started.md/#ingest--nodes)
    + [Data Nodes](001-getting-started/001-getting-started.md/#data-nodes)
    + [Tribe Nodes](001-getting-started/001-getting-started.md/#tribe-nodes)
    + [A Minimal, Fault Tolerant Elasticsearch Cluster](001-getting-started/001-getting-started.md/#a-minimal--fault-tolerant-elasticsearch-cluster)
  * [What's an Elasticsearch index](001-getting-started/001-getting-started.md/#what-s-an-elasticsearch-index)
  * [Deploying your first Elasticsearch cluster](001-getting-started/001-getting-started.md/#deploying-your-first-elasticsearch-cluster)
    + [Deploying Elasticsearch on Debian](001-getting-started/001-getting-started.md/#deploying-elasticsearch-on-debian)
    + [Deploying Elasticsearch on RHEL / CentOS](001-getting-started/001-getting-started.md/#deploying-elasticsearch-on-rhel---centos)
  * [First step using Elasticsearch](001-getting-started/001-getting-started.md/#first-step-using-elasticsearch)
  * [Elasticsearch Configuration](001-getting-started/001-getting-started.md/#elasticsearch-configuration)
  * [Elasticsearch Plugins](001-getting-started/001-getting-started.md/#elasticsearch-plugins)

- [Elasticsearch and the Java Virtual Machine](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#elasticsearch-and-the-java-virtual-machine)
  * [Supported JVM and operating systems / distributions](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#supported-jvm-and-operating-systems---distributions)
    + [Operating system matrix](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#operating-system-matrix)
    + [Java Virtual Machine matrix](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#java-virtual-machine-matrix)
  * [Memory management](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#memory-management)
  * [Garbage collection](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#garbage-collection)
    + [Concurrent Mark & Sweep Garbage Collector](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#concurrent-mark---sweep-garbage-collector)
    + [Garbage First Garbage Collector](002-elasticsearch-and-the-jvm/002-elasticsearch-and-the-jvm.md/#garbage-first-garbage-collector)

-  [A few things you need to know about Lucene](#a-few-things-you-need-to-know-aboutlucene)
    * [Lucene segments](#lucene-segments)
    * [Lucene deletes and updates](#lucene-deletes-andupdates)

- [Designing the Perfect Elasticsearch Cluster](004-cluster-design/004-cluster-design.md/#designing-the-perfect-elasticsearch-cluster)
  * [Elasticsearch is elastic, for real](004-cluster-design/004-cluster-design.md/#elasticsearch-is-elastic--for-real)
  * [Design for failure](004-cluster-design/004-cluster-design.md/#design-for-failure)
  * [Hardware](004-cluster-design/004-cluster-design.md/#hardware)
    + [CPU](004-cluster-design/004-cluster-design.md/#cpu)
    + [Memory](004-cluster-design/004-cluster-design.md/#memory)
    + [Network](004-cluster-design/004-cluster-design.md/#network)
    + [Storage](004-cluster-design/004-cluster-design.md/#storage)
  * [Software](004-cluster-design/004-cluster-design.md/#software)
    + [The Linux (004-cluster-design/004-cluster-design.md//or FreeBSD) kernel](004-cluster-design/004-cluster-design.md/#the-linux--or-freebsd--kernel)
    + [The Java Virtual Machine](004-cluster-design/004-cluster-design.md/#the-java-virtual-machine)
    + [The filesystem](004-cluster-design/004-cluster-design.md/#the-filesystem)
  * [Designing your indices](004-cluster-design/004-cluster-design.md/#designing-your-indices)
    + [Sharding](004-cluster-design/004-cluster-design.md/#sharding)
    + [Replication](004-cluster-design/004-cluster-design.md/#replication)
  * [Optimising allocation](004-cluster-design/004-cluster-design.md/#optimising-allocation)
  * [Troubleshooting and scaling](004-cluster-design/004-cluster-design.md/#troubleshooting-and-scaling)
    + [CPU](004-cluster-design/004-cluster-design.md/#cpu-1)
    + [Memory](004-cluster-design/004-cluster-design.md/#memory-1)

- [Design for Event Logging](005-design-event-logging/005-design-event-logging.md/#design-for-event-logging)
  * [Design of an event logging infrastructure cluster](005-design-event-logging/005-design-event-logging.md/#design-of-an-event-logging-infrastructure-cluster)
    + [Throughput: how many events per second (005-design-event-logging/005-design-event-logging.md//eps) are you going to collect?](005-design-event-logging/005-design-event-logging.md/#throughput--how-many-events-per-second--eps--are-you-going-to-collect-)
    + [Retention: how long do you want to keep your data, hot and cold?](005-design-event-logging/005-design-event-logging.md/#retention--how-long-do-you-want-to-keep-your-data--hot-and-cold-)
    + [Size: what is the average size of a collected event?](005-design-event-logging/005-design-event-logging.md/#size--what-is-the-average-size-of-a-collected-event-)
    + [Fault tolerance: can you afford losing your indexed data?](005-design-event-logging/005-design-event-logging.md/#fault-tolerance--can-you-afford-losing-your-indexed-data-)
    + [Queries](005-design-event-logging/005-design-event-logging.md/#queries)
  * [Which hardware do I need?](005-design-event-logging/005-design-event-logging.md/#which-hardware-do-i-need-)
  * [How to design my indices?](005-design-event-logging/005-design-event-logging.md/#how-to-design-my-indices-)
  * [What about some tuning?](005-design-event-logging/005-design-event-logging.md/#what-about-some-tuning-)

- [Operating Daily](006-operating-daily/006-operating-daily.md/#operating-daily)
  * [Elasticsearch most common operations](006-operating-daily/006-operating-daily.md/#elasticsearch-most-common-operations)
    + [Mass index deletion with pattern](006-operating-daily/006-operating-daily.md/#mass-index-deletion-with-pattern)
    + [Mass optimize, indexes with the most deleted docs first](006-operating-daily/006-operating-daily.md/#mass-optimize--indexes-with-the-most-deleted-docs-first)
    + [Restart a cluster using rack awareness](006-operating-daily/006-operating-daily.md/#restart-a-cluster-using-rack-awareness)
    + [Optimize your cluster restart](006-operating-daily/006-operating-daily.md/#optimize-your-cluster-restart)
    + [Remove data nodes from a cluster the safe way](006-operating-daily/006-operating-daily.md/#remove-data-nodes-from-a-cluster-the-safe-way)
  * [Get useful information about your cluster](006-operating-daily/006-operating-daily.md/#get-useful-information-about-your-cluster)
    + [Nodes information](006-operating-daily/006-operating-daily.md/#nodes-information)
    + [Monitor your search queues](006-operating-daily/006-operating-daily.md/#monitor-your-search-queues)
    + [Indices information](006-operating-daily/006-operating-daily.md/#indices-information)
    + [Shard allocation information](006-operating-daily/006-operating-daily.md/#shard-allocation-information)
    + [Recovery information](006-operating-daily/006-operating-daily.md/#recovery-information)
    + [Segments information (006-operating-daily/006-operating-daily.md//can be extremely verbose)](006-operating-daily/006-operating-daily.md/#segments-information--can-be-extremely-verbose-)
    + [Cluster stats](006-operating-daily/006-operating-daily.md/#cluster-stats)
    + [Nodes stats](006-operating-daily/006-operating-daily.md/#nodes-stats)
    + [Indice stats](006-operating-daily/006-operating-daily.md/#indice-stats)
    + [Indice mapping](006-operating-daily/006-operating-daily.md/#indice-mapping)
    + [Indice settings](006-operating-daily/006-operating-daily.md/#indice-settings)
    + [Cluster dynamic settings](006-operating-daily/006-operating-daily.md/#cluster-dynamic-settings)
    + [All the cluster settings (006-operating-daily/006-operating-daily.md//can be extremely verbose)](006-operating-daily/006-operating-daily.md/#all-the-cluster-settings--can-be-extremely-verbose-)


- [Monitoring Elasticsearch](007-monitoring-es/007-monitoring-es.md/#monitoring-elasticsearch)
  * [Tools](007-monitoring-es/007-monitoring-es.md/#tools)
  * [Monitoring at the host level](007-monitoring-es/007-monitoring-es.md/#monitoring-at-the-host-level)
  * [Monitoring at the node level](007-monitoring-es/007-monitoring-es.md/#monitoring-at-the-node-level)
  * [Monitoring at the cluster level](007-monitoring-es/007-monitoring-es.md/#monitoring-at-the-cluster-level)
  * [Monitoring at the index level](007-monitoring-es/007-monitoring-es.md/#monitoring-at-the-index-level)

- [How we reindexed 36 billion documents in 5 days within the same Elasticsearch cluster](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#how-we-reindexed-36-billion-documents-in-5-days-within-the-same-elasticsearch-cluster)
  * [The "Blackhole" cluster](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#the--blackhole--cluster)
  * [Elasticsearch configuration](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#elasticsearch-configuration)
  * [Tuning the Java virtual machine](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#tuning-the-java-virtual-machine)
    + [Blackhole Initial indexing](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#blackhole-initial-indexing)
  * [Blackhole initial migration](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#blackhole-initial-migration)
  * [Blackhole reindexing](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#blackhole-reindexing)
    + [The reindexing process](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#the-reindexing-process)
    + [Logstash configuration](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#logstash-configuration)
    + [Reindexing Elasticsearch configuration](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#reindexing-elasticsearch-configuration)
    + [Introducing Yoko and Moulinette](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#introducing-yoko-and-moulinette)
  * [Reindexing in 5 days](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#reindexing-in-5-days)
  * [Conclusion](100-use-cases-reindexing-36-billion-docs/100-use-cases-reindexing-36-billion-docs.md/#conclusion)

- [Use Case: Migrating a Cluster Across the Ocean Without Downtime](101-use-case-migrating-cluster-over-ocean/101-use-case-migrating-cluster-over-ocean.md/#use-case--migrating-a-cluster-across-the-ocean-without-downtime)

- [Use Case: An Advanced Elasticsearch Architecture for High-volume Reindexing](102-use-case-advanced-architecture-high-volume-reindexing/102-use-case-advanced-architecture-high-volume-reindexing.md/#use-case--an-advanced-elasticsearch-architecture-for-high-volume-reindexing)
  * [A glimpse at our infrastructure](102-use-case-advanced-architecture-high-volume-reindexing/102-use-case-advanced-architecture-high-volume-reindexing.md/#a-glimpse-at-our-infrastructure)
  * [Using Elasticsearch for fun and profit](102-use-case-advanced-architecture-high-volume-reindexing/102-use-case-advanced-architecture-high-volume-reindexing.md/#using-elasticsearch-for-fun-and-profit)
  * [Conclusion](102-use-case-advanced-architecture-high-volume-reindexing/102-use-case-advanced-architecture-high-volume-reindexing.md/#conclusion)

- [Migrating a 130TB Cluster from Elasticsearch 2 to 5 in 20 Hours with 0 Downtime and a Rollback Strategy](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#migrating-a-130tb-cluster-from-elasticsearch-2-to-5-in-20-hours-with-0-downtime-and-a-rollback-strategy)
  * [Elasticsearch @Synthesio, November 2017](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#elasticsearch--synthesio--november-2017)
  * [The Blackhole Cluster](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#the-blackhole-cluster)
  * [Migration Strategies: Cluster restart VS Reindex API VS Logstash VS the Fun Way](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#migration-strategies--cluster-restart-vs-reindex-api-vs-logstash-vs-the-fun-way)
    + [The Cluster Restart Strategy](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#the-cluster-restart-strategy)
    + [The Reindex API Strategy](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#the-reindex-api-strategy)
    + [The Logstash Strategy](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#the-logstash-strategy)
    + [The Fun Way](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#the-fun-way)
  * [Migrating Blackhole for Real](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#migrating-blackhole-for-real)
    + [Expanding Blackhole](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#expanding-blackhole)
    + [Splitting Blackhole in 2](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#splitting-blackhole-in-2)
  * [Conclusion](103-use-case-migrating-130tb-cluster-without-downtime/103-use-case-migrating-130tb-cluster-without-downtime.md/#conclusion)

---

## Styling

This is the Markdown styling used in this book. If you plan to contribute, please use it.

### Chapter title

```markdown
# This is a chapter title

```

### Chapter part

```markdown
---

## A chapter part title is preceded by an horizontal line
```

### Chapter subpart

```markdown
### A level 1 subpart
#### A level 2 subpart
```

### Images

```markdown
![An image should have an alt text](use/a/relative.link)
```

### Code:

```markdown
An `inline code block` goes like this
```

API calls go the Curl way

```bash
curl -X POST "localhost:9200/_search" -H 'Content-Type: application/json' -d'
{
    "query" : {
        "match_all" : {}
    },
    "stats" : ["group1", "group2"]
}
'
```

Yaml code is expanded for more readability
```yaml
---
some:
  value:
    goes: "like this"
```

### Links

```markdown
[An internal link](has/a/relative.path)
[An external link](https://has.an.absolute/path)
```

### Lists

Urdered lists:

```markdown
Only one line break between a paragraph and

* An
* unordered
* list
	* with
	* subitems
```

Ordered lists:

```markdown
1. An
2. Ordered
3. List
	1. With
	2. subitems
```

