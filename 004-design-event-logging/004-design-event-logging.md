```
WIP, COVERS ELASTICSEARCH 5.5.x
```

# Design for Event Logging

Elasticsearch has made a blast in the event analysis world thanks --- or because of --- the famous Elasticsearch / Logstash / Kibana (ELK) trinity. In this specific use cas, Elasticsearch acts as a hot storage that makes normalized events searchable.

The usual topology of an event analysis infrastructure is more or less the same whatever the technical stack.

Heterogeneous events are pushed from various location into a queue. Queuing has 2 purposes: make sure the data processing won't act as a bottleneck in case of unexpected spike, and make sure no event is lost if the data processing stack crashes.

A data processing tool normalises the events. You have 0 chance to have homogeneous events in an event analysis infrastructure. Events can be logs, metrics, or whatever you can think about, and they need to be normalised to be searchable.

The data processing tool forwards the events to a hot storage where they can be searched. Here, the hot storage is, indeed, Elasticsearch.

## Design of an event logging infrastructure cluster

Event analysis is the typical use case where you can start small, with a single node cluster, and scale when needed. Most of the times, you won't collect all the events you want to analyse from day 1, so it's OK not to over engineer things.

The event logging infrastructure is the typical tricky use case that might have you pull your hair for some times saying Elasticsearch is the worst software ever. It's both extremely heavy on writes, with only a few search query.

Writes can easily become the bottleneck of the infrastructure, either from a CPU or storage point of view, one more reason to chose the software prior to Elasticsearch wisely to avoid losing events.

Searches are performed on such an amount of data that one of them might trigger an out of memory error on the Java heap space, or an infinite garbage collection.

Before you start, there's a few things you need to think about. Since we focus on designing an Elasticsearch cluster, we'll start from the moment events are normalised and pushed into Elasticsearch.

### Throughput: how many events per second (eps) are you going to collect?

This is not a question you can answer out of the box unless you already have a central events collection platform. It's an important one though, as it will define most of your hardware requirements. Events logging varies a lot according to your platform activity, so I'd recommend tracking them for a week or more before you start building your Elasticsearch cluster.

One important things to know is: do you need realtime indexing, or can you accept some lag. If the latter is an option, then you can let the lag being indexed after a spike of events happen, so you don't need to build for the maximum amount of events you can get.

### Retention: how long do you want to keep your data, hot and cold?

Hot data is data you can access immediately, while cold data is what can be accessed within a reasonable amount of time. Retention depends both on your needs and national regulation. For example, in France, we're supposed to keep our access logs during a full year, financial transactions need to be kept for 3 to 5 years, etc.

On Elasticsearch, hot data means opened, accessible indexes. Cold data means closed indexes, or backups of an index snapshot you can easily and quickly transfer and reopen.

### Size: what is the average size of a collected event?

This metric is important as well. Knowing about throughput * retention period * events size will help you define the amount and type of storage you need, hence the cost of your events logging platform.

Storage = throughput * events size * retention period.

Hot data is made of opened, searchable indices. They are the one you'll search into on a regular basis, for debugging or statistics purpose.

### Fault tolerance: can you afford losing your indexed data?

Most of the time, losing your search backend is an option. Lots of people use the ELK stack to store application logs so they are easier to debug, and they are not a critical part of their infrastructure. Logs are also stored somewhere else, for example on a central syslog server so they are still searchable using some shell skills.

When you can lose your search backend for a few hours, or don't want to invest in a full cluster, then a single Elasticsearch server is enough, provided your throughput allows it.

The host minimal configuration is then:

```yaml
master: true
data: true
index:
  number_of_replicas: 0
```

If you start combining events analysis with alerting, or if you need your events to be searchable in realtime without downtime, then things get a bit more expensive. For example, you might want to correlate your whole platform auth.log to look for intrusion attempts or port scanning, so you can deploy new firewall rules accordingly. Then you'll have to start with a 3 nodes cluster. 3 nodes is a minimum since you need 2 active master nodes to avoid a split brain.

![](images/image6.svg)

Here, the minimal hosts configuration for the master / http node is:

```yaml
master: true
data: false
index:
  number_of_replicas: 1
```

And for the data nodes:

```yaml
master: true
data: true
index:
  number_of_replicas: 1
```

If you decide to go cheap and combine the master and data nodes in a 3 hosts cluster, never use bulk indexing.

Bulk indexing can put lots of pressure on the server memory, leading the master to exit the cluster. If you plan to run bulk indexing, then add one or 2 dedicated http node.

The same applies to highly memory consuming queries. If you plan to run such queries, then move your master nodes out of the data nodes.

### Queries

The last thing you need to know about is the type of queries that are going to be ran against your Elasticsearch cluster. If you need to run simple queries, like looking for an error message, then memory pressure won't be a real problem, even against large data. Things get more interested when you need to perform complex filtered queries, or aggregations against a large set of data. Then you'll put lots of pressure on the cluster memory.

## Which hardware do I need?

Once you've gathered all your prerequisites, it's time for hardware selection.

Unless you're using ZFS as a filesystem to profit from compression and snapshots, you should not need more than 64GB RAM. ZFS is popular both to manage extremely large file systems and for its feature, but is greedy on memory.

Chose the CPU depending on both your throughput and your filesystem. ZFS is more greedy than ext4, for example. Elasticsearch index thread pool is equal to the number of available processors + 1, with a default queue of 200. So if you have a 24 core host, Elasticsearch will be able to mange 25 indexing at once, with a queue of 200. Everything else will be rejected.

You can choose to use bulk indexing, which will allow you to index more events at the same time. The default thread pool and queue size are the same as the index thread pool.

The storage part will usually be your bottleneck.

Indeed, local storage and SSD are preferred, but lots of people will chose spinning disks or an external storage with fiberchannel to have more space.

Whatever you chose, the more disks, the better. More disks provide you more axis, hence a faster indexing. If you go with some RAID10, then chose smaller disks, as very large disks such as 4TB+ spinning disks will take ages to rebuild.

On a single node infrastructure, my favorite setup for a huge host is a RAID10 with as many 3.8TB SSD disks possible. Some servers can host up to 36 of them, which makes 18 available axes for more or less 55TB of usable space.

On a multiple node infrastructure, I prefer to multiply the smaller hosts with a RAID0 and 8TB to 10TB space. This works great with 8 data nodes and more since rebuilding takes lots of time.

## How to design my indices?

As usual, it depends on your needs, but this is the time to play with aliases and timestamped indexes. For example, if you're storing the output of your infrastructure auth.log, your indices can be:

```bash
auth-\$(date +%Y-%m-%d)
```

You'll probably want to have 1 index for each type of event you want to index, so you can build various, more adapted mappings. Event collection for syslog does not require the same index topology as an application event tracing, or even some temperature metrics you might want to put in a TSDB.

While doing it, remember that too many indexes and too many shards might put lots of pressure on a single host. Constant writing create lots of Lucene segments, so make sure Elasticsearch won't have \"too many open files\" issues.

## What about some tuning?

Here starts the fun part.

Depending on your throughput, you might need a large [indexing buffer](https://www.elastic.co/guide/en/elasticsearch/reference/current/indexing-buffer.html). The indexing buffer is a bunch of memory that stores the data to index. It differs from the index and bulk thread pools which manage the operations.

Elasticsearch default index buffer is 10% of the memory allocated to the heap. But for heavy indexing operations, you might want to raise it to 30%, if not 40%.

```yaml
indices:
  memory:
    index_buffer_size: "40%"
```

Elasticsearch provides a per node [query cache](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-cache.html). Let's put it this way: you don't need caching on an event logging infrastructure. There's a way to disable it completely and that's what we want.

```yaml
indices:
  query:
    cache.enabled: false
```

You will also have a look at the indexing thread pool. I don't recommend changing the thread pool size, but depending on your throughput, changing the queue size might be a good idea in case of indexing spike.

```yaml
thread_pool:
  bulk:
    queue_size: 3000
  index:
    queue_size: 3000
```

Finally, you will want to disable the store throttle if you're running on enough fast disks.

```yaml
store:
  throttle.type: 'none'
```

One more thing: when you don't need data in realtime, but can afford waiting a bit, you can cut your cluster a little slack by raising the indices refresh interval.

```yaml
index:
  refresh_interval: "1m"
```
