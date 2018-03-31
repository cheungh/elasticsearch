### Tuning Tips

<p>1.	Assign the right heap size, default setting is 1G. In a 64GB example, 31GB should be allocate to JVM heap size. The rest for Lucene which is designed to leverage the underlying OS for caching in-memory data structures.  Prolong heap over 80% are not good.</p>
<p>2.	Turn off swap at all cost</p>
<p>3.	Shard size not too small. Quoted from ES: “Small shards result in small segments, which increases overhead. Aim to keep the average shard size between a few GB and a few tens of GB. … A good rule-of-thumb is to ensure you keep the number of shards per node below 20 to 25 per GB heap it has configured. A node with a 30GB heap should therefore have a maximum of 600-750 shards”</p>
<p>4.	Indexing schema correctly via good mapping, turn off some not used feature.</p>
<p>5.	Watch for Deleted_document</p>
<p>6.	Watch for CPU utilization via htop: are all cores peak and prolong at 100%?</p>
<p>7.	DISK I/O, is it write intensive?</p>
<p>8.	Watch for high watermarks.</p>
<p>9.	At some point, additional nodes may be needed on horizontal scaling to improve performance</p>
<p>10.	Set circuit breaker limit: query existing quota via http://localhost:9200/_nodes/stats/breaker</p>
<p>11.	Query cache: http://localhost:9200/_nodes/stats/indices/query_cache</p>
<p>12.	http://localhost:9200/_nodes/stats/indices/request_cache</p>
<p>13. Segment API: information on index segments stats (e.g. "num_docs": 190, "deleted_docs": 7,"size_in_bytes": 95944)<br>
 This query will query against the bank index: http://localhost:9200/bank/_segments
</p>
<p>
14. cluster state api gives quick summary of the ES cluster: http://localhost:9200/_cluster/state  
</p>
<p>15. node api gives ES cluster node level information: http://localhost:9200/_nodes?pretty
