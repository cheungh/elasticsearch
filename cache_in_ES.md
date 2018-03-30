### Node query cache
<pre>
1. query result cache -- only apply to filter context
2. There is one queries cache per node that is shared by all shards
3. LRU eviction policy -- when it reaches limit, the oldest used data will be evicted for new data
4. cluster data node setting -- indices.queries.cache.size: 5%
5. index level setting -- index.queries.cache.enabled: [true|false]
6. cluster api call -- curl -XGET "http://localhost:9200/_nodes/stats/indices/query_cache?pretty" 
</pre>

### Shard request cache
<p>
Shard request cache is responsible for caching the local results of each shard. This is used for heavy aggregation.
  It is turned on by default.<br>
  indices.requests.cache.size: 2%<br>
Quoted from ES:
"When a search request is run against an index or against many indices, 
each involved shard executes the search locally and returns its local results to the coordinating node, 
which combines these shard-level results into a global result set."
refer to: https://www.elastic.co/guide/en/elasticsearch/reference/current/shard-request-cache.html
</p>

### The indexing buffer
<p>
The indexing buffer is used to store newly indexed documents. 
When it fills up, the documents in the buffer are written to a segment on disk. 
It is divided between all shards on the node.
</p>

### Clear cache


### The field data cache
