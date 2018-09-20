**WIP, covers Elasticsearch 5.5.x**

# Operating Daily

## Elasticsearch most common operations

### Mass index deletion with pattern

I often have to delete hundreds of indexes at once. Their name usually follow some patterns, which makes batch deletion easier.

```
for index in $(curl -XGET esmaster:9200/_cat/indices | awk '/pattern/ {print $3}'); do 
	curl -XDELETE esmaster:9200/$index?master_timeout=120s;
done
```

### Mass optimize, indexes with the most deleted docs first

Lucene, which powers Elasticsearch has a specific behavior when it comes to delete or update documents. Instead of actually deleting or overwriting the data, if flags it as deleted and write a new one. The only way to get rid of a deleted document is to run an *optimize* on your indexes.

This snippet sorts your existing indexes by the number of deleted documents before it runs the optimize.

```
for indice in $(CURL -XGET esmaster:9200/_cat/indices | sort -rk 7 | awk '{print $3}'); do
	curl -XPOST http://esmaster:9200/${indice}/_optimize?max_num_segments=1; 
done
```

### Restart a cluster using rack awareness

Using rack awareness allows to split your replicated data evenly between hosts or data center. It's convenient to restart half of your cluster at once instead of host by host.

```
curl -XPUT 'host:9200/_cluster/settings' -d '{
	"transient" : {
		"cluster.routing.allocation.enable": "none"
	}
}'

for host in $(curl -XGET esmaster:9200/_cat/nodeattrs?attr | awk '/rack_id/ {print $2}'); do
	ssh $host service elasticsearch restart
done

sleep 60

curl -XPUT 'host:9200/_cluster/settings' -d '{
	"transient" : {
		"cluster.routing.allocation.enable": "all
	}
}'
```

### Optimize your cluster restart

There's a simple way to accelerate your cluster restart. Once you've brought your masters back, run this snippet. Most of the options are self explanatory:

```
curl -XPUT 'host:9200/_cluster/settings -d '{
	"transient" : {
		"cluster.routing.allocation.cluster_concurrent_rebalance": 20,
		"indices.recovery.concurrent_streams": 20,
		"cluster.routing.allocation.node_initial_primaries_recoveries": 20,
		"cluster.routing.allocation.node_concurrent_recoveries": 20,
		"indices.recovery.max_bytes_per_sec": "2048mb",
		"cluster.routing.allocation.disk.threshold_enabled" : true,
		"cluster.routing.allocation.disk.watermark.low" : "90%",
		"cluster.routing.allocation.disk.watermark.high" : "98%",
		"cluster.routing.allocation.enable": "primary"
	}
}'
```

Then, once your cluster is back to yellow, run that one:

```
curl -XPUT 'http://escluster:9200/_cluster/settings' -d '{
	"transient" : {
		"cluster.routing.allocation.enable": "all"
	}
}'
```

### Remove data nodes from a cluster the safe way

```
curl -XPUT 'http://escluster:9200/_cluster/settings' -d '{
	"transient" : {
		"cluster.routing.allocation.exclude._ip" : "<data node 1>,<data node 2>,<data node x>"
	}
}'
```

## Get useful information about your cluster

### Nodes information

This snippet gets the most useful information from your Elasticsearch nodes:

* hostname
* role (master, data, nothing)
* free disk space
* heap used
* ram used
* file descriptors used
* load

```
curl -XGET https://escluster/_cat/nodes?v&h=host,r,d,hc,rc,fdc,l

host r d hc rc fdc l

192.168.1.139 d 1tb 9.4gb 58.2gb 20752 0.20
192.168.1.203 d 988.4gb 16.2gb 59.3gb 21004 0.12
192.168.1.146 d 1tb 14.1gb 59.2gb 20952 0.18
192.168.1.169 d 1tb 14.3gb 58.8gb 20796 0.10
192.168.1.180 d 1tb 16.1gb 60.5gb 21140 0.17
192.168.1.188 d 1tb 9.5gb 59.4gb 20928 0.19
```

Then, it's easy to sort the output to get interesting information.

Sort by free disk space

```
curl -XGET https://escluster/_cat/nodes?h=host,r,d,hc,rc,fdc,l | sort -hrk 3
```

Sort by heap occupancy:

```
curl -XGET https://escluster/_cat/nodes?h=host,r,d,hc,rc,fdc,l | sort -hrk 4
```

And so on.

### Monitor your search queues

It's sometimes useful to know what happens on your data nodes search queues. Beyond the search thread pool(default thread pool being ((CPU * 3) / 2) + 1 on each data node, queries get stacked into the search queue, a 1000 buffer.

```
while true; do 
	curl -XGET 'host:9200/_cat/thread_pool?v&h=host,search.queue,search.active,search.rejected,search.completed' | sort -unk 2,3
	sleep 5
done
```

That code snippet only displays the data node running active search queries so it's easier to read on large cluster.

### Indices information

This snippet gets most information you need about your indices. You can then grep on what you need to know: open, closed, green / yellow / red...

```
curl -XGET https://escluster/_cat/indices?v
```

### Shard allocation information

Shards movement have lots of impact on your cluster performances. These snippets allows you to get the most critical information about your shards.

```
curl -XGET https://escluster/_cat/shards?v

17_20140829 4 r STARTED 2894319 4.3gb 192.168.1.208 esdata89
17_20140829 10 p STARTED 2894440 4.3gb 192.168.1.206 esdata87
17_20140829 10 r STARTED 2894440 4.3gb 192.168.1.199 esdata44
17_20140829 3 p STARTED 2784067 4.1gb 192.168.1.203 esdata48
```

### Recovery information

Recovery information comes under the form of a JSON output but it's still easy to read to understand what happens on your cluster.

```
curl -XGET https://escluster/_recovery?pretty&active_only
```

### Segments information (can be extremely verbose)

```
curl -XGET https://escluster/_cat/nodes?h=host,r,d,hc,rc,fdc,l | sort -hrk 3
```

### Cluster stats

```
curl -XGET https://escluster/_cluster/stats?pretty
```

### Nodes stats

```
curl -XGET https://escluster/_nodes/stats?pretty
```

### Indice stats

```
curl -XGET https://escluster/someindice/_stats?pretty
```

### Indice mapping

```
curl -XGET https://escluster/someindice/_mapping
```

### Indice settings

```
curl -XGET https://escluster/someindice/_mapping/settings
```

### Cluster dynamic settings

```
curl -XGET https://escluster/_cluster/settings
```

### All the cluster settings (can be extremely verbose)

```
curl -XGET https://escluster/_settings
```
