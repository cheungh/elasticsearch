## Important concepts in ElasticSearch
### Inverted Index:
<p>That is what make ES fast, see the simple explanation 
from https://github.com/cheungh/elasticsearch/blob/master/Inverted_index.md</p>

### FieldData cache 
<p>
Quoted from ES Official doc: "The field data cache is used mainly when sorting on or computing aggregations on a field..."  
It need enough memory to do this job otherwise it will be really expensive to build. 
</p>
#### Need to watch for these settings
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

## To Be Continue ...
 
