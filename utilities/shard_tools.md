### Cluster Allocation Explain API
<p> quoted from ES, "The purpose of the cluster allocation explain API is to <b>provide explanations for shard allocations in the cluster</b>. For unassigned shards, the explain API provides an explanation for why the shard is unassigned. For assigned shards, the explain API provides an explanation for why the shard is remaining on its current node and has not moved or rebalanced to another node. This API can be very useful when attempting to diagnose why a shard is unassigned or why a shard continues to remain on its current node when you might expect otherwise."
</p>

