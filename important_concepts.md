## Important concepts in ElasticSearch
### Inverted Index:
<p>That is what make ES fast, see the simple explanation 
from https://github.com/cheungh/elasticsearch/blob/master/Inverted_index.md</p>

### FieldData cache 
<p>
Quoted from ES Official doc: "The field data cache is used mainly when sorting on or computing aggregations on a field..."  
It need enough memory to do this job otherwise it will be really expensive to build. 
</p>

### Need to watch for these settings
<pre>
indices.fielddata.cache.size.  
Note from ES: reloading the field data which does not fit into your cache will be expensive and perform poorly.   

indices.breaker.fielddata.limit  
check and balance! We don't want this cache use up all the memory on the node.  
</pre>

### Heap Size
This is how much memory Elasticsearch is using
<pre>
1. Setting  
2. How much is consumed? If system uses over 80% heap size, it is in an alarm state. 
System will get very slow and users will have bad experience encountering circuit breaker.  

For large 64 bit OS with over 64G memory: this should set to 31GB.
</pre>

### high disk watermark warning
The node is about to be out of disk. 
ES cluster will move data to another node resulting in an unbalanced Shards cluster.

### Reindex 
It is the ES tool to copy from one to another 
copy index from one to another  
can copy from remote host

### refresh
<p>The refresh API allows to explicitly refresh one or more index, making all operations performed since the last refresh available for search. <br>
 API call: curl -XPOST 'localhost:9200/bank_account/_refresh?pretty'
</p>
 
### flush
ES Doc: "frees memory from the index by flushing data to the index storage and clearing the internal transaction log."

### shard allocation awareness
<p>It ensures the primary shard and its replica shards are spread across different physical servers, racks, or zones, to minimise the risk of losing all shard copies at the same time. ES will allocate the shards evenly on different rack. So <br>
e.g. 
cluster.routing.allocation.awareness.attributes: rack_id<br> 
node1 with: node.rack_id:rack_1 <br>
node2 with: node.rack_id:rack_1 <br>
node3 with: node.rack_id:rack_2 <br>
node4 with: node.rack_id:rack_2 <br>
</p>

### Rolling restart or hardware upgrade
<pre>
- prevent shard allocation (otherwise ES will allocate shard accross other nodes
PUT /_cluster/settings
{
    "transient" : {
        "cluster.routing.allocation.enable" : "none"
    }
}

- restart a single node -- let say memory upgrade, network card upgrade, add harddrive
 a. stop service
 b. perform upgrade
 c. start ES service 

- restart all nodes by rack_id group (this is roll restart step)
Let say you have 3 rack ids in your grade: (a) rack_1, (b) rack_2, (c) rack_3
start rack_1 group first, then rack_2, then rack_3

- confirm all nodes have joined the cluster

- re-enable shard allocation:
PUT /_cluster/settings
{
    "transient" : {
        "cluster.routing.allocation.enable" : "all"
    }
}
</pre>

### index default settings
<pre>
Most important default settings are not associated with any specific index module 
index.number_of_replicas: 1
index.number_of_shards:5
index.refresh_interval: 60
index.max_result_window: 2000 -- too high will result in using heap memory
</pre>

### How would the master reacts When a node leaves the cluster for whatever reason:
<pre>
1. Promoting a replica shard to primary to replace any primaries that were on the missing node.
2. Allocating replica shards to replace the missing replicas (assuming there are enough nodes).
3. Rebalancing shards evenly across the remaining nodes.

default setting 1m
index.unassigned.node_left.delayed_timeout

if the node is not going to return:
run:
PUT _all/_settings
{
  "settings": {
    "index.unassigned.node_left.delayed_timeout": "0"
  }
}
</pre>

### Cluster Update Settings
<pre>
1. persistent (applied across restarts) 
2. transient (will not survive a full cluster restart)

curl -XGET 'localhost:9200/_cluster/settings?pretty'
return:
{
  "persistent": {},
  "transient": {}
}
</pre>

## To Be Continue ...
 
