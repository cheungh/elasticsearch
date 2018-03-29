### Node query cache


### Shard request cache
<p>
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
