### 1. create a snapshot repo named as backup_1_0
<pre>
curl -XPUT 'localhost:9200/_snapshot/backup_1_0?pretty' -H 'Content-Type: application/json' -d'  
{  
  "type": "fs",  
  "settings": {  
    "location": "backup_1_0"  
  }  
} ' 
</pre>

### 2. check repo list
curl -XGET 'localhost:9200/_snapshot/?pretty'  

### 3. create a snapshot backup on index "bank" and stored in backup_1_0 repo
<pre>
curl -XPUT 'http://localhost:9200/_snapshot/backup_1_0/bank_2018_03_01?wait_for_completion=false' -H 'Content-Type: application/json' -d '{
	"indices": "bank",
	"ignore_unavailable": "true",
	"include_global_state": false
}'
</pre>

### 4. Check existing backup snapshots
<pre>
curl -XGET 'localhost:9200/_cat/snapshots/backup_1_0?v&s=id&pretty'

return:
id               status start_epoch start_time end_epoch  end_time duration indices successful_shards failed_shards total_shards
bank_2018_03_01 SUCCESS 1521952903  21:41:43   1521952903 21:41:43     75ms  
</pre>
### Test our backup by deleting records
count doc returns 1000 docs, and then delete record next on customers on state: PA  
Don't run this on PROD, the query below will delete data 
<pre>
curl -XGET "http://localhost:9200/bank/_count" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": {}
  }
}'

curl -XPOST "http://localhost:9200/bank/_delete_by_query" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_phrase": {
      "state": "PA"
    }
  }
}'

</pre>

### Restore Steps:
### 1. Close the index before restore the backup
<pre>
curl -XPOST 'localhost:9200/bank/_close?pretty'
</pre>

### 2. 

### 3. 
<pre>
curl -XPOST 'http://localhost:9200/_snapshot/my_backup/bank_2018_03_28/_restore?pretty' -H 'Content-Type: application/json' -d '{
	"indices":"bank",
	"ignore_unavailable": "true"
}'
<pre>
