### start up and shutdown and tail log
I installed ES via rpm
<pre>
systemctl stop elasticsearch.service && tail -f /var/log/elasticsearch/elasticsearch.log
systemctl start elasticsearch.service && tail -f /var/log/elasticsearch/elasticsearch.log
startup script located in (Centos7): /usr/lib/systemd/system/elasticsearch.service
</pre>

### process check for elasticsearch
<pre>
ps aux| grep elasticlastic

systemctl status elasticsearch.service
</pre>

### tools to monitor cpu utilitization, memory, swap
htop
top

### ES hot threads API
http://localhost:9200/_nodes/hot_threads

### ES Pending tasks API
http://localhost:9200/_cluster/pending_tasks

### check firewall rule
<pre>
some time firewalld block the ES communication zen discovery port 
firewall-cmd --zone=external --list-all  

Just run curl master_node_ip:9200 to see if you can connect to master
or stop firewall running
sudo systemctl stop firewalld
</pre>

### check log referred to ES config log4j2.properties
<pre>
1. check ES log for error
2. change log level to see more detail log4j2.properties
3. check slow log for trouble index
</pre>
<p>check my tuning tips at https://github.com/cheungh/elasticsearch/blob/master/Tuning.md</p>
### disk write speed check
delete the /tmp/tempfile afterward!
<pre>
sync; dd if=/dev/zero of=/tmp/tempfile bs=1M count=1024; sync
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 1.67315 s, 642 MB/s
</pre>

### disk read speed check
<pre>
dd if=/tmp/tempfile of=/dev/null bs=1M count=1024
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 0.675615 s, 1.6 GB/s
</pre>

### disk space check
<pre>
df -h
</pre>

### directory space check
<pre>
du --max-depth=1 -h
</pre>
