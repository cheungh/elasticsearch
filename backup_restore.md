## Example on Snapshot Backup and Restore

### Create test data  
download sample data from elasticsearch, look for the Account.zip  
`wget https://download.elastic.co/demos/kibana/gettingstarted/accounts.zip`  
`unzip account.zip`  
`curl -H 'Content-Type: application/x-ndjson' -XPOST 'localhost:9200/bank/account/_bulk?pretty' --data-binary @accounts.json`  

### make sure you have path.repo setup in your elasticsearch config
Please refer to the ES reference document

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
### Test our backup and restore process by deleting records and restoring the snapshot
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

### 2. Run restore snapshot
<pre>
curl -XPOST 'http://localhost:9200/_snapshot/my_backup/bank_2018_03_28/_restore?pretty' -H 'Content-Type: application/json' -d '{
	"indices":"bank",
	"ignore_unavailable": "true"
}'
</pre>
### 3. Open the index
<pre>
curl -XPOST 'localhost:9200/bank/_open?pretty'
</pre>
### 4. Test the result
You should see 1000 records if the restore process is successful.
<pre>
curl -XGET "http://localhost:9200/bank/_count" -H 'Content-Type: application/json' -d'
{
  "query": {
    "match_all": {}
  }
}'
</pre>

### Important!!! Never delete snapshot by other methods.

#### 1.Snapshot(s) only store delta exact for the first one
<p>Quoted from ES, "This process will take the current state and data in your cluster and save it to a shared repository. This backup process is "smart." Your first snapshot will be a complete copy of data, but all subsequent snapshots will save the delta between the existing snapshots and the new data. Data is incrementally added and deleted as you snapshot data over time. This means subsequent backups will be substantially faster since they are transmitting far less data."
</p>
#### 2. Never delete snapshot by other mechanism otherwise likely cause corrupt backup snapshot
<pre>
Quoted from Elasticsearch:
"It is important to use the API to delete snapshots, and not some other mechanism 
(such as deleting by hand, or using automated cleanup tools on S3). 
Because snapshots are incremental, it is possible that many snapshots are relying on old segments. 
The delete API understands what data is still in use 
by more recent snapshots, and will delete only unused segments.

If you do a manual file delete, 
however, you are at risk of seriously corrupting 
your backups because you are deleting data that is still in use."

</pre>
